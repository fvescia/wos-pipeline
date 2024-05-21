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