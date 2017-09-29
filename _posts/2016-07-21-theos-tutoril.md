---
layout: post
title: "Theos tutoril"
date: 2016-07-21 17:49:19 +0800
description: ""
category: 
tags: []
---

### Install theos
<pre>
git clone https://github.com/coolstar/theos.git

// delete \r 
perl -i -pe 'y|\r||d' dpkg-deb
</pre>

### Package
<pre>
make package messages=yes
</pre>

### Install
<pre>
THEOS_DEVICE_IP = 192.168.1.110
make package install
</pre>


### SSH login without password

#### Your aim
You want to use Linux and OpenSSH to automate your tasks. Therefore you need an automatic login from host A / user a to Host B / user b. You don't want to enter any passwords, because you want to call ssh from a within a shell script.

#### How to do it

First log in on A as user a and generate a pair of authentication keys. Do not enter a passphrase:
<pre>
a@A:~> ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/a/.ssh/id_rsa): 
Created directory '/home/a/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/a/.ssh/id_rsa.
Your public key has been saved in /home/a/.ssh/id_rsa.pub.
The key fingerprint is:
3e:4f:05:79:3a:9f:96:7c:3b:ad:e9:58:37:bc:37:e4 a@A
</pre>

Now use ssh to create a directory ~/.ssh as user b on B. (The directory may already exist, which is fine):
<pre>
a@A:~> ssh b@B mkdir -p .ssh
b@B's password: 
</pre>

Finally append a's new public key to b@B:.ssh/authorized_keys and enter b's password one last time:
<pre>
a@A:~> cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'
b@B's password:
</pre>