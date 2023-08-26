---
description: Find any hidden or unknown subdomains and directories on the target system.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# Subdomains & Directories

## <mark style="color:red;">Directory and Subdomain Discovery</mark>

### <mark style="color:yellow;">Subdomain finder one-liners</mark>

#### <mark style="color:purple;">Find subdomains from various sources and add them to output.txt file.</mark>&#x20;

```
(curl -s "https://rapiddns.io/subdomain/$TARGET?full=1#result" 2>/dev/null | grep "<td><a" 2>/dev/null | cut -d '"' -f 2  2>/dev/null | grep http 2>/dev/null | cut -d '/' -f3 2>/dev/null | sed 's/#results//g' 2>/dev/null | sort -u 2>/dev/null) > output.txt

(curl -s https://dns.bufferover.run/dns?q=.$TARGET 2>/dev/null |jq -r .FDNS_A[] 2>/dev/null |cut -d',' -f2 2>/dev/null|sort -u 2>/dev/null ) >> output.txt

(curl -s "https://riddler.io/search/exportcsv?q=pld:${TARGET}" 2>/dev/null| grep -Po "(([\w.-]*)\.([\w]*)\.([A-z]))\w+" 2>/dev/null| sort -u 2>/dev/null ) >> output.txt

(curl -s "https://www.virustotal.com/ui/domains/${TARGET}/subdomains?limit=40" 2>/dev/null | grep -Po "((http|https):\/\/)?(([\w.-]*)\.([\w]*)\.([A-z]))\w+" 2>/dev/null | sort -u 2>/dev/null ) >> output.txt

(curl -s "https://certspotter.com/api/v1/issuances?domain=${TARGET}&include_subdomains=true&expand=dns_names"  2>/dev/null | jq .[].dns_names 2>/dev/null | tr -d '[]"\n ' 2>/dev/null | tr ',' '\n'2>/dev/null  ) >> output.txt

(curl -s "https://jldc.me/anubis/subdomains/${TARGET}"  2>/dev/null | grep -Po "((http|https):\/\/)?(([\w.-]*)\.([\w]*)\.([A-z]))\w+"  2>/dev/null | sort -u  2>/dev/null   ) >> output.txt

(curl -s "https://securitytrails.com/list/apex_domain/${TARGET}"  2>/dev/null | grep -Po "((http|https):\/\/)?(([\w.-]*)\.([\w]*)\.([A-z]))\w+"   2>/dev/null| grep "${TARGET}" 2>/dev/null | sort -u 2>/dev/null ) >> output.txt

(curl --silent https://sonar.omnisint.io/subdomains/$TARGET 2>/dev/null | grep -oE "[a-zA-Z0-9._-]+\.$TARGET" 2>/dev/null | sort -u 2>/dev/null ) >> output.txt

(curl --silent -X POST https://synapsint.com/report.php -d "name=https%3A%2F%2F$TARGET" 2>/dev/null| grep -oE "[a-zA-Z0-9._-]+\.$TARGET" 2>/dev/null | sort -u 2>/dev/null ) >> output.txt

(curl -s "https://crt.sh/?q=%25.$TARGET&output=json" 2>/dev/null| jq -r '.[].name_value' 2>/dev/null| sed 's/\*\.//g' 2>/dev/null| sort -u 2>/dev/null ) >> output.txt
```

### <mark style="color:yellow;">Sublist3r</mark>

```
sublist3r -d [domain] -t [threads] -o [output] -v -b [brute force mode>]
```

### <mark style="color:yellow;">OWASP Opendoor</mark>

```
python3 opendoor.py --host 10.10.10.6 --scan=directories -t 50
```

### <mark style="color:yellow;">Cansina</mark>

```
python ./cansina.py -u 10.10.10.6 -p ./directories.dat --persist -t 50 --show-type --full-path -b 403,404
```

### <mark style="color:yellow;">Dirb</mark>

```
dirb http://example.com -r
```

### <mark style="color:yellow;">Dirsearch</mark>

#### <mark style="color:purple;">One of the best tools for discovering sub-directories and metadata search.</mark>

{% embed url="https://github.com/maurosoria/dirsearch" %}

```
python3 dirsearch.py -u http://192.168.56.122 -e php,exe.elf.cgi,asp,txt,pdf,png,jpg -r  -t 5  -w db/dicc.txt -x 403,404
```

### <mark style="color:yellow;">Patator</mark>

```
patator http_fuzz url=[url] method=POST body
```

### <mark style="color:yellow;">Gobuster</mark>

#### <mark style="color:purple;">Used for both subdomain/vhost and subdirectory discovery.</mark>

```
gobuster dir -w /usr/share/wordlists/dirb/directory-list-2.3-medium.txt -u 10.10.10
```

#### <mark style="color:purple;">Some useful options:</mark>

```
-x → extention
dir → directory brutforce
dns → subdomain bruteforce
-c → cookie string
-e → print full url
-U → username for auth
-p → password for basic auth
-P → proxy [http(s)://host:port]
-s → set positive status codes will be overwritten with statuscodesblacklist if set) (default "200,204,301,302,307,401,403")
-u → url
-o → output to a file
-k → skip ssl
-a → set user agent
```

#### <mark style="color:purple;">Gobuster Web Content Discovery</mark>

```
gobuster -u http:/// -w /usr/share/seclists/Discovery/ Web_Content/common.txt -s '200,204,301,302,307,403,500' -e
```

#### <mark style="color:purple;">Gobuster subdomain brute forcing</mark>

```
gobuster dns -w /usr/share/seclists/Discovery/ DNS/subdomains-top1million-110000.txt -d target.com
```

#### <mark style="color:purple;">Gobuster subdomain vhost search</mark>

```
gobuster vhost -u example.com -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt 
```

### <mark style="color:yellow;">Dirbuster</mark>

{% hint style="info" %}
Add URL with port, example:  [http://127.0.0.1:80/](http://127.0.0.1)
{% endhint %}

* Wordlist for bruteforce is in /usr/share/wordlists/dirbuster
* Set go faster 200 threats (usually works best) in file extensions add any file type you want to look for like rar, docx, zip, etc.

<figure><img src="../../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:yellow;">Wfuzz</mark>

#### <mark style="color:purple;">It can fuzz any given location in a url, the location is specified by the "FUZZ" parameter:</mark>

```
wfuzz -u http://sneakycorp.htb -w /usr/share/wordlists/dirbuster/directory-list -2.3-small.txt -H 'Host: FUZZ.sneakycorp.htb' --hw 1
```

#### <mark style="color:purple;">Find files and extensions, Hide 404 codes:</mark>

```
wfuzz -u http://10.10.15.205/firstdirc/seconddir/FUZZ.extension -w /usr/share/dirbuster/wordl
```
