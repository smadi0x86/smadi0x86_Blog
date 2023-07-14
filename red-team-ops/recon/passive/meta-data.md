---
description: >-
  Metadata is data that describes, structures and administers different forms of
  content.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# Meta data

## <mark style="color:red;">Gathering site/domain metadata from online sources</mark>

### <mark style="color:yellow;">Metadata Sites</mark>

* [**Archive.org**](https://archive.org)&#x20;
* [**metapicz**](http://metapicz.com/#landing)&#x20;
* [**image forensic**](http://www.imageforensic.org)&#x20;
* [**findexif**](http://findexif.com)&#x20;
* [**exif regex**](http://exif.regex.info/exif.cgi)

### <mark style="color:purple;">wget</mark>

```
wget -nd -r -R htm,html,php,asp,aspx,cgi -P
wget -nd -r -A pdf,doc,docx,xls,xlsx -P
```

### <mark style="color:purple;">metagoofil</mark>

**Find all files on a domain**

```
metagoofil  -d [domain] -t pdf,docx,txt -o metagoofil -n 50 -l 200
```

### <mark style="color:purple;">AWS Recon</mark>

{% embed url="https://github.com/darkbitio/aws-recon" %}

### <mark style="color:purple;">Infoga</mark>

**Linux and Windows**

```
git clone https://github.com/m4ll0k/Infoga.git
cd Infoga
python setup.py install
python infoga.py

python infoga.py --domain nsa.gov --source all --breach -v 2 --report ../nsa_gov.txt
python infoga.py --info m4ll0k@protonmail.com --breach -v 3 --report ../m4ll0k.txt
```

### <mark style="color:purple;">VPN Configuration files</mark>

**OME organizations publicly distribute configuration files and keys for VPN systems.**&#x20;

**Cisco profile configuration files (PCFs) contain IPsec VPN client variables, including the following:**

• VPN server endpoint addresses

• Plaintext credentials (group name and password)

• Encrypted credentials (an obfuscated group password)

#### Using google search strings:

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

#### Crack / decode the passwords

{% embed url="https://artemkin.github.io/cisco-password-decoder/index_plain.html" %}

{% embed url="http://artemkin.github.io/cisco-password-decoder" %}

{% embed url="https://www.ifm.net.nz/cookbooks/passwordcracker.html" %}

## <mark style="color:red;">Gathering site/domain metadata from offline sources</mark>

### <mark style="color:yellow;">Extract metadata from the data we have.</mark>

### <mark style="color:purple;">exiftool</mark>

```
exiftool poissonrecon.pdf
```

### <mark style="color:purple;">Strings</mark>

```
-e >>> for 16 bit big-endian unicode
-el >>> for 16 bit little-endian unicode
-n >>> set minimum caracter length
-a >>> ascii
-u >>> unicode

strings -n 4 test.elf
```

### <mark style="color:purple;">tesseract</mark>

**Find text in images**

```
apt install tesseract-ocr
apt-get install tesseract-ocr-LANG
apt-get install tesseract-ocr-all

for x in *.jpg; do tesseract $x stdout -psm 11 -l eng >> tesser.out; done
```

### <mark style="color:purple;">Music Metadata</mark>

{% embed url="https://github.com/mischif/music-metadata-tools" %}
