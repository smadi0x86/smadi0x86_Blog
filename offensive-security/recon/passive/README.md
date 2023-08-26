---
description: >-
  OSINT (short for Open-Source Intelligence Gathering) is a way of knowing your
  target without any sorts of direct contact or leaving any evidence of the
  recon.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# Passive

## <mark style="color:red;">The OSINT Process</mark>

### <mark style="color:yellow;">OSINT reconnaissance can be further broken down into the following 5 Phases:</mark>

<figure><img src="../../../.gitbook/assets/image (24).png" alt="" width="479"><figcaption></figcaption></figure>

<mark style="color:purple;">**Source Identification**</mark><mark style="color:purple;">:</mark> As the starting point, in this initial phase the attacker identifies potential sources from which information may be gathered from. Sources are internally documented throughout the process in detailed notes to come back to later if necessary.

<mark style="color:purple;">**Data Harvesting**</mark><mark style="color:purple;">:</mark> In this phase, the attacker collects and harvests information from the selected sources and other sources that are discovered throughout this phase.

<mark style="color:purple;">**Data Processing and Integration**</mark><mark style="color:purple;">:</mark> During this phase, the attacker processes the harvested information for actionable intelligence by searching for information that may assist in enumeration.

<mark style="color:purple;">**Data Analysis**</mark><mark style="color:purple;">:</mark> In this phase, the attacker performs data analysis of the processed information using OSINT analysis tools.

<mark style="color:purple;">**Results Delivery**</mark><mark style="color:purple;">:</mark> In the final phase, OSINT analysis is complete and the findings are presented/reported to other members of the Red Team.

