# Importando dados da internet

### Importando flat files da internet

Esta operação pode ser feita manualmente pelo browser, mas  não é reproduzível nem escalável. Por isso é necessário desenvolvermos o código para automatizar esta rotina.

* **o pacote urllid**
* provê uma interface para buscar dados pela rede;
* <code>urlopen</code> aceita URL (Universal Resource Locator) no lugar de nomes de arquivos;

```python
from urllib.request import urlretrieve
url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv'
# a função urlretriever salva o conteúdo de url no arquivo especificado
urlretrieve(url, 'winequality-white.csv')
```
<code>('winequality-white.csv', <http.client.HTTPMessage at 0x103cf1128>)</code>

### Solicitações HTTP para importar arquivos da rede

Sempre que acessamos um site nós enviamos uma solicitação HTTP (Hyper Text Transfer Protocol), conhecida como GET request. A função <code>urlretrieve()</code> realiza uma GET request e salva localmente os dados relevantes. 

* **GET request utilizando urllib**
```python
# o código abaixo retorna o HTML da página como string -> .read()
from urllib.request import urlopen, Request
url = "https://www.wikipedia.org/"
request = Request(url)
response = urlopen(request)
html = response.read()
response.close()
```
* **GET request utilizando o pacote request do Python**
```python
# o código abaixo retorna o HTML da página como string -> .text
import requests
url = "https://www.wikipedia.org/"
r = requests.get(url)
text = r.text
```

### Scraping com Python

HTML é um mix de dados estruturados e não estruturados. De uma forma geral, dados estruturados são:
* dados que que tem um modelo pré definido;
* dados organizados em uma maneira definida;

Dados não estruturados são os que não possuem nenhuma das características acima. 

Embora contenha muitos dados não estruturados, HTML possui diversas tags em sua estrutura que identificam onde estão links e headings,por exemplo.

Utilizando o pacote Python BeautifulSoup podemos analisar e extrair dados estruturados a partir do HTML obtido da rede.

```python
# primeiro é usado request para extrair o HTML da rede
# cria-se um objeto BeautifulSoup
# utilizando o método prettify, temos o HTML identado como esperado
from bs4 import BeautifulSoup
import requests
url = 'https://www.crummy.com/software/BeautifulSoup/'
r = requests.get(url)
html_doc = r.text
soup = BeautifulSoup(html_doc)
```
* **Prettified Soup**
<code>print(soup.prettify())</code>

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/transitional.dtd">
<html>
    <head>
        <meta content="text/html; charset=utf-8" http-equiv="Content-Type"/>
        <title>
        Beautiful Soup: We called him Tortoise because he taught us.
        </title>
        <link href="mailto:leonardr@segfault.org" rev="made"/>
        <link href="/nb/themes/Default/nb.css" rel="stylesheet" type="text/css"/>
        <meta content="Beautiful Soup: a library designed for screen-scraping HTML and XML." name="Description"/>
        <meta content="Markov Approximation 1.4 (module: leonardr)" name="generator"/>
        <meta content="Leonard Richardson" name="author"/>
    </head>
<body alink="red" bgcolor="white" link="blue" text="black" vlink="660066">
<img align="right" src="10.1.jpg" width="250"/>
<br/>
<p>
```

* **explorando BeautifulSoup**

<code>print(soup.title)</code> 

```html
<title>Beautiful Soup: We called him Tortoise because he taught us.</title>
```

<code>print(soup.get_text())</code>

```html
Beautiful Soup: We called him Tortoise because he taught us.
You didn't write that awful page. You're just trying to
get some data out of it. Beautiful Soup is here to
help. Since 2004, it's been saving programmers hours or
days of work on quick-turnaround screen scraping
projects.
```

* **find_all()**
```python
for link in soup.find_all('a'):
    print(link.get('href'))
```
```html
bs4/download/
#Download
bs4/doc/
#HallOfFame
https://code.launchpad.net/beautifulsoup
https://groups.google.com/forum/?fromgroups#!forum/beautifulsoup
http://www.candlemarkandgleam.com/shop/constellation-games/
http://constellation.crummy.com/Constellation%20Games%20excerpt.html
https://groups.google.com/forum/?fromgroups#!forum/beautifulsoup
https://bugs.launchpad.net/beautifulsoup/
http://lxml.de/
http://code.google.com/p/html5lib/
```
