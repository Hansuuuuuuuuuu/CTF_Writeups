Makeshift
> Weak and starved, you struggle to plod on. Food is a commodity at this stage, but you can’t lose your alertness - to do so would spell death. You realise that to survive you will need a weapon, both to kill and to hunt, but the field is bare of stones. As you drop your body to the floor, something sharp sticks out of the undergrowth and into your thigh. As you grab a hold and pull it out, you realise it’s a long stick; not the finest of weapons, but once sharpened could be the difference between dying of hunger and dying with honour in combat.

#### Difficulty: very easy

Downloading and extracting the zip file gives us a directory that contains an `output.txt` and a `source.py`. `output.txt` looks to be a mangled version of the flag and a look at `source.py` confirms this.

To decode the flag, I just reversed the steps to make it:

```
old_flag = ''
new_flag = '!?}De!e3d_5n_nipaOw_3eTR3bt4{_THB'
new_flag = new_flag[::-1]

for i in range(0, len(new_flag), 3):
    old_flag += new_flag[i+1]
    old_flag += new_flag[i+2]
    old_flag += new_flag[i]

print(old_flag)
```

running this snippet gave me the flag:

`HTB{4_b3tTeR_w3apOn_i5_n3edeD!?!}`