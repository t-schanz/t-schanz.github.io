---
title: Creating a SSH config for Port Forwarding and Proxy Jumps
tags: [SSH, Linux]
style: 
color: primary
description: Tutorial on how to create a SSH config file that does easy port forwarding even with proxy jumps.
external_url:
---

I use SSH a lot to connect to remote machines. Most of the times I want to also forward a few ports to my local machine,
so that I can use Jupyter Notebooks or Tensorboard on the remote machine. 

In Linux and MacOS there is a file called `~/.ssh/config` that can be used to configure SSH. In this post I want to show
you how to use this file to make port forwarding easier. I will also show you how to use it to do port forwarding even
when you have to use a proxy jump to connect to the remote machine.

I assume you have already set up your SSH keys on your local and the remote machines.

## The SSH config file

```bash
ServerAliveInterval 30
ServerAliveCountMax 60
TCPKeepAlive yes
    
Host nickname
    Hostname address.of.remote.machine.com
    User your_username
    IdentityFile ~/.ssh/id_rsa
    LocalForward 8787 127.0.0.1:8787
    LocalForward 8880 127.0.0.1:8880
    LocalForward 8881 127.0.0.1:8881
    LocalForward 8882 127.0.0.1:8882
    LocalForward 8883 127.0.0.1:8883
    LocalForward 8884 127.0.0.1:8884
    LocalForward 8885 127.0.0.1:8885
    LocalForward 8886 127.0.0.1:8886
    LocalForward 8887 127.0.0.1:8887
    LocalForward 8889 127.0.0.1:8889
    LocalForward 6006 127.0.0.1:6006
    LocalForward 2222 127.0.0.1:22
    LocalForward 2023 127.0.0.1:2023
    ForwardAgent yes
```

You should now be able to connect to the remote machine by just typing `ssh nickname`. The `nickname` is the name you
gave the host in the config file. 

You see that I have a few settings that are not related to port forwarding. The first two lines are used to keep the
connection alive. The `TCPKeepAlive` setting is used to keep the connection alive even when there is no data transfered.
The `ServerAliveInterval` setting is used to send a keep alive signal every 30 seconds. The `ServerAliveCountMax` setting
is used to close the connection after 60 keep alive signals without a response. 

The `Host` section is used to define the settings for a specific host. You can give it a nickname, so that you don't have
to type the full address every time you want to connect to the remote machine. The `ForwardAgent` setting is used to
forward the ssh agent to the remote machine. This is useful when you want to connect to another machine from the remote
machine. The `LocalForward` settings are used to forward ports from the remote machine to your local machine. The syntax
is `LocalForward local_port remote_address:remote_port`. The `local_port` is the port on your local machine that you want
to forward to your local machine. The `remote_address` is the address of the remote machine. The `remote_port` is the
port on the remote machine that you want to forward to your local machine. Most of the time I use the same port on the
remote machine and on my local machine, but you can use different ports if you want to. Be aware, that you should not
just forward any port to your local machine, because it could be a security risk.

A special mention is the port 22 that I forward to port 2222 on my local machine. This port is used by the python 
interpreter. If you want to use a remote python interpreter in your IDE, you have to forward this port to your local
machine. You don't want to forward it to your local port 22 though, but to a different port to avoid conflicts with the
local python interpreter.

## Proxy Jump

If your remote machine is not directly accessible from the internet, you probably have to use a proxy jump to connect to
it, going over some kind of firewall or bastion host. In this case, you can use the `ProxyJump` setting to define the
proxy jump. The following example shows how to use it:

```bash
ServerAliveInterval 30
ServerAliveCountMax 60
TCPKeepAlive yes

Host bastion
    Hostname bastion.company.com
    User your_username
    IdentityFile ~/.ssh/id_rsa

Host frontend_any
    Hostname strand.fzg.local
    User your_username
    IdentityFile ~/.ssh/id_rsa

Host frontend_specific
    Hostname server_name_of_remote_machine_within_firewall
    User your_username
    ProxyJump bastion, frontend_any
    IdentityFile ~/.ssh/id_rsa
    LocalForward 8787 127.0.0.1:8787
    LocalForward 8880 127.0.0.1:8880
    LocalForward 8881 127.0.0.1:8881
    LocalForward 8882 127.0.0.1:8882
    LocalForward 8883 127.0.0.1:8883
    LocalForward 8884 127.0.0.1:8884
    LocalForward 8885 127.0.0.1:8885
    LocalForward 8886 127.0.0.1:8886
    LocalForward 8887 127.0.0.1:8887
    LocalForward 8889 127.0.0.1:8889
    LocalForward 6006 127.0.0.1:6006
    LocalForward 5555 127.0.0.1:5555
    LocalForward 2222 127.0.0.1:22
    LocalForward 2023 127.0.0.1:2023
    ForwardAgent yes
```

To connect to the remote machine, you can now just type `ssh frontend_specific`. 

In this example, I have a bastion host and multiple frontend hosts. Usually I want to end up on the same frontend host,
this is why I have an extra jump from `frontend_any` to `frontend_specific`.
