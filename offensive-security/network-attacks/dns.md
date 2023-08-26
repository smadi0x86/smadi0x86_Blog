---
description: Running on port 53 TCP/UDP
---

# DNS

<figure><img src="https://798372259-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FLHD0RLfJGvAbxEUqHSRx%2Fuploads%2FkPHRoB068GGO9p8AeiiD%2Fimage%20(277)%20(1)%20(1)%20(1)%20(1)%20(1).png?alt=media" alt=""><figcaption></figcaption></figure>

**​**[**Domain Name Service**](https://tools.ietf.org/html/rfc1035)**​**​

#### <mark style="color:purple;">TCP port 53 by default, fall back to UDP port 53 if not possible.</mark>

## <mark style="color:red;">Quick Check</mark>

```
whois domain.com
dig {a|txt|ns|mx} domain.com
dig {a|txt|ns|mx} domain.com @ns1.domain.com
host -t {a|txt|ns|mx} megacorpone.com
host -a megacorpone.com
host -l megacorpone.com ns1.megacorpone.com
dnsrecon -d megacorpone.com -t axfr @ns2.megacorpone.com
dnsenum domain.com
nslookup -> set type=any -> ls -d domain.com
for sub in $(cat subdomains.txt);do host $sub.domain.com|grep "has.address";done
```

## <mark style="color:red;">DNS Enumeration</mark>

### <mark style="color:yellow;">Nslookup</mark>

#### <mark style="color:purple;">Run in interactive mode:</mark>

```
>set type=a >>> set record type A (ipv4)
> set type=ns >>> set record type NS (name server)
> server [domain] >>> find the default erver for a domain
> set type=mx >>> find mail exchanger of a domain
> set type=CNAME >>> find the cannonical name of the domain
```

### <mark style="color:yellow;">Dig</mark>

#### <mark style="color:purple;">Mostly used to perform a zone transfer.</mark>

```
dig @[dns server] [domain]
dig -x [ip] >>> reverse lookup
dig axfr cronos.htb @[ip]
dig sec542.org -t any
```

### <mark style="color:yellow;">Fierce</mark>

```
fierce -dns [domain]

fierce -dns [domain] -wordlist [file.txt] >>> dns brute force

fierce -dns example.com -wordlist /root/tools/wordlist/SecLists/Discovery/DNS/fierce-hostlist.txt 
```

### <mark style="color:yellow;">Nmap</mark>

```
nmap -p 53 --script dns-brute zonetransfer.me
nmap -p 53 --script dns-brute zonetransfer.me --script-args=dns-brute.hostlist=[wordlist path]
```

### <mark style="color:yellow;">Dnsrecon</mark>

* `dnsrecon -d [domain]` - Displays S0A, NS, A , AAAA , MX, and SRV of the target domain
* `dnsrecon -d [domain] -t rvl` - Performs reverse DNS lookup for IP address or CIDR range
* `dnsrecon -d [domain] -t axfr` - Attempts a zone transfer of all NS record nameservers
* dn`srecon -d [domain] -t zonewalk` - Performs a DNSSEC zone walk by querying for NSEC records
* `dnsrecon -d [domain] -t snoop` - D \[dictionary file] - Scans for DNS cache snooping using a supplied dictionary file

#### <mark style="color:purple;">DNSRecon can also perform subdomain brute forcing with a dictionary using the following command:</mark>

• dnsrecon -d \[domain ] -t brt - D \[dictionary file]

#### <mark style="color:purple;">Finally, DNSRecon can output the returned data to an XML file using the — xml \[output file] flag or to an SQLite database using the db \[output file] flag.</mark>

```
# examples:

dnsrecon -d [domain] -n [name server if known] -r [range-optional] -t axfr
dnsrecon -d example.com -t axfr
dnsrecon -d example.com -D ~/list.txt -t brt >>> list contains prefixes for domain
dnsrecon -t brt -d example.com -n 127.0.0.1 -D wordlist.txt
```

### <mark style="color:yellow;">Dnsmap</mark>

```
dnsmap zonetransfer.me -w /root/tools/wordlist/SecLists/Discovery/DNS/dns-Jhaddix.txt
```

### <mark style="color:yellow;">Host</mark>

```
host -t ns zonetransfer.me
-t mx >>> mail server
-l [website] [domain name] >>> look for zone transfer
```

### <mark style="color:yellow;">Dnsenum</mark>

```
dnsenum [domain or site]
dnsenum example.com
```

### <mark style="color:yellow;">Identifying private addresses by using dig</mark>

```
dig @ns3.isc-sns.info -f /tmp/paypal.txt +noall +answer | awk '{printf("%s %s\n",$5,$1);}' | grep -E '^(10\.)'
```

### <mark style="color:yellow;">Bash zone transfer</mark>

#### <mark style="color:purple;">Here is a simple bash script which performs a zone transfer:</mark>

```
#!/bin/bash
if [ -z "$1" ]; then
echo "[*] Simple Zone transfer script [*]"
echo "Usage : $0 [domain name] "
exit 0
fi
for server in $(host -t ns $1 | cut -d " " -f4); do
host -l $1 $server |grep "has address"
done
```

### <mark style="color:yellow;">IPv6</mark>

#### <mark style="color:purple;">Brute force using "AAAA" requests to gather IPv6 of the subdomains.</mark>

```
dnsdict6 -s -t <domain>
```

#### <mark style="color:purple;">Bruteforce reverse DNS in usi ng IPv6 addresses.</mark>

```
dnsrevenum6 pri.authdns.ripe.net 2001:67c:2e8::/48 #Will use the dns pri.authdns.ripe.net
```

## <mark style="color:red;">NSEC/NSEC3</mark>

#### <mark style="color:purple;">You can quiz name servers supporting DNSSEC to reveal valid hostnames.</mark>&#x20;

#### <mark style="color:purple;">Scripts that automate this are dns-nsec-enum and dns-nsec3-enum.</mark>

```
nmap -sSU -p53 --script dns-nsec-enum --script-args dns-nsec-enum.domains=paypal.com ns3.isc-sns.info
```

## <mark style="color:red;">DNS Recursion DDoS</mark>

#### <mark style="color:purple;">If</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**DNS recursion is enabled**</mark><mark style="color:purple;">, an attacker could</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**spoof**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**origin**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">on the UDP packet in order to make the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**DNS send the response to the victim server**</mark><mark style="color:purple;">.</mark>&#x20;

#### <mark style="color:purple;">An attacker could abuse</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**ANY**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">or</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**DNSSEC**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">record types as they use to have the bigger responses.</mark>

#### <mark style="color:purple;">The way to</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**check**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">if a DNS supports</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**recursion**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">is to query a domain name and</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**check**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">if the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**flag "ra"**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">(</mark>_<mark style="color:purple;">recursion available</mark>_<mark style="color:purple;">) is in the response.</mark>

```
dig google.com A @<IP>
```

## <mark style="color:red;">Known Vulnerabilities</mark>

### <mark style="color:yellow;">Sigred</mark>

{% embed url="https://research.checkpoint.com/2020/resolving-your-way-into-domain-admin-exploiting-a-17-year-old-bug-in-windows-dns-servers" %}

### <mark style="color:yellow;">Simple DNS Plus Remote DoS</mark>

{% embed url="https://www.exploit-db.com/exploits/6059" %}

{% embed url="https://www.cvedetails.com/vulnerability-list/vendor_id-8348/product_id-14553/Simpledns-Simple-Dns-Plus.html" %}
