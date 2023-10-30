---
title: Two Python Async Use Cases
tags: [Python, asyncio, ]
style: fill
color: info
description: Demo on (a) how asycio can be used to download many small files and (b) how they can be extracted.
external_url:
---

Especially in the field of Earth System Sciences it is very common to pack data into many small files. This is often done
to save space and make it possible to download only the data that is needed. However, this can make it very cumbersome to
download and extract the data. In this post I will show you how you can use Python's asyncio module to download and extract
many small files asynchronously. As an example I will use radar data from the [German Weather Service](https://www.dwd.de).
The data can be accessed via the [Open Data Portal](https://opendata.dwd.de/)

This is not going to be a tutorial on how to use asyncio, just a showcase and a starting point for your own projects.

## Downloading the data

```python

"""
This script is for downloading data in an asynchronous way (recommended if you have to download a lot of
files).
Usage: define the addresses/files to download in the protected __main__ part so that `url_list` contains
        only complete http-paths to existing files.
Example:
    ```python
    OUTPUT_PATH = "."
    url_list = ["https://opendata.dwd.de/climate_environment/CDC/grids_germany/5_minutes/radolan/reproc/2017_002/bin/" \
               "2002/YW2017.002_200201.tar",
               "https://opendata.dwd.de/climate_environment/CDC/grids_germany/5_minutes/radolan/reproc/2017_002/bin/" \
               "2002/YW2017.002_200202.tar"]
    main(urls=url_list)
    ```
"""

import asyncio
import os

import aiohttp
import numpy as np


async def download_and_save_file(session, url, chunk_size=10, overwrite=True):
    # print("downloading: ", url)
    async with session.get(url) as response:
        assert response.status == 200
        file_name = os.path.split(url)[-1]
        if not os.path.isfile(os.path.join(OUTPUT_PATH, file_name)) or overwrite:
            with open(os.path.join(OUTPUT_PATH, file_name), 'wb') as fd:
                while True:
                    chunk = await response.content.read(chunk_size)
                    if not chunk:
                        break
                    fd.write(chunk)
            print("finished: ", file_name)
        else:
            print(f"{file_name} already exists and therefore is skipped")


async def gather_with_concurrency(n, *tasks):
    semaphore = asyncio.Semaphore(n)

    async def sem_task(task):
        async with semaphore:
            return await task
    return await asyncio.gather(*(sem_task(task) for task in tasks))


async def download_multiple(urls: list):
    timeout = aiohttp.ClientTimeout(total=6000)
    async with aiohttp.ClientSession(timeout=timeout) as session:
        coroutines = [download_and_save_file(session=session, url=url) for url in urls]
        await gather_with_concurrency(5, *coroutines)


def main(urls):
    loop = asyncio.get_event_loop()
    loop.run_until_complete(download_multiple(urls))
    print('finished')


if __name__ == "__main__":
    # # to download radar data:
    OUTPUT_PATH = "./radar_data"
    BASE_URL = "https://opendata.dwd.de/climate_environment/CDC/grids_germany/5_minutes/radolan/reproc/2017_002/bin/" \
               "{year}/YW2017.002_{year}{month}.tar"

    url_list = []
    for year in np.arange(2001, 2020, 1):
        for month in np.arange(1, 13, 1):
            month_str = f"{month:02d}"
            url_list.append(BASE_URL.format(year=str(year), month=str(month_str)))

    main(urls=url_list)
```

## Extracting the data

```python
import asyncio
import glob
import os
import tarfile
from tqdm import tqdm


def extract(input_file, output_path, pbar=None):
    tar = tarfile.open(input_file)
    tar.extractall(path=output_path)
    tar.close()
    if pbar:
        pbar.update(1)


async def custom_system_command(command, pbar=None):
    command_list = command
    proc = await asyncio.create_subprocess_exec(*command_list)
    returncode = await proc.wait()
    if pbar:
        pbar.update(1)

    file = command[2]
    os.remove(file)

    return returncode


async def gather_with_concurrency(n, *tasks):
    semaphore = asyncio.Semaphore(n)

    async def sem_task(task):
        async with semaphore:
            return await task

    return await asyncio.gather(*(sem_task(task) for task in tasks))


async def prepare_system_command(commands):
    coroutines = [custom_system_command(command) for command in commands]
    await gather_with_concurrency(8, *coroutines)


def main(file: str, output_base_path: str):
    if not os.path.exists(output_base_path):
        os.makedirs(output_base_path)

    output_path = os.path.join(output_base_path, os.path.splitext(os.path.basename(file))[0])
    extract(file, output_path)
    print(f"Extracted base of {file}")
    files = glob.glob(os.path.join(output_path, "*"))

    commands = []
    for file in sorted(files):
        commands.append(["tar", "-xzf", file, "-C", output_path])

    loop = asyncio.get_event_loop()
    loop.run_until_complete(prepare_system_command(commands))


if __name__ == "__main__":
    OUTPUT_BASE_PATH = "/output"
    INPUT_DATA = "/tar_data/*.tar"

    files = glob.glob(INPUT_DATA)
    for file in tqdm(files):
        main(file, OUTPUT_BASE_PATH)
```