{% hint style="info" %}
**Note:** In OSINT you should always ask questions like: how, who, when, where and why. Also try to collect and sort everything you find and make a structured map of the intel you have gathered using a mind mapping tool like [XMind](https://www.xmind.net) or [Mind Master](https://www.mindmaster.io).
{% endhint %}

## <mark style="color:red;">Workflow of OSINT Cheatsheet</mark>

<figure><img src="../../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (59).png" alt="" width="488"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (46).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (54).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (48).png" alt="" width="563"><figcaption></figcaption></figure>

## <mark style="color:red;">DNS Harvesting</mark>

{% embed url="https://dnsdumpster.com/" %}

{% embed url="https://dnschecker.org/all-dns-records-of-domain.php" %}

## <mark style="color:red;">Email Harvesting</mark>

{% embed url="https://www.twilio.com/docs/glossary/email-harvesting" %}

{% embed url="https://hunter.io/" %}

{% embed url="https://www.nymeria.io/blog/how-to-manually-find-email-addresses-for-github-users" %}

## <mark style="color:red;">GitHub Dorks</mark>

**We can use GitHub advanced search keywords and dorks to find sensitive data in repositories.**

{% hint style="info" %}
GitHub dorks work with filenames and extensions
{% endhint %}

```
filename:bashrc
extension:pem
langage:bash
```

**Some examples of GitHub search keywords:**

```
extension:pem private # Private SSH Keys
extension:sql mysql dump # MySQL dumps
extension:sql mysql dump password # MySQL dumps with passwords
filename:wp-config.php # Wordpress config file
filename:.htpasswd # .htpasswd
filename:.git-credentials # Git stored credentials
filename:.bashrc password # .bashrc files containing passwords
filename:.bash_profile aws # AWS keys in .bash_profiles
extension:json mongolab.com # Keys/Credentials for mongolab
HEROKU_API_KEY language:json # Heroku API Keys
filename:filezilla.xml Pass # FTP credentials
filename:recentservers.xml Pass # FTP credentials
filename:config.php dbpasswd # PHP Applications databases credentials
shodan_api_key language:python # Shodan API Keys (try others languages)
filename:logins.json # Firefox saved password collection (key3.db usually in same repo)
filename:settings.py SECRET_KEY # Django secret keys (usually allows for session hijacking, RCE, etc)
```

## <mark style="color:red;">Open Job Requisitions</mark>

### <mark style="color:purple;">Job requisitions can help us get information about the information technology products used in a target organization, such as:</mark>

* Web Server Type
* Web Application Development Environment
* Firewall Type
* Routers

### <mark style="color:purple;">Google searches to find job requisitions</mark>

* site: \[ companydomain ] careers Q , Keyword or Title 9
* site: \[ companydomain ] jobs .
* site: \[ companydomain ] openings
* Also, searches of job-related sites such as LinkedIn

## <mark style="color:red;">PGP Public Key Servers</mark>

**Organizations maintain servers that provide public PGP keys to clients. You can query these to reveal user email addresses and details.**

{% embed url="https://keyserver.ubuntu.com/" %}

{% embed url="https://pgp.mit.edu/" %}

## <mark style="color:red;">Cloudflare / Tor IP Detection</mark>

**Some tips to find real IP addresses hiding behind Cloudflare and Tor**

{% embed url="https://www.secjuice.com/finding-real-ips-of-origin-servers-behind-cloudflare-or-tor/" %}

## <mark style="color:red;">Identify Host Sharing</mark>

**See if a single server or ip is hosting multiple websites/domains:**

```
# Bing dorks to identify host sharing
ip:xxx.xxx.xxx.xxx
```

## <mark style="color:red;">Shodan</mark>

**Search engine for the Internet of everything.**&#x20;

Shodan is the world's first search engine for Internet-connected devices including computers, servers, CCTV cameras, SCADA systems and everything that is connected to the internet with or without attention.&#x20;

Shodan can be used both as a source for gathering info about random targets for mass attacks and a tool for finding weak spots in a large network of systems to attack and take the low-hanging fruit.&#x20;

Shodan has a free and commercial membership and is accessible at [shodan.io](https://www.shodan.io).&#x20;

The search syntax in the search engine is somehow special and can be found in the help section of the website.&#x20;

With Shodan you can search for specific systems, ports, services, regions and countries or even specific vulnerable versions of a software or OS service running on systems like SMB v1 and much more.

**Here the keywords that are mostly used in Shodan search queries:**

{% embed url="https://www.osintme.com/index.php/2021/01/16/ultimate-osint-with-shodan-100-great-shodan-queries" %}

## <mark style="color:red;">Other OSINT Websites</mark>

**I have put together a list of the most used OSINT sources that will usually cover about 90% of your needs in a regular pentest.**&#x20;

**Remember there are endless ways to find Intel about your target.**&#x20;

**The OSINT process is limited to your own imagination.**

### <mark style="color:purple;">Top sources ( most used )</mark>

* #### [Skip Tracing Framework (kind of all-in-one directory for recon)](https://makensi.es/stf/)
* #### [Robtex (search for IPs, domain names, etc )](https://www.robtex.com)
* #### &#x20;[Netcraft (very useful for website and domain recon)](https://searchdns.netcraft.com)&#x20;
* #### [SSL labs (test websites and domains SSL cert security)](https://www.ssllabs.com/ssltest)&#x20;
* #### [Security Headers (test website headers (browser plugin is available)](https://securityheaders.com)&#x20;
* #### [Archive.org (the largest Internet archive)](https://archive.org)&#x20;
* #### [iseek (not as deep as others but still useful)](https://www.iseek.com)&#x20;
* #### [Global file search (search for any file, used for passive metadata search )](http://globalfilesearch.com)&#x20;
* #### [NSLookup (query DNS records, both web and CLI tool )](https://network-tools.com/nslookup/)&#x20;
* #### [DNSdumpster (great for DNS recon)](https://dnsdumpster.com)&#x20;
* #### [Whois (both web and CLI tool )](https://www.whois.net)&#x20;
* #### [ONYPHE (internet SIEM website, that's what they call themselves )](https://www.onyphe.io)

### <mark style="color:purple;">Image search</mark>

* [**TinEye ( reverse image search )**](https://tineye.com)&#x20;
* #### [photo bucket ( image search )](https://photobucket.com)

### <mark style="color:purple;">Username and people search</mark>

* [**User search ( search for usernames, mostly social media networks )**](https://usersearch.org)&#x20;
* [**pipi ( investigation and research, you should sign up for it )**](https://pipl.com)&#x20;
* [**Social mention ( social media search )**](http://socialmention.com)&#x20;
* [**Social searcher ( social media search )**](https://www.social-searcher.com)&#x20;
* [**SPOKEO ( name, phone number, address, etc. )**](https://www.spokeo.com)&#x20;
* [**Find people search ( people search )**](http://www.findpeoplesearch.com)&#x20;

### <mark style="color:purple;">IOT and device search</mark>

* [**shodan ( search engine for internet connected devices, command line )**](https://www.shodan.io)
* &#x20;[**open stream cam ( open stream camera )**](file:///root/work/w4lk3rn3t/recon/osint/index.html)&#x20;
* [**insecam ( live video camera search )**](file:///root/work/w4lk3rn3t/recon/osint/index.html)

### <mark style="color:purple;">Dark web engines</mark>

pubpeer.com

scholar.google.com

arxiv.org

guides.library.harvard.edu

deepdotweb.com

Core.onion

OnionScan

Tor Scan

ahmia.fi

not evil

### <mark style="color:purple;">Monitoring and alerting</mark>

Google Alerts

HaveIBeenPwned.com

Dehashed.com

Spycloud.com

Ghostproject.fr

weleakinfo.com/

### <mark style="color:purple;">social-analyzer</mark>

**For analyzing and finding a person's profile across 800+ social media websites**

```
python3 -m pip install social-analyzer

social-analyzer --username "johndoe" --metadata --extract --mode fast
```

### <mark style="color:purple;">TWINT</mark>

**Advanced Twitter scraping tool written in Python that allows for scraping Tweets from Twitter profiles without using Twitter's API.**

{% embed url="https://github.com/twintproject/twint" %}
