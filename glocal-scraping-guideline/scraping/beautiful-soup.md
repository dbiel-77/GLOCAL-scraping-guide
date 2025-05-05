# BeautifulSoup

Beautiful Soup is a python library that makes scraping easy - providing a simple method for navigating, searching, and modifying a parse tree. It contains most of what will be necessary for exctracting information from an HTML file (Be it one specified inline, or retrieved via requests) - especially useful in that case for automating the data extraction process. 

While you can copy and paste the information on a webpage line by line, you will quickly realize that



## 1. Prerequisites

- **Python version**: 3.7 or later  
- **Install packages**:
```bash
pip install requests beautifulsoup4
```

## 2. `requests` and `beautifulsoup`

In order to understand why we need requests, it's important to understand that BeautifulSoup is nothing more than a website-structure-navigator - you still need to provide a source for it to navigate. Using requests as we do below, we retrieve the raw HTML, and pass it on as a variable to beautiful soup to parse

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://example.gov/parks")
response.raise_for_status()    # stops on 4xx/5xx
html = response.text

# feed it your HTML and choose a parser
soup = BeautifulSoup(html, "html.parser")

# given the soup, you can retrieve whatever it
# is that you are looking for within that URL
# functions are explained in next chapter
```
## Choosing a Parser

```{figure} /_static/bs/choosehtmlparser.png
:name: choo-choo-choose
You should probably choo-choo-choose html.parser (unless you know what you're doing)
```
Choosing the right parser can make all the difference - depending on the site. 9 times out of 10, python's native html parser `html.parser` is enough. The most common parsers are as

| Parser        | Install Requirement | Speed       | Leniency with Broken HTML | Notes                                                 |
|---------------|---------------------|-------------|----------------------------|-------------------------------------------------------|
| html.parser   | Built-in (standard) | Moderate    | High                       | Python’s built-in parser; good fallback; no install.  |
| lxml          | `pip install lxml`  | Fast        | High                       | Very fast and lenient; also supports XML parsing.     |
| html5lib      | `pip install html5lib` | Slow    | Very High                  | Parses HTML like a browser; best for worst HTML.      |
| lxml-xml      | `pip install lxml`  | Fast        | Low (strict XML rules)     | Use for well-formed XML, not ideal for broken HTML.   |

### XML parsing
If however, you come across a website that has embedded xml:
```html
<html>
    <body>
        <data>
            <!------  XML BELOW  ------>
            <entry key="foo">123</entry>
            <entry key="bar">456</entry>
        </data>
    </body>
</html>
```
You would use `lxml`, because html.parser and `html5lib` do not support xml parsing.


