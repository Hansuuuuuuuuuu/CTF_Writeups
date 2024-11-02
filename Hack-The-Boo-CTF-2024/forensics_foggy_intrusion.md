# Forensics: Foggy Intrusion
> On a fog-covered Halloween night, a secure site experienced unauthorized access under the veil of darkness. With the world outside wrapped in silence, an intruder bypassed security protocols and manipulated sensitive areas, leaving behind traceable yet perplexing clues in the logs. Can you piece together the fragments of this nocturnal breach?

#### Difficulty: easy


We are given a zip file that contains a pcap. Opening the pcap using Wireshark gives us a bunch of TCP and HTTP entries. Scrolling through the window, I spotted a promising line that contained `POST /?%ADd+allow_url_include%3d1+%ADd+auto_prepend_file%3dphp://input HTTP/1.1`. I followed this stream, which ended up being `tcp.stream eq 3`, and saw the php payloads.

Decoding the payload shows us this:

```
powershell.exe -C "$output = whoami; $bytes = [Text.Encoding]::UTF8.GetBytes($output); $compressedStream = [System.IO.MemoryStream]::new(); $compressor = [System.IO.Compression.DeflateStream]::new($compressedStream, [System.IO.Compression.CompressionMode]::Compress); $compressor.Write($bytes, 0, $bytes.Length); $compressor.Close(); $compressedBytes = $compressedStream.ToArray(); [Convert]::ToBase64String($compressedBytes)"
```

Looks like the payload is being compressed using functions fromm System.IO.Compression on powershell, which means we can just decompress it using  `[System.IO.Compression.CompressionMode]::Decompress`.

After trying a few of the payloads, I stumbled upon the correct one: `dY9RS8MwFIX/ynUIyWDKZNkYTjdSW/DFKe3Ux0ttbligpjVtGTL2311a58bA+xIO37nnntwtynUJirSxxFkYYBLFb1HMBsDUB+vPTtHrni3lU9RBbCpyZ44XmSTvz3HoHY+rYKuHE1Q3Y1GWI+FGCoVVqHMxwY2oUA8bqy52ZxGhXMlAJu2RdBwsU6W9Ay4/v6uv3MA9WNpAJ/hf3wGc9GvFoUorDqE+yGjgv2FX86ywlrIaybnC9WELfpQh3nvoiCks6NTkpG6hB9fwz+YMdnBkFdWYrVO3fzlraj31P1jMfwA=` which gave me the flag:

`HTB{f06_d154pp34r3d_4nd_fl46_w4s_f0und!}`