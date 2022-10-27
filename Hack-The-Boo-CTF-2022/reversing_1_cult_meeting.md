# Reversing 1: Cult Meeting
> "I told them it was too soon and in the wrong season to deploy such a website, but they assured me that theming it properly would be enough to stop the ghosts from haunting us. I was wrong." Now there is an internal breach in the `Spooky Network` and you need to find out what happened. Analyze the the network traffic and find how the scary ghosts got in and what they did.


#### Difficulty: easy

We are given a zip file that contains `meeting`. Checking it with `file` shows:
`meeting: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=72d8b06e4ca750d5c24395d3349c3121b9b95283, for GNU/Linux 3.2.0, not stripped`

I used nc to connect to the docker instance that was spawned for the challenge. I received this prompt:

```
You knock on the door and a panel slides back
|/👁️ 👁️ \|   A hooded figure looks out at you
"What is the password for this week's meeting?"
```

I loaded up the binary on IDA, and on `IDA View-A` , I double clicked on `lea     rdi, aWhatIsThePassw ; "\"What is the password for this week's "...`
Just below this line, the password is displayed: `'sup3r_s3cr3t_p455w0rd_f0r_u!'`

I entered this password on the prompt, which then gave me access to a shell. `ls` shows that there is a `flag.txt`, which I viewed using `cat`, giving me the flag:

```
The panel slides closed and the lock clicks
|      | "Welcome inside..."
/bin/sh: 0: can't access tty; job control turned off
$ ls -lrth
ls -lrth
total 24K
-rw-r--r-- 1 root root  36 Oct 17 13:53 flag.txt
-rwxr-xr-x 1 root root 17K Oct 17 13:55 meeting
$ cat flag.txt
cat flag.txt
HTB{1nf1ltr4t1ng_4_cul7_0f_str1ng5}
```
