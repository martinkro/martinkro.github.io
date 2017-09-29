---
layout: post
title: How build shadowsocks on ubuntu
date: 2016-11-18 17:49:19 +0800
---

### Install shadowsocks

```powershell
apt-get update
apt-get install python-pip
pip install shadowsocks
```

### Create Configuration File

Path:/etc/shadowsocks.json

```Json
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

### Run shadowsocks

```powershell
ssserver -c /etc/shadowsocks.json -d start
```

Save above command in /etc/rc.local