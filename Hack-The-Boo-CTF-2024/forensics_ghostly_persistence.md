# Forensics: Ghostly Persistence
> On a quiet Halloween night, when the world outside was wrapped in shadows, an intrusion alert pierced through the calm. The alert, triggered by an internal monitoring system, pinpointed unusual activity on a specific workstation. Can you illuminate the darkness and uncover what happened during this intrusion? Note: flag is split into two parts

#### Difficulty: easy

We are given a zip file that contains a folder of Windows Event Logs. Sorting by size, we can quickly spot the correct one as every other file is 68KB, which is the size of an empty `.evtx`. After opening the `Microsoft-Windows-PowerShell_Operational.evtx` log and scrolling to the chronological first event, we see that the user downloaded and executed a `.ps1` file.

The events afterward show the actual commands. The first command that contained the first half of the flag contained `$action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-EncodedCommand JHRlbXBQYXRoID0gIiRlbnY6d2luZGlyXHRlbXBcR2gwc3QudHh0IgoiSFRCe0doMHN0X0wwYzR0MTBuIiB8IE91dC1GaWxlIC1GaWxlUGF0aCAkdGVtcFBhdGggLUVuY29kaW5nIHV0Zjg="
` which then just needed to be decoded from base64.

After scrolling a few lines up, we see the second part of the hack and the payload contains the latter half of the flag `X1c0c19SM3YzNGwzZH0=`.

Combining and decoding gives us the flag:
`HTB{Gh0st_L0c4t10n_W4s_R3v34l3d}`