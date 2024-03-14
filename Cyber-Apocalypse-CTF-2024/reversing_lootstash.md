reversing_lootstash
> A giant stash of powerful weapons and gear have been dropped into the arena - but there's one item you have in mind. Can you filter through the stack to get to the one thing you really need?

#### Difficulty: very easy

Downloading and extracting the zip file gives us a directory that contains a single binary `stash`. 
Running `strings stash` gives me quite a large output of strings, and just to quickly check, I run the output through `grep 'HTB'` and that gave me the flag

`HTB{n33dl3_1n_a_l00t_stack}`