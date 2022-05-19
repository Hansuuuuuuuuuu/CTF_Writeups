# Reversing 2: Omega One
> You've been sent to the library planet Omega-One. Here, records from all over the galaxy are collected, sorted and archived with perfect efficiency. You need to retrieve records about Draeger's childhood, but the interface is impossibly large. Can you unravel the storage system?


#### Difficulty: ★☆☆☆

We are given a zip file that contains `output.txt` and `omega-one`.
Nothing happened after executing `./omega-one` so I checked the contents of output.txt:

<details><summary>output.txt</summary>
```
Crerceon
Ezains
Ummuh
Zonnu
Vinzo
Cuzads
Emoi
Ohols
Groz'ens
Ukox
Ehnu
Pheilons
Cuzads
Khehlan
Ohols
Ehnu
Munis
Inphas
Pheilons
Ehnu
Dut
Ukox
Ohols
Pheilons
Pheilons
Zimil
Ehnu
Honzor
Vinzo
Ukteils
Falnain
Dhohmu
Baadix
```
</details>

Seems like some alien words. I tried `strings` on omega-one and got a bigger set of alien words, but not much else.
I fired up IDA (free) and loaded `omega-one`. I looked for the main function and decompiled it (aka I pressed F5), which got me the character mappings per word:

<details><summary>Mappings</summary>
```
sub_1870(qword_203018, "k", "Lendrens");
sub_1870(qword_203018, "d", "Thauv'i");
sub_1870(qword_203018, "P", "Throrqiek");
sub_1870(qword_203018, "e", "Inqods");
sub_1870(qword_203018, "6", "Tarquts");
sub_1870(qword_203018, "p", "Dut");
sub_1870(qword_203018, "A", "Krolkel");
sub_1870(qword_203018, "n", "Emoi");
sub_1870(qword_203018, "|", "Dakroith");
sub_1870(qword_203018, "*", "Creiqex");
sub_1870(qword_203018, "Y", "Thomois");
sub_1870(qword_203018, "4", "Groz'ens");
sub_1870(qword_203018, "D", "Urqek");
sub_1870(qword_203018, "v", "Nid");
sub_1870(qword_203018, "H", "Crerceon");
sub_1870(qword_203018, "#", "Yonphie");
sub_1870(qword_203018, "S", "Xits");
sub_1870(qword_203018, "I", "Thohul");
sub_1870(qword_203018, "W", "Zahrull");
sub_1870(qword_203018, "i", "Om'ons");
sub_1870(qword_203018, "F", "Kradraks");
sub_1870(qword_203018, "+", "Ielkul");
sub_1870(qword_203018, "q", "Vranix");
sub_1870(qword_203018, "M", "Trun");
sub_1870(qword_203018, "h", "Craz'ails");
sub_1870(qword_203018, ".", "Xoq'an");
sub_1870(qword_203018, "r", "Ukox");
sub_1870(qword_203018, "N", "Evods");
sub_1870(qword_203018, ";", "Taxan");
sub_1870(qword_203018, "b", "Munis");
sub_1870(qword_203018, "g", "Trurkror");
sub_1870(qword_203018, "?", "Tulphaer");
sub_1870(qword_203018, "_", "Ehnu");
sub_1870(qword_203018, "$", "Krets");
sub_1870(qword_203018, ",", "Grons");
sub_1870(qword_203018, ")", "Ingell");
sub_1870(qword_203018, "(", "Ecruns");
sub_1870(qword_203018, "m", "Khehlan");
sub_1870(qword_203018, "R", "Velzaeth");
sub_1870(qword_203018, "Q", "Cuhix");
sub_1870(qword_203018, "l", "Vinzo");
sub_1870(qword_203018, "E", "Istrur");
sub_1870(qword_203018, ">", "Zuvas");
sub_1870(qword_203018, "s", "Honzor");
sub_1870(qword_203018, "0", "Ukteils");
sub_1870(qword_203018, "}", "Baadix");
sub_1870(qword_203018, "{", "Zonnu");
sub_1870(qword_203018, "\\", "Aarcets");
sub_1870(qword_203018, "[", "Nevell");
sub_1870(qword_203018, "!", "Dhohmu");
sub_1870(qword_203018, "X", "Xan");
sub_1870(qword_203018, "O", "Zissat");
sub_1870(qword_203018, "x", "Iscax");
sub_1870(qword_203018, "t", "Pheilons");
sub_1870(qword_203018, "`", "Ghiso");
sub_1870(qword_203018, "-", "Scrigvil");
sub_1870(qword_203018, "B", "Ummuh");
sub_1870(qword_203018, "u", "Inphas");
sub_1870(qword_203018, "/", "Vurqails");
sub_1870(qword_203018, "a", "Vruziels");
sub_1870(qword_203018, ":", "Ghut'ox");
sub_1870(qword_203018, "^", "Aahroill");
sub_1870(qword_203018, "L", "Gairqeik");
sub_1870(qword_203018, "U", "Qeks");
sub_1870(qword_203018, "'", "Scuvvils");
sub_1870(qword_203018, "3", "Ohols");
sub_1870(qword_203018, "5", "Som'ir");
sub_1870(qword_203018, "C", "Onzear");
sub_1870(qword_203018, "2", "Dhaesux");
sub_1870(qword_203018, "w", "Falnain");
sub_1870(qword_203018, " ", "Draalpho");
sub_1870(qword_203018, "G", "Yemor");
sub_1870(qword_203018, "c", "Thraurgok");
sub_1870(qword_203018, "\"", "Vogeath");
sub_1870(qword_203018, "1", "Cuzads");
sub_1870(qword_203018, "Z", "Gagro");
sub_1870(qword_203018, "=", "Zad");
sub_1870(qword_203018, "f", "Dhieqe");
sub_1870(qword_203018, "&", "Xustrek");
sub_1870(qword_203018, "o", "Harned");
sub_1870(qword_203018, "V", "Dhulgea");
sub_1870(qword_203018, "y", "Zimil");
sub_1870(qword_203018, "z", "Thretex");
sub_1870(qword_203018, "8", "Bravon");
sub_1870(qword_203018, "%", "Krugreall");
sub_1870(qword_203018, "J", "Vaendred");
sub_1870(qword_203018, "@", "Osux");
sub_1870(qword_203018, "T", "Ezains");
sub_1870(qword_203018, "K", "Mik'ed");
sub_1870(qword_203018, "<", "Cruz'oll");
sub_1870(qword_203018, "]", "Dhognot");
sub_1870(qword_203018, "7", "Drids");
sub_1870(qword_203018, "9", "Drercieks");
sub_1870(qword_203018, "j", "Statars");
```
</details>

I then threw a quick python script together (with some finnicky entries removed) to decrypt the output
<details><summary>script</summary>
```
mapping = {"Lendrens":"k","Thauv'i":"d","Throrqiek":"P","Inqods":"e","Tarquts":"6","Dut":"p","Krolkel":"A","Emoi":"n","Dakroith":"|","Creiqex":"*","Thomois":"Y","Groz'ens":"4","Urqek":"D","Nid":"v","Crerceon":"H","Yonphie":"#","Xits":"S","Thohul":"I","Zahrull":"W","Om'ons":"i","Kradraks":"F","Ielkul":"+","Vranix":"q","Trun":"M","Craz'ails":"h","Xoq'an":".","Ukox":"r","Evods":"N","Taxan":";","Munis":"b","Trurkror":"g","Tulphaer":"?","Ehnu":"_","Krets":"$","Grons":",","Ingell":")","Ecruns":"(","Khehlan":"m","Velzaeth":"R","Cuhix":"Q","Vinzo":"l","Istrur":"E","Zuvas":">","Honzor":"s","Ukteils":"0","Baadix":"}","Zonnu":"{", "Aarcets":"\\","Nevell":"[","Dhohmu":"!","Xan":"X","Zissat":"O","Iscax":"x","Pheilons":"t","Ghiso":"`","Scrigvil":"-","Ummuh":"B","Inphas":"u","Vurqails":"/","Vruziels":"a","Ghut'ox":":","Aahroill":"^","Gairqeik":"L","Qeks":"U","Scuvvils":"'","Ohols":"3","Som'ir":"5","Onzear":"C","Dhaesux":"2","Falnain":"w","Draalpho":" ","Yemor":"G","Thraurgok":"c","Cuzads":"1","Gagro":"Z","Zad":"=","Dhieqe":"f","Xustrek":"&","Harned":"o","Dhulgea":"V","Zimil":"y","Thretex":"z","Bravon":"8","Krugreall":"%","Vaendred":"J","Osux":"@","Ezains":"T","Mik'ed":"K","Cruz'oll":"<","Dhognot":"]","Drids":"7","Drercieks":"9","Statars":"j"}

input_list = ['Crerceon','Ezains','Ummuh','Zonnu','Vinzo','Cuzads','Emoi','Ohols','Groz\'ens','Ukox','Ehnu','Pheilons','Cuzads','Khehlan','Ohols','Ehnu','Munis','Inphas','Pheilons','Ehnu','Dut','Ukox','Ohols','Pheilons','Pheilons','Zimil','Ehnu','Honzor','Vinzo','Ukteils','Falnain','Dhohmu','Baadix']

for i in input_list:
    print(mapping[i], end='')
```
</details>

which gave me the flag

`HTB{l1n34r_t1m3_but_pr3tty_sl0w!}`
