---
description: >-
  Scanning or active enumeration is the phase where the attacker begins to touch
  the systems and possibly leaving some traces behind.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# Active

## <mark style="color:red;">The active recon chain is somehow pretty obvious:</mark>&#x20;

1\) You find the target (might have done this in the passive recon phase).

2\) Trace the route to target IP or network and try to map the network as best as you can.

3\) Search for open ports and services, if its a web app try using it and viewing the source code or use browser extensions that can give you some info about the technologies and apps behind it.

4\) Finally move on to the next phase (threat modeling or vulnerability assessment).

## <mark style="color:red;">Useful Resources</mark>

* [**Recon Everything**](https://infosecwriteups.com/recon-everything-48aafbb8987)&#x20;
* [**payload all the things**](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Network%20Discovery.md)&#x20;
* [**Hacking Tricks**](https://book.hacktricks.xyz/)&#x20;
* [**Nmap.org**](https://nmap.org/)&#x20;
* [**Nmap Cookbook**](https://b-ok.asia/book/3640353/cace51)

## <mark style="color:red;">Vulnerability Scanners</mark>

#### <mark style="color:purple;">Although using vulnerability scanners is not usual in advanced pentesting or red team engagements, It's useful to know about different vuln scanners out there.</mark>

* [**Nessus ( Commertial/Free Trial )**](https://www.tenable.com/products/nessus)
* [**Nexpose ( Commertial/Free Trial )**](https://www.rapid7.com/try/nexpose/)
* [**OpenVAS (Free)**](https://www.openvas.org/)
* [**Nmap NSE scripts (Free)**](https://nmap.org/book/man-nse.html)&#x20;
* [**Nikto (Free)**](https://github.com/sullo/nikto)&#x20;
* [**BurpSuite Scanner ( Free and commertial )**](https://portswigger.net/burp/documentation/desktop/getting-started/proxy-troubleshooting)&#x20;
* [**OWASP Zaproxy scanner (Free)**](https://www.zaproxy.org/)
* [**Acunetix ( Commertial/Free Trial )**](https://www.acunetix.com/)
* [**Netsparker ( Commertial/Free Trial )**](https://www.netsparker.com/)
