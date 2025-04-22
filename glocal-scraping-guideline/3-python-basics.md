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
4. **Read the Hidden Poem**  
   ```bash
   python3
   # in python terminal
   >>> import this
   ```


## Text Editors

Before you start writing and running Python scripts from the command line or a basic text file, it’s worth investing a little time in a dedicated text editor. A good editor gives you syntax highlighting, bracket matching, code snippets, and an integrated terminal or debugger—all of which save you from hunting through lines of plain text to spot errors. Instead of juggling multiple windows and copy‑pasting, you can write, test, and refactor your code in one place—making your workflow smoother and less error‑prone.


| Editor                   | Key Strengths                                                                 | Link                                     |
|--------------------------|-------------------------------------------------------------------------------|------------------------------------------|
| **Visual Studio Code**   | • Integrated terminal & debugger<br>• Rich extension ecosystem<br>• Built‑in Git support<br>• Best for beginners | [code.visualstudio.com](https://code.visualstudio.com/) |
| **Sublime Text**         | • Ultra‑lightweight & blazing fast<br>• “Goto Anything” fuzzy finder<br>• Powerful multi‑selection editing<br>• Package Control | [www.sublimetext.com](https://www.sublimetext.com/)   |
| **Atom**                 | • Highly hackable (built on Electron)<br>• GitHub integration<br>• Teletype for real‑time collaboration | [atom.io](https://atom.io/)             |
| **Vim**                  | • Modal, keyboard‑driven editing<br>• Very low resource usage<br>• Ubiquitous on Unix systems<br>• Steep learning curve but massive speed gains | [www.vim.org](https://www.vim.org/)     |
| **Emacs**                | • Infinitely extensible via Emacs Lisp<br>• Integrated shell, mail, calendar<br>• Org‑mode for notes/tasks | [www.gnu.org/software/emacs/](https://www.gnu.org/software/emacs/) |


## Basic Python: Variables and Types

Python is dynamically typed. You can assign any value to a variable:

```python
# string
url = "https://example.gov/data"

# integer
retry_limit = 3 

# dictionary with key:value pairs
headers = {"User-Agent": "MyScraper/1.0"}
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

Data comes in all shapes and configurations on the web - you will inevitably run into cases where text isnt displayed in a format that matches the receiving database. Python’s string methods make this easy:

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
- Replace all whitespace in `"GLOCAL foundation of Canada"` with underscores.
- Turn `GLOCAL` into a list of the letters that make it up, in lowercase, output should be `['g', 'l', 'o', 'c', 'a', 'l']`

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

This is especially helpful when scraping large amounts of similar data - for example, the process for scraping federal election candidates used key/value assignment before appending each entry to a csv. Scraped values would be collected, cleaned, and assigned a variable before being aggregated into a dict:

```python
    candidate_record = {
        "name": full_name,
        "constituency": riding,
        "role": role,
        "affiliation": sel["affiliation"],
        "image_file": photo_url,
        "bio": bio_text,
        "socials": json.dumps(socials, ensure_ascii=False),
        "sources": json.dumps(sources, ensure_ascii=False)
    }
```

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

## Functions and Modularity

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

Python is special because it is a verbose language - it was designed with the understanding that it is humans who will be reading, writing, and maintaining python code - computers only speak in 1s and 0s. 

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

### Reading  
When reading the contents of a file—such as a CSV—it is best practice to do so within a `with` statement. Imagine that you want to read a book: you take it, open it to a page, read its contents. Do you leave it open on your desk when you're done? Probably not. The same goes for files. `with open() as file:` ensures that the file is **automatically closed** once the block is done, even if an error occurs. This is called using a **context manager**, and it's a clean, safe way to handle file input.

```python
with open("data.csv", "r", encoding="utf-8") as fp:
    for line in fp:
        print(line.strip())
```

### Writing  
Just as context managers help when reading, they’re equally valuable when writing. Without `with`, you'd need to remember to call `file.close()` yourself—which risks bugs or locked files if you forget. By using `with`, Python handles cleanup for you.

```python
with open("data.csv", "w", encoding="utf-8") as fp:
    writer = csv.writer(fp)
    for record in results:
        writer.writerow([record["name"], record["email"]])
```

Here, `"w"` opens the file for writing (overwriting if it already exists), and the context manager guarantees that all buffered data is flushed and the file is closed properly when the block finishes.

### File Open Modes: A Quick Reference

When using Python’s built-in `open()` function, you can specify how the file should be opened using **modes**. These modes control whether you're reading, writing, appending, or working with binary data.

| Mode | Stands For         | Description                                                                 |
|------|---------------------|-----------------------------------------------------------------------------|
| `'r'`  | **Read**             | Opens a file for reading (default). File must exist.                         |
| `'w'`  | **Write**            | Opens a file for writing. **Overwrites** if it exists, creates if it doesn’t. |
| `'a'`  | **Append**           | Opens a file for writing. Creates if not exists, **appends** to the end.     |
| `'x'`  | **Exclusive Create** | Creates a new file, fails if it already exists.                              |
| `'r+'` | **Read & Write**     | Opens a file for both reading and writing. File must exist.                  |
| `'w+'` | **Write & Read**     | Opens for reading and writing. Overwrites the file.                          |
| `'a+'` | **Append & Read**    | Opens for reading and appending. Creates file if it doesn’t exist.           |
| `'b'`  | **Binary**           | Add to mode for binary files (e.g., `'rb'`, `'wb'`).                         |
| `'t'`  | **Text**             | Default. Add to explicitly indicate text mode (e.g., `'rt'`, `'wt'`).       |

#### Examples

- `open("file.txt", "r")`: open for reading text  
- `open("file.bin", "rb")`: open for reading binary  
- `open("file.txt", "w+")`: open for reading and writing (overwrites file)  
- `open("file.txt", "a")`: open for writing, appends to end if file exists  

## Encoding: Handling Text the Right Way

When opening text files, especially ones with non-English characters (like accents or symbols), it’s important to specify an **encoding**. Without it, Python may default to a system-specific encoding (like `cp1252` on Windows), which can cause weird characters or outright errors when reading or writing.

The safest, most universal choice? `encoding="utf-8"`

#### Why `utf-8`?
- It supports **all Unicode characters** (i.e., most languages and symbols)
- It’s the **standard encoding for the web and modern systems**
- It avoids platform-specific issues

#### Example

```python
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()
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

With these Python fundamentals in place, you’re ready to dive into BeautifulSoup’s rich parsing API in the next chapter.