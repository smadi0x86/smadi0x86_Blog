---
description: Content Management Systems are most vulnerable from plugins they use.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# CMS

## <mark style="color:red;">Wordpress</mark>

#### <mark style="color:purple;">The WordPress version is shown in the "generator" meta tag (unless removed by the site).</mark>&#x20;

#### <mark style="color:purple;">You may search the source code (CTRL-F) for "generator" to see the version.</mark>&#x20;

#### <mark style="color:purple;">This curl command will also show it. The "-s" flag is for "silent"</mark>

```
curl -s http://example.com/wordpress/ | grep generator
```

### <mark style="color:yellow;">Basic information</mark>

```
wpscan --url https://192.168.26.141
```

### <mark style="color:yellow;">Check for vulnerable plugins</mark>

```
wpscan --url https://192.168.26.141:12380/blogblog --enumerate vp
```

### <mark style="color:yellow;">Check for exploits that match the version of wordpress</mark>

```
wpscan --no-update --url http://www.example.com/wordpress/
wpscan --no-update --url http://www.example.com/wordpress/ | grep Title
wpscan --no-update --url http://www.example.com/wordpress/ | grep Title | wc -l
```

### <mark style="color:yellow;">Vulnerability and plugin scan</mark>

```
wpscan --url sandbox.local --enumerate ap,at,cb,dbe
```

### <mark style="color:yellow;">Enumerate usernames</mark>

```
wpscan --url http://192.168.56.149/wordpress/ --enumerate u --force --wp-content-dir wp-content
```

### <mark style="color:yellow;">Password attack on discovered usernames</mark>

```
wpscan --url http://192.168.56.149/wordpress/ --passwords /usr/share/wordlists/fasttrack.txt --usernames userlist -t 25
```

### <mark style="color:yellow;">Enumerate everything</mark>

```
wpscan --url https://192.168.26.141
```

### <mark style="color:yellow;">Scan with nmap NSE scripts</mark>

```
nmap -sV --script http-wordpress-enum 10.11.1.234
nmap -Pn --script http-wordpress-enum --script-args check-latest=true,search-limit=10 10.11.1.234
nmap -sV 10.11.1.234 --script http-wordpress-enum --script-args limit=25
```

## <mark style="color:red;">Drupal</mark>

### <mark style="color:yellow;">Droopscan</mark>

#### <mark style="color:purple;">Installation:</mark>

```
apt-get install python-pip
pip install droopescan
```

#### <mark style="color:purple;">Scanning:</mark>

```
droopescan scan drupal -u example.org        
droopescan scan -u example.org
droopescan scan -U list_of_urls.txt
```

## <mark style="color:red;">Joomla</mark>

### <mark style="color:yellow;">Joomscan</mark>

```
joomscan --url http://192.168.56.126 -ec
```

#### <mark style="color:purple;">Get components running on the website</mark>

```
joomscan --url http://10.10.10.150/ --random-agent --enumerate-components
```

#### <mark style="color:purple;">You can also check</mark>

```
/administrator/manifests/files/joomla.xml
```

#### <mark style="color:purple;">If you find components, you can often access the configuration file</mark>

```
JCE component → /components/com_jce/jce.xml
```

### <mark style="color:yellow;">Joomlavs</mark>

#### <mark style="color:purple;">Check for vulnerabilities affecting components</mark>

{% embed url="https://github.com/rastating/joomlavs" %}

## <mark style="color:red;">Nikto</mark>

#### <mark style="color:purple;">A free web application vulnerability scanner preinstalled on kali linux</mark>.

```
nikto -host example.com
```
