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