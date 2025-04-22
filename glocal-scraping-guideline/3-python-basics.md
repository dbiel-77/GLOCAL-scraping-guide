# Python Foundations for Web Scraping

Python is an open‑source, interpreted language that combines clear, readable syntax with a “batteries‑included” standard library—everything from HTTP clients to JSON parsers is ready to go out of the box. It supports multiple paradigms (procedural, object‑oriented, functional), automatic memory management, and an enormous ecosystem of third‑party packages. That makes Python both easy to learn and incredibly powerful for tasks like web scraping, data analysis, automation, and more.

**Installing Python & getting started**  
1. **Download & install**  
   - **Windows/macOS**: Grab the latest installer from https://python.org/downloads/ and follow the prompts.  
   - **Linux**:  
     ```bash
     # Debian/Ubuntu
     sudo apt update && sudo apt install python3 python3-venv python3-pip
     # Fedora
     sudo dnf install python3 python3-venv python3-pip
     ```
2. **Open a terminal**  
   - **Windows**:  
     - Press Win+R, type `cmd` or `powershell`, hit Enter.  
   - **macOS**:  
     - Spotlight (Search ⌘+Space) → “Terminal” → Enter.  
   - **Linux**:  
     - Ctrl+Alt+T or find “Terminal” in your applications menu.
3. **Verify installation**  
   ```bash
   python3 --version    # should print something like "Python 3.x.y"
   ```
4. **A Hidden Poem**  
   ```bash
   python3
   >>> import this
   ```

## Setting Up Your Python Environment

Make sure you have Python 3.7 or later installed. If you don’t, download it from https://www.python.org/downloads/.  

Create and activate a virtual environment to isolate your scraping projects:

```bash
python3 -m venv venv
# macOS/Linux
source venv/bin/activate
# Windows (PowerShell)
.\venv\Scripts\Activate.ps1
```

Upgrade pip and install the core libraries:

```bash
pip install --upgrade pip
pip install requests beautifulsoup4 lxml
```

Your environment now has everything you need to fetch pages and parse HTML.

## Basic Python: Variables and Types

Python is dynamically typed. You can assign any value to a variable:

```python
url = "https://example.gov/data" # string
retry_limit = 3 # integer
headers = {"User-Agent": "MyScraper/1.0"}  # dictionary with key:value pairs
parks = ['riverside park', 'lakeside park', 'bayside park'] # list, an ordered set
parks_unordered = ('riverside park', 'lakeside park', 'bayside park') # an unordered set
```

Common built‑in types you’ll use:

- **Strings** (`str`): sequences of characters
- **Integers** (`int`): a whole number, for example 3
- **Floating Point** ('float'): a floating decimal/point - for example, 3.5
- **Booleans** (`bool`): `True` or `False`
- **Lists** (`list`): ordered sequences
- **Tuples** (`tuple`): immutable sequences
- **Dictionaries** (`dict`): key–value mappings

## Working with Strings

Web scraping often means cleaning and extracting text. Python’s string methods make this easy:

```python
text = "   Hello, World!  \n"
clean = text.strip()                   # "Hello, World!"
lower = clean.lower()                  # "hello, world!"
parts = clean.split(",")               # ["Hello", " World!"]
joined = "-".join([p.strip() for p in parts])  
# "Hello-World!"    
snippet = clean[7:12]                  # "World"
```

Try these exercises:

- Given `"Page 123 of 456"`, extract the page numbers as integers.
- Replace all whitespace in `"New York City"` with underscores.
- Normalize a list of URLs by lowercasing and stripping trailing slashes.

## Data Structures for Scraping

You’ll often gather multiple items of data. Lists and dictionaries let you organize them:

```python
urls = [f"https://example.gov/page/{i}" for i in range(1, 6)]
results = []  # list of dicts

for url in urls:
    record = {"url": url, "status": None}
    results.append(record)
```

A single page’s parsed data can live in a dict:

```python
park = {
    "name": "Riverside Park",
    "address": "123 River Rd.",
    "hours": "6 AM–10 PM"
}
```

## Control Flow: Loops and Conditionals

Scrapers loop over multiple pages and make decisions:

```python
for page in range(1, 11):
    url = f"https://example.gov/page/{page}"
    resp = requests.get(url)
    if resp.status_code != 200:
        print(f"Skipping {url}: {resp.status_code}")
        continue
    # parse resp.text…
```

Use `while` loops for retries:

```python
attempt = 0
while attempt < retry_limit:
    attempt += 1
    resp = requests.get(url)
    if resp.ok:
        break
    time.sleep(2 ** attempt)
```

## Functions and Modularity

Wrap repeated logic into functions:

```python
import requests

def fetch(url, headers=None):
    resp = requests.get(url, headers=headers)
    resp.raise_for_status()
    return resp.text

def parse(html):
    from bs4 import BeautifulSoup
    soup = BeautifulSoup(html, "lxml")
    # extract data…
    return data

if __name__ == "__main__":
    raw = fetch("https://example.gov/data")
    data = parse(raw)
    print(data)
```

Functions let you test, reuse, and compose your scraping pipeline.

## Intermediate Features: List Comprehensions & Generators

List comprehensions make filtering and transforming concise:

```python
emails = [a["href"][7:] for a in soup.select("a[href^='mailto:']")]
```

Use generators for memory‑efficient iteration:

```python
def generate_urls(start, end):
    for i in range(start, end + 1):
        yield f"https://example.gov/page/{i}"

for url in generate_urls(1, 1000):
    process(url)
```

## Context Managers & File I/O

Use `with` to manage resources:

```python
with open("data.csv", "w", encoding="utf-8") as fp:
    writer = csv.writer(fp)
    for record in results:
        writer.writerow([record["name"], record["email"]])
```

When fetching pages, you can also stream large downloads:

```python
with requests.get(url, stream=True) as resp:
    for chunk in resp.iter_content(1024):
        fp.write(chunk)
```

## Error Handling and Logging

Catch exceptions and log progress:

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

try:
    html = fetch(url)
except requests.HTTPError as e:
    logger.error(f"Failed to fetch {url}: {e}")
else:
    data = parse(html)
```

## Advanced: Decorators, Iterators, and Packaging

- **Decorators** can add retry logic to any function:

  ```python
  from tenacity import retry, stop_after_attempt, wait_fixed

  @retry(stop=stop_after_attempt(3), wait=wait_fixed(2))
  def fetch_with_retry(url):
      return fetch(url)
  ```

- **Custom iterators** let you encapsulate complex crawling patterns.
- **Turning your scraper into a package** with a `setup.py` or `pyproject.toml` makes sharing and versioning easier.

## Bridging to BeautifulSoup

Everything above lays the groundwork for parsing HTML:

- Use your `fetch` functions to grab `resp.text`.
- Clean and normalize strings before searching.
- Pass HTML to BeautifulSoup and combine its navigation methods with Python loops, comprehensions, and exception handling.
- Collect parsed items into dicts or dataclasses for structured output.

With these Python fundamentals in place, you’re ready to dive into BeautifulSoup’s rich parsing API in the next chapter.