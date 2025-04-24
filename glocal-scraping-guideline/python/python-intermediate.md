# Intermediate Python

## Control Flow: Loops and Conditionals

Scrapers loop over multiple pages and make decisions:

```python
# For example, 10 similar pages at example.gov/page/1 through example.gov/page/10
for page in range(1, 11): # range(start, stop-before) ==> 1 through 10.
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

## Functions & Modularity
You will quickly realize that parts of your program are repeating themselves - functions help minimize repetition, and allow for easy maintenance and debugging - in any code you write, knowing how to write a clearly defined and simply designed function is paramount.

A useful approach is 'top-down design' - identify the problem, and figure out how it can be broken down into several smaller problems. Take this script for example:

```python
import requests
from bs4 import BeautifulSoup

resp = requests.get("https://example.gov/data", headers=None)
resp.raise_for_status()
raw = resp.text
soup = BeautifulSoup(raw, "lxml")
data = data
print(data)
```
This script functions as intended - it is however, very hard to look at.

Going line by line, we can see that there are four tasks being completed in the script:
- `imports`: importing requests and beautifulsoup
- `fetching`: using requests to connect to a url, and retrieve the data
- `parsing`: using beautifulsoup to parse the raw data
- `output`: printing the raw data

So why not isolate each task?

Imports do not need to be wrapped in a function, they are almost always declared in the first line of the script.

Fetching, Parsing are the two 'main' components of the script - two unique steps to producing the output - they can be wrapped in their own respective functions.

Unlike direct commands, functions need to be called to execute - this is where the two final steps come in:
```python
def main():
    # call functions, pass arguments, etc here
    pass

if __name__ == "__main__": # a top level execution statement - not neccessary, but best-practice
    main()
```
When you run this script, the `if __name__ == "__main__":` guard kicks off our single entry point—`main()`. Inside `main()`, we simply call `fetch()` to grab the HTML, hand it off to `parse()`, then print the result. By organizing code this way, each step stays small and self‑contained, and `main()` reads like a concise roadmap of the program's flow.

Combining the above, we get a script that looks like this:

```python
import requests
from bs4 import BeautifulSoup

def fetch(url, headers=None):
    resp = requests.get(url, headers=headers)
    resp.raise_for_status()
    return resp.text

def parse(html):
    soup = BeautifulSoup(html, "lxml")
    return data

def main():
    raw = fetch("https://example.gov/data")
    data = parse(raw)
    print(data)
    pass

if __name__ == "__main__":
    main()
```


## List Comprehensions & Generators

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