# BeautifulSoup

## 1. Prerequisites

- **Python version**: 3.7 or later  
- **Install packages**:
```bash
pip install requests beautifulsoup4
```

## 2. Why Use `requests`

In the last chapter we learned how HTTP works and how to craft requests. `requests` handles that for us in Python:

```python
import requests

response = requests.get("https://example.gov/parks")
response.raise_for_status()    # stops on 4xx/5xx
html = response.text
```

We need `requests` to retrieve the raw HTML before parsing it with BeautifulSoup.

## Importing & Initializing BeautifulSoup

```python
from bs4 import BeautifulSoup

# feed it your HTML and choose a parser
soup = BeautifulSoup(html, "html.parser")
```

- `"html.parser"` is built in.  
- Alternatives (`"lxml"`, `"html5lib"`) can be faster or more lenient if installed.

