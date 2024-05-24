How can I install the `zipfile` Python module from https://github.com/python/cpython/tree/3.12/Lib/zipfile/?

***

How does this code work? `urllib.request.urlretrieve(link.get('href'), 'test.zip')`

***

What does this code do?

```
def download_and_unzip(url, extract_to='.'):
    http_response = urlopen(url)
    zipfile = ZipFile(BytesIO(http_response.read()))
    zipfile.extractall(path=extract_to)
```

***

Does this return an HTTP message?

```
# For now: Retrieve Schema_2023.zip with urlretrieve
url = 'https://restricted.lib.uchicago.edu/licensed_data/Clarivate/Web-of-Science-Core_2024-01-16/Schema_2023.zip'
file = 'xml/Schema_2023.zip'
urlretrieve(url, file)
```

***

What is wrong with this code?

```
url = 'https://restricted.lib.uchicago.edu/licensed_data/Clarivate/Web-of-Science-Core_2024-01-16/Schema_2023.zip'
file = 'xml/Schema_2023.zip'
urlretrieve(url, file)
```

***

After I run the code you provided, I tried to run:

```
with ZipFile('xml/Schema_2023.zip', 'r') as f:
    f.extractall()
```

And it throws "BadZipFile: File is not a zip file." Why?

***

Why is Schema_2023.zip 51 KB if I download it by clicking on it by only 28 KB if I download it with this code:

```
param = {'downloadformat': 'zip'}
r = requests.get(url, params = param)
with open('xml/Schema_2023.zip', 'wb') as f:
    f.write(r.content)
```

***

How do I tell what headers my browser has?

***

What are these encoding types? Accept-Encoding: gzip, deflate, br, zstd

***

What difference between these headers would explain why the first downloads a 28 KB file and the second downloads a 51 KB file?

FIRST HEADER:
```
{'Date': 'Thu, 23 May 2024 20:24:13 GMT',
'Content-Type': 'text/html;charset=utf-8',
'Transfer-Encoding': 'chunked',
'Connection': 'keep-alive',
'Server': 'nginx', 'Vary': 'Accept-Encoding', 'x-okta-request-id': '64a6773a70066427ab74ea1105bbb7e7', 'x-xss-protection': '0', 'p3p': 'CP="HONK"', 'set-cookie': 'sid="";Version=1;Path=/;Max-Age=0, autolaunch_triggered=""; Expires=Thu, 01 Jan 1970 00:00:10 GMT; Path=/, JSESSIONID=1976A2B1B673ECDFF56198FA99A3E060; Path=/; Secure; HttpOnly, t=red-dark; Path=/, DT=DI1cyJSdLXWTfiW-5T_oe0k7Q;Version=1;Path=/;Max-Age=63072000;Secure;Expires=Sat, 23 May 2026 20:24:13 GMT;HttpOnly', 'content-security-policy': "default-src 'self' uchicago.okta.com *.oktacdn.com; connect-src 'self' uchicago.okta.com uchicago-admin.okta.com *.oktacdn.com *.mixpanel.com *.mapbox.com *.mtls.okta.com uchicago.kerberos.okta.com *.authenticatorlocalprod.com:8769 http://localhost:8769 http://127.0.0.1:8769 *.authenticatorlocalprod.com:65111 http://localhost:65111 http://127.0.0.1:65111 *.authenticatorlocalprod.com:65121 http://localhost:65121 http://127.0.0.1:65121 *.authenticatorlocalprod.com:65131 http://localhost:65131 http://127.0.0.1:65131 *.authenticatorlocalprod.com:65141 http://localhost:65141 http://127.0.0.1:65141 *.authenticatorlocalprod.com:65151 http://localhost:65151 http://127.0.0.1:65151 https://oinmanager.okta.com data: data.pendo.io pendo-static-5634101834153984.storage.googleapis.com pendo-static-5391521872216064.storage.googleapis.com; script-src 'unsafe-inline' 'unsafe-eval' 'self' uchicago.okta.com *.oktacdn.com; style-src 'unsafe-inline' 'self' uchicago.okta.com *.oktacdn.com; frame-src 'self' uchicago.okta.com uchicago-admin.okta.com login.okta.com com-okta-authenticator: api-322c2749.duosecurity.com; img-src 'self' uchicago.okta.com *.oktacdn.com *.tiles.mapbox.com *.mapbox.com data: data.pendo.io pendo-static-5634101834153984.storage.googleapis.com pendo-static-5391521872216064.storage.googleapis.com blob:; font-src 'self' uchicago.okta.com data: *.oktacdn.com fonts.gstatic.com; frame-ancestors 'self'", 'x-rate-limit-limit': '60', 'x-rate-limit-remaining': '59', 'x-rate-limit-reset': '1716495913', 'accept-ch': 'Sec-CH-UA-Platform-Version', 'cache-control': 'no-cache, no-store', 'pragma': 'no-cache', 'expires': '0', 'x-frame-options': 'SAMEORIGIN', 'x-content-type-options': 'nosniff', 'x-ua-compatible': 'IE=edge', 'content-language': 'en', 'Strict-Transport-Security': 'max-age=315360000; includeSubDomains', 'X-Robots-Tag': 'noindex,nofollow', 'Content-Encoding': 'gzip'}
```

