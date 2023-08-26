---
description: Detecting any firewalls so we can find a way to bypass them.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# WAF Detection

#### WAF stands for Web Application Firewall.

#### &#x20;Its goal is to protect the website behind it by filtering/monitoring the traffic.

#### &#x20;Fingerprinting is a method used to gather information (about any WAF in this context).

## <mark style="color:red;">Tools</mark> <a href="#tools" id="tools"></a>

#### <mark style="color:purple;">Detecting WAFs with</mark> [WAFW00F](https://github.com/EnableSecurity/wafw00f)

```
wafw00f $URL
```

#### <mark style="color:purple;">Detecting WAFs with</mark> [WhatWaf](https://github.com/Ekultek/WhatWaf)

```
whatwaf -u $URL
```

#### <mark style="color:purple;">Detecting WAFs with</mark> [nmap](https://nmap.org)

```
nmap -p 80,443 --script=http-waf-fingerprint $URL
```

{% hint style="info" %}
Another script called `http-waf-detect`can be used. It detects IDS/IPS/WAF but doesn't give information about the vendor, or version...
{% endhint %}

## <mark style="color:red;">Other examples</mark> <a href="#other-examples" id="other-examples"></a>

#### <mark style="color:purple;">A manual testing workflow could be to check the cookies and response headers.</mark>

**Cookies**: some WAF can be identified by the cookie's name.&#x20;

**Response headers**: sometimes they are changed to apparently "confuse the attacker".

## <mark style="color:red;">Resources</mark> <a href="#resources" id="resources"></a>

{% embed url="https://nmap.org/nsedoc/scripts/http-waf-fingerprint.html" %}
