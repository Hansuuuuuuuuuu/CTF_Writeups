# Crypto 2: Small StEps
> "As you continue your journey, you must learn about the encryption method the aliens used to secure their communication from eavesdroppers. The engineering team has designed a challenge that emulates the exact parameters of the aliens' encryption system, complete with instructions and a code snippet to connect to a mock alien server. Your task is to break it."


#### Difficulty: very easy

We are given a zip file that contains 3 files: `README.md`, `server.py`, and `solver.py`. Using the solver, I was able to get 3 different values which correspond to elements of RSA:

```
N = 5741179304925454140087696087681952949454074055466311217154209598314674944116079016386818605645122907067599615459714447006278946908981336587710488439449749
e = 3
C = 70407336670535933819674104208890254240063781538460394662998902860952366439176467447947737680952277637330523818962104685553250402512989897886053
```

From what we can gather, the e is suspiciously small and the challenge's name corroborates this - it's a `small e attack`. I went to [dcode](https://www.dcode.fr/rsa-cipher) and provided the three values, which gave me the flag:

`HTB{5ma1l_E-xp0n3nt}`