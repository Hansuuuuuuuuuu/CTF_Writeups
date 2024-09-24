Too many Layers
> gkkab://bgrsktsq.vk/NWPVF  
Hey there, fellow code-cracker! Today, my brother, who’s fond of ciphers and steganography, sent me a mysterious link to a file filled with secrets! 🕵️‍♂️🗝️ He hinted that I need to use an algorithm but was delierious from all the attempts. I couldn't tell what code to use but it sounded like cAFFINE and the magical code word is “today.”  
Can you help me unravel this enigma and uncover the elusive flag hidden within? Let’s put our detective hats on and decrypt this together! 🔍✨

#### Points: 100

Based on the hints in the description, the url provided is encrypted using the Affine Cipher.
Using dcode.fr's Affine Decoder, we're able to get the decrypted url: `https://shorturl.at/CDIAE`.

Opening this URL gives us a drive link for an image of Voldemort named `steganography.io`. After adding a `.png` to this file, I was able to upload it to Aperi'Solve. Without a password, there's not much usable info to be had on the output though. Fortunately, we are given a hint about the password in the description - `today`, which can be interpreted as `0921` for September 21. 

Using `0921` as a password gets us the full content of the file, which I won't include here as it's quite long. The important part is:

`b2,rgb,lsb,yx .. zlib: data="I saved millions of lives when I created a machine to crack the the most important code of all time....", offset=63, size=211`

Googling this tells us that it's about Alan Turing, who is also the flag.


`FLAG{Alan Turing}`
