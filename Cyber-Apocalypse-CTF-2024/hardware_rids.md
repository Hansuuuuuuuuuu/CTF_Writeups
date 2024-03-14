Rids
> Upon reaching the factory door, you physically open the RFID lock and find a flash memory chip inside. The chip's package has the word W25Q128 written on it. Your task is to uncover the secret encryption keys stored within so the team can generate valid credentials to gain access to the facility.

#### Difficulty: easy

We are given a zip file and a Docker spawn. After extracting the zip file, we are given a `client.py` that lets us access data from the Docker instance. Since this is a Hardware challenge, W25Q128 is probably a real chip. After a quick search, I found the datasheet for this chip and scrolled to the Instruction Set Table. 
This table corresponds to what we should send using the `exchange()` function of the script.

```
# Example command
jedec_id = exchange([0x9F], 3)
print(jedec_id)
```
I ran the script with `0x03` as the first argument as this corresponds to `Read Data` (according to the table) and got `[72, 84, 66]`, which, when converted from ASCII, is `HTB`. I increased the bytes to receive (second argument) to `64`:
```
flag = exchange([0x03], 64)
flag_decoded=''.join(chr(byte) for byte in flag)
print(flag_decoded)
```

and received a response that contains the flag (and some extra characters):

`HTB{m3m02135_57023_53c2375_f02_3v32y0n3_70_533!@}`