SECOND HEADER:
```
GET /licensed_data/Clarivate/Web-of-Science-Core_2024-01-16/Schema_2023.zip HTTP/1.1
Host: restricted.lib.uchicago.edu
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:126.0) Gecko/20100101 Firefox/126.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br, zstd
DNT: 1
Connection: keep-alive
Referer: https://restricted.lib.uchicago.edu/licensed_data/Clarivate/Web-of-Science-Core_2024-01-16/
Cookie: _ga_Z8EJD4Z3L6=GS1.1.1716171245.111.1.1716171562.0.0.0; _ga=GA1.1.250659345.1695649685; _ga_GRPFZE8KD9=GS1.2.1716324594.411.1.1716324597.0.0.0; _hp2_id.3001039959=%7B%22userId%22%3A%228813884382229590%22%2C%22pageviewId%22%3A%227874216926301144%22%2C%22sessionId%22%3A%22958501556086817%22%2C%22identity%22%3A%22uu-2-6f709dc02b9c191e9e02c5b792810f720d98595f76344fa5b7afec35af52a757-Ok0GrV1qAYbyFNV6eFfXhlP3vqsviXTaNQluzyAj%22%2C%22trackerVersion%22%3A%224.0%22%2C%22identityField%22%3Anull%2C%22isIdentified%22%3A1%2C%22oldIdentity%22%3Anull%7D; _ga_TN8MPQY6Z6=GS1.2.1715961575.11.0.1715961575.0.0.0; _ga_961RVMDCZW=GS1.1.1715976297.33.1.1715976854.0.0.0; _ga_FJZP0XWT17=GS1.1.1713550202.7.0.1713550205.0.0.0; _ga_ETKKQ3QSH9=GS1.1.1715975824.11.0.1715975824.0.0.0; _ga_DH5MFSYV4N=GS1.1.1715461771.3.1.1715462288.0.0.0; _ce.s=v~9dd7aa7c22062de87840273d10b6bb10bf7ba0ef~lcw~1715885147431~vpv~2~lva~1715885115475~v11.cs~210933~v11.s~6ff1bfd0-13b4-11ef-a8ed-839d2ca9bd45~v11.sla~1715885147434~gtrk.la~lw9lpp9j~v11.send~1715885147425~lcw~1715885147434; _ga_WDPY57VX02=GS1.1.1715875437.4.0.1715875437.60.0.0; _ga_CP0H0SNRBP=GS1.1.1715726972.9.0.1715726972.0.0.0; _ga_2V4M4Z833J=GS1.1.1709930758.4.1.1709930796.0.0.0; _ga_DBDWJCS0JR=GS1.2.1709930759.4.0.1709930759.0.0.0; _ga_VG1BZJSZLR=GS1.2.1715357006.21.1.1715357230.0.0.0; _ga_F5DNYMJSY2=GS1.1.1696090025.1.1.1696090831.0.0.0; _ga_EWT5B3NZ15=GS1.1.1716411818.42.1.1716411872.0.0.0; _ga_81PY6B3PJ9=GS1.2.1713283161.10.1.1713283546.0.0.0; _ga_G5WBF5B17C=GS1.1.1696171268.1.1.1696171896.0.0.0; _ga_3RX49G9TFX=GS1.1.1715458365.90.0.1715458365.60.0.0; _ga_ME7M0NVE46=GS1.1.1702312517.3.0.1702312548.0.0.0; __hstc=70356175.28438e43a03f05cbaf942fb6df152758.1696529391377.1711842728926.1712189337240.28; hubspotutk=28438e43a03f05cbaf942fb6df152758; _ga_92VPCXTFPH=GS1.1.1710540623.16.1.1710540957.0.0.0; _ga_7J8R89PK8B=GS1.1.1696888334.1.0.1696888334.0.0.0; _ga_R65NEXK9GS=GS1.1.1715705184.5.1.1715706398.0.0.0; _ga_7QSFD2XELZ=GS1.1.1697731803.1.1.1697732316.0.0.0; _ga_GY2RTGK4MR=GS1.1.1698337554.1.0.1698337604.0.0.0; _ga_546Y742C5P=GS1.1.1713889740.2.1.1713889864.0.0.0; _ga_BMYWQX4N28=GS1.1.1698509677.1.0.1698509677.0.0.0; _ga_LSPF2XQTRN=GS1.1.1699886152.1.1.1699886324.0.0.0; _hp2_props.3001039959=%7B%22Base.appName%22%3A%22Canvas%22%7D; _ga_R3LR1TE0B9=GS1.1.1702660643.1.1.1702661576.0.0.0; _ga_BXGESQEFLQ=GS1.1.1711473572.6.0.1711473572.0.0.0; _hjSessionUser_3672197=eyJpZCI6ImRhZjc3OTgyLTlkMzQtNTZkNy04NjZhLTU3NzlmNzkyZDMxOSIsImNyZWF0ZWQiOjE3MDI2NjMxOTUyNjUsImV4aXN0aW5nIjp0cnVlfQ==; _ga_8BKBDBKZJG=GS1.1.1704417184.1.0.1704417192.0.0.0; AMCV_242B6472541199F70A4C98A6%40AdobeOrg=179643557%7CMCIDTS%7C19744%7CMCMID%7C89348243899300971912351675503303229530%7CMCAAMLH-1706458526%7C7%7CMCAAMB-1706458526%7C6G1ynYcLPuiQxYZrsz_pkqfLG9yMXBpb2zX5dvJdYQJzPXImdj0y%7CMCOPTOUT-1705860926s%7CNONE%7CvVersion%7C5.5.0; __qca=P0-1712649493-1705350126149; _ga_P7DT1QXXSK=GS1.1.1709749906.2.1.1709749951.0.0.0; _ga_7PM892EE02=GS1.1.1709749906.2.1.1709749951.15.0.0; _ga_ZYGQ8432T2=GS1.1.1709749906.2.1.1709749951.15.0.0; _ga_T8K9FT0CMZ=GS1.2.1709749910.2.1.1709749919.51.0.0; _hjSessionUser_2580298=eyJpZCI6IjE1OWQzODQzLTE0MTktNTYzNC1iNTAyLWNjMTY2NzQ3YTIzMSIsImNyZWF0ZWQiOjE3MDY0MTg3Njg5MjIsImV4aXN0aW5nIjp0cnVlfQ==; _ga_VJ81RKXDL1=GS1.1.1706418980.1.0.1706419043.60.0.0; kndctr_1B6E34B85282A0AC0A490D44_AdobeOrg_identity=CiYxNjk2ODYxNzAwMTQ1NzYxMDMxMDU5MTUxOTk4MjYxNDYyNzM3MFISCPCqgvTUMRABGAEqA1ZBNjAA8AGetOnV7DE=; AMCV_1B6E34B85282A0AC0A490D44%40AdobeOrg=MCMID|16968617001457610310591519982614627370; _ga_9C4T5P6PP6=GS1.1.1712798195.2.1.1712798325.60.0.0; _clck=1rjlw6b%7C2%7Cfku%7C0%7C1488; _ga_4819PJ6HEN=GS1.1.1710783247.2.1.1710783405.0.0.0; _ga_0HYE8YG0M6=GS1.1.1710783248.2.1.1710783405.0.0.0; optimizelyEndUserId=oeu1706419247023r0.537539957764616; _hjSessionUser_864760=eyJpZCI6ImZkMGE4MDg5LTdkYmEtNTVhMS1hZWM5LTgxYWJmMDM2ODMwZCIsImNyZWF0ZWQiOjE3MDY0MTkyNDcxOTUsImV4aXN0aW5nIjp0cnVlfQ==; _ga_FK139JDLDB=GS1.1.1706899140.1.1.1706899164.36.0.0; _ga_CBV5E5LWRW=GS1.1.1707240356.1.1.1707240452.60.0.0; google-analytics_v4_zeJV__engagementDuration=2953; google-analytics_v4_zeJV__engagementStart=1707240450896; google-analytics_v4_zeJV__counter=2; google-analytics_v4_zeJV__session_counter=1; google-analytics_v4_zeJV__ga4=88ceb61c-1631-4029-80cc-c6ce8be9ed60; google-analytics_v4_zeJV__let=1707240413121; _ga_313878794=GS1.1.1707665497.1.0.1707665502.0.0.0; _ga_2Q93Y52QZ4=GS1.1.1708577877.4.0.1708577877.0.0.0; __utma=198748336.250659345.1695649685.1708029947.1708029947.1; __utmz=198748336.1708029947.1.1.utmcsr=google|utmccn=(organic)|utmcmd=organic|utmctr=(not%20provided); _ga_RT01FGW8X2=GS1.1.1711987266.3.1.1711987398.0.0.0; _ga_9RQ8J9GLNS=GS1.1.1709048056.3.0.1709048056.0.0.0; _ga_4CK6M3GRNP=GS1.1.1714770366.3.1.1714770771.0.0.0; _hjSessionUser_3545609=eyJpZCI6IjY4YzUxNjdkLThlMWItNWMwNS04MDc2LTI2NDJhZDIwNDA1ZSIsImNyZWF0ZWQiOjE3MDk5MzA3NTkzOTIsImV4aXN0aW5nIjpmYWxzZX0=; _ga_XM25KL7FV9=GS1.1.1715885114.2.1.1715885147.0.0.0; _ga_HZZ4S5GSTE=GS1.1.1710037299.1.0.1710037299.0.0.0; _ga_Z1ELTQ3X15=GS1.1.1710533559.1.0.1710533560.0.0.0; _ga_WDS6L3YSXZ=GS1.1.1711141241.1.0.1711141241.0.0.0; AMCV_4D6368F454EC41940A4C98A6%40AdobeOrg=179643557%7CMCIDTS%7C19809%7CMCMID%7C36212697128455556181844536557281908954%7CMCAID%7CNONE%7CMCOPTOUT-1711568711s%7CNONE%7CMCAAMLH-1712166311%7C7%7CMCAAMB-1712166311%7Cj8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI%7CMCCIDH%7C-554462621%7CMCSYNCSOP%7C411-19814%7CvVersion%7C5.5.0; s_pers=%20c19%3Dsd%253Aproduct%253Ajournal%253Aarticle%7C1711563321558%3B%20v68%3D1711561521254%7C1711563321560%3B%20v8%3D1711561521563%7C1806169521563%3B%20v8_s%3DLess%2520than%25201%2520day%7C1711563321563%3B; _gcl_au=1.1.1036132354.1711650341.1708486383.1711717282.1711717282; _ga_5H3MYEZ3LW=GS1.2.1711716742.1.1.1711716793.0.0.0; _ga_H4TRYTBL9H=GS1.2.1715748404.2.1.1715748460.0.0.0; _ga_MH0P8STC4F=GS1.1.1712635993.1.1.1712636011.0.0.0; _ga_9ERS4XE1Q5=GS1.1.1714875528.3.0.1714875528.0.0.0; _ga_Z5FZK186D9=GS1.1.1713712776.2.1.1713712900.0.0.0; _ga_JRGETX8K6F=GS1.1.1715458359.3.0.1715458359.0.0.0; _ga_6VKR0E840K=GS1.1.1713889540.1.1.1713889866.0.0.0; _fbp=fb.1.1713889748342.1013035901; _uetvid=bf2b6570b3e311eeb52941210e97b472|1sc1vdg|1715302804164|1|1|bat.bing.com/p/insights/c/t; _ga_KC8J6L04RE=GS1.1.1715458408.3.1.1715458493.0.0.0; _ce.irv=notReturning; cebs=1; cebsp_=1; _opensaml_req_ss%3Amem%3A5e290bddb5f309fc4d04f04f450cdc776da2b3d77377542ab9bed2b598c526fa=_d798b9be183f88c11d2da6ee99a38cf3; _gid=GA1.2.1551584411.1716411819; ezproxy=uW17c1x7k4eGfy3; ezproxyl=uW17c1x7k4eGfy3; ezproxyn=uW17c1x7k4eGfy3; _shibsession_64656661756c7468747470733a2f2f726573747269637465642e6c69622e756368696361676f2e6564752f73686962626f6c657468=_6e537844bde04b7ac1171fb42a613553
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=1
```
***

