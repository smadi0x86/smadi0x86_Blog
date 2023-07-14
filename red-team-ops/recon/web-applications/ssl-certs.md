---
description: SSL is used to encrypt communication and can be used in our report.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# SSL Certs

#### Since the SSL rating of the target organization website is valuable to the customers/partners.

Not really important as an attack vector but its something you should definitely include in your report.&#x20;

#### <mark style="color:purple;">Nmap script for enumerating SSL</mark>

```
nmap -p 443 --script=ssl-enum-ciphers target.com
```
