---
name: Live Quicklook Barbados Weather
tools: [Python, Matplotlib, NetCDF, Numpy, Pandas]
image: http://bcoweb.mpimet.mpg.de/quicklooks/LIVEQL/LIVE_QL.png
description: A live quicklook for the weather sitiuation at the Barbados Cloud Observatory with the most important
            parameters all in one view.
            <span style="color:#8a96a1">(2017)</span>
---

[![my image](http://bcoweb.mpimet.mpg.de/quicklooks/LIVEQL/LIVE_QL.png)](http://bcoweb.mpimet.mpg.de/quicklooks/LIVEQL/LIVE_QL.png)

# Live Quicklook Barbados Weather

This is a live quicklook for the weather sitiuation at the **Barbados Cloud Observatory** with the most important
parameters all in one view. It is used by the scientists at the Max-Planck-Institute for Meteorology to get quick
information about the current weather situation at the observatory. Some of the instruments are very sensitive and
can be damaged by extreme weather conditions. Therefore, it is important to have a quick overview of the current
weather situation.

I created this little tool during my time as a student helper at the **Max-Planck-Institute for Meteorology**. It is a 
python script that is executed every 5 minutes using a cronjob. It downloads the latest data from the FTP server of the
observatory and plots it using matplotlib. The resulting image is then uploaded to the FTP server of the institute.