How does this code work?

```
url = "https://www.something.edu/apps/account/cas/login?service=https%3A%2F%2Frf.something.something.edu"
r = session.get(url, headers = headers)
soup = BeautifulSoup(r.content, 'html5lib')
login_data['lt'] = soup.find('input', attrs={'name' : 'lt' })['value']
r = session.post(url, data = login_data, headers = headers, )
print(r.content)```
```

***

How do I log into this page with a POST request? https://uchicago.okta.com/app/uchicago_canvas_1/exk4c36t2q9kwtQ18697/sso/saml?SAMLRequest=fVLLctowFN33Kzza27KFY8caYIaGyYSZtKWBZNENI%2BRr0GBLjq6Ux99XmDQli2R7dB46Rxqj6Nqez7zb6zt49IAueulajXw4mBBvNTcCFXItOkDuJF%2FNftxylqS8t8YZaVpyJvlaIRDBOmU0iRbzCdmIqtiWWcPikUzzON%2FWTVylbBuXBWNFKS%2BrpixI9AAWg2ZCgkUQInpYaHRCuwClLI%2FTi5iN1ozxPOcX6R8SzUMPpYUbVHvneuSUerlXUuxMYg5OJNJ0VPT9O7qRQj8J3GQUXg65HBWOPVaHZ%2Fc7uyyqkiIaeuxHotm%2FDldGo%2B%2FArsA%2BKQn3d7f%2Fs05myXsk1J62Zqf0m8nybbrvStdK775ebXsiIb9Zr5fx8tdqTabjow8ftrDTY%2Bp5QRXWsV46b2HoeeSyMT2XjE8P%2FzOELeZL0yr5Gl0b2wn3%2BV2yJBsQVcfNQOVeYw9SNQrqMEzbmucrC8LBhIR8IHR6Cv34wabf%2FgI%3D

***

How would I update this code to specify version 3.4 of PySpark?

```
%%configure -f
{
    "conf": {
        "spark.pyspark.python": "python3",
        "spark.pyspark.virtualenv.enabled": "true",
        "spark.pyspark.virtualenv.type":"native",
        "spark.pyspark.virtualenv.bin.path":"/usr/bin/virtualenv"
    }
}
```

***

How do I display my files in a 3.0.1-amzn-0 Cpark context?

***

What is the equivalent of "ls" in a 3.0.1-amzn-0 Cpark context?

***

How do I make obj a ZIP file? `obj = s3r.Object('wos-bucket', 'xml/2023_CORE/WR_2023_20240112181857_CORE_0001.xml.gz')`

***

How do I read 'xml/2023_CORE/WR_2023_20240112181857_CORE_0001.xml.gz' from S3 into a Spark context?

***

Does this code unzip the .gz file before writing it to an RDD?

```
path = 'xml/2023_CORE/WR_2023_20240112181857_CORE_0001.xml.gz'
rdd = spark.sparkContext.textFile(path)
```

***

Please explain this regex to me: `regex = re.compile('(.*zip$)|(.*rar$)|(.*r01$)')`

***

Please explain this code to me:

```
for filename in os.listdir('.'):
  if filename.endswith('.gz'): 
     with gzip.open(filename, 'rb') as f_in:
       with open(filename[:-3], 'wb') as f_out:
         shutil.copyfileobj(f_in, f_out)
```