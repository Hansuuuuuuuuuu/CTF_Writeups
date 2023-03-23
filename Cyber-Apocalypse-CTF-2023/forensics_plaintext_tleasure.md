# Forensics 1: Plaintext Tleasure
> "Threat intelligence has found that the aliens operate through a command and control server hosted on their infrastructure. Pandora managed to penetrate their defenses and have access to their internal network. Because their server uses HTTP, Pandora captured the network traffic to steal the server's administrator credentials. Open the provided file using Wireshark, and locate the username and password of the admin."


#### Difficulty: very easy

We are given a zip file which contains `capture.pcap`. Opening it with Wireshark shows us about 960 lines.

Judging from the spiel of the challenge, I figured that I should be able to just export an object via Wireshark.
Using `File -> Export Objects -> HTTP`, I was able to spot a `token.txt` whose file type was `multipart/form-data`, which I then exported. Opening the exported file gave me the flag:

`HTB{th3s3_4l13ns_st1ll_us3_HTTP}`
