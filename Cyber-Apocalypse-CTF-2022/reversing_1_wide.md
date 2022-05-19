# Reversing 1: WIDE
> We've received reports that Draeger has stashed a huge arsenal in the pocket dimension Flaggle Alpha. You've managed to smuggle a discarded access terminal to the Widely Inflated Dimension Editor from his headquarters, but the entry for the dimension has been encrypted. Can you make it inside and take control?


#### Difficulty: ★☆☆☆

We are given a zip file that contains `wide` and `db.ex`


Upon running `./wide` we are given the right way to execute it:

`Usage: ./wide db.ex`

After doing so, we are presented a menu:

```
[*] Welcome user: kr4eq4L2$12xb, to the Widely Inflated Dimension Editor [*]
[*]    Serving your pocket dimension storage needs since 14,012.5 B      [*]
[*]                       Displaying Dimensions....                      [*]
[*]       Name       |              Code                |   Encrypted    [*]
[X] Primus           | people breathe variety practice  |                [*]
[X] Cheagaz          | scene control river importance   |                [*]
[X] Byenoovia        | fighting cast it parallel        |                [*]
[X] Cloteprea        | facing motor unusual heavy       |                [*]
[X] Maraqa           | stomach motion sale valuable     |                [*]
[X] Aidor            | feathers stream sides gate       |                [*]
[X] Flaggle Alpha    | admin secret power hidden        |       *        [*]
```

I actually got stuck here because I thought I was supposed to type the name or code of the dimension.
After many unfruitful attempts, I discovered that I'm supposed to input numbers, as in 6 for Flaggle Alpha which seems to be the one we want.

```
[*] Welcome user: kr4eq4L2$12xb, to the Widely Inflated Dimension Editor [*]
[*]    Serving your pocket dimension storage needs since 14,012.5 B      [*]
[*]                       Displaying Dimensions....                      [*]
[*]       Name       |              Code                |   Encrypted    [*]
[X] Primus           | people breathe variety practice  |                [*]
[X] Cheagaz          | scene control river importance   |                [*]
[X] Byenoovia        | fighting cast it parallel        |                [*]
[X] Cloteprea        | facing motor unusual heavy       |                [*]
[X] Maraqa           | stomach motion sale valuable     |                [*]
[X] Aidor            | feathers stream sides gate       |                [*]
[X] Flaggle Alpha    | admin secret power hidden        |       *        [*]
Which dimension would you like to examine? 6
[X] That entry is encrypted - please enter your WIDE decryption key:

```

Now we need a decryption key, so I then checked what wide and db.ex had using `strings` but I did not see anything relevant.

Afterwards, I loaded up `wide` on [Online Disassembler](https://onlinedisassembler.com/) and clicked on the string that I got from entering a random decryption key:

`[X] That entry is encrypted - please enter your WIDE decryption key:`

Lo and behold, just below this section, spelled out with gaps of 3 00s per letter, is the decryption key: `sup3rs3cr3tw1d3`
Entering this decryption key gave me the flag

`HTB{str1ngs_4r3nt_4lw4ys_4sc11}`
