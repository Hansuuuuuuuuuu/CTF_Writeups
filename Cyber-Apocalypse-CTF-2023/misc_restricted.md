misc_3_restricted

# Misc 3: Restricted
> You 're still trying to collect information for your research on the alien relic. Scientists contained the memories of ancient egyptian mummies into small chips, where they could store and replay them at will. Many of these mummies were part of the battle against the aliens and you suspect their memories may reveal hints to the location of the relic and the underground vessels. You managed to get your hands on one of these chips but after you connected to it, any attempt to access its internal data proved futile. The software containing all these memories seems to be running on a restricted environment which limits your access. Can you find a way to escape the restricted environment ?



#### Difficulty: easy

We are given a zip file that contains several files, notably: a `src` directory, a `Dockerfile` and `build_docker.sh`. Reading through the contents of the `src` folder, it seems like the usual functions have been rendered unusable by changing PATH to `$HOME/.bin`. 

The `sshd_config` revealed the weak point: the server can be ssh'd to passwordlessly by the user named `restricted`:

```
Match user restricted
    PermitEmptyPasswords yes
```
I then connected to the docker instance spawned for the challenge:
`ssh restricted@68.183.37.122 -p 31639`

But as mentioned, I was unable to use `ls`, `cat` and other commands - though I was able to get the name of the flag file by tabbing through the contents of /. 
I realized that I can just scp the said flag file to my local machine:
`scp -P 31639 restricted@68.183.37.122:/flag_8dpsy .`

which gave me the flag:

`HTB{r35tr1ct10n5_4r3_p0w3r1355}`
