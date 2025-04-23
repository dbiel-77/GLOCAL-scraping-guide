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

## Pythonic Principles

Python is special because it's a **verbose** and **human-centric** language. It was designed with the philosophy that **code is read more often than it's written**, and it's *humans*, not machines, who will be reading, writing, and maintaining it.

So, while computers ultimately understand only binary (1s and 0s), **Python embraces readability and simplicity**, making it closer to natural language than most programming languages.

### What Does It Mean to Write Pythonic Code?

Writing "Pythonic" code means writing code that adheres to the idioms, principles, and style guidelines embraced by the Python community. It's not just about getting the job done — it's about doing it *the Python way*.

### Characteristics of Pythonic Code

| Trait                  | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **Readable**           | Prioritizes clarity and easy understanding over cleverness or brevity      |
| **Concise**            | Avoids unnecessary repetition or verbosity                                  |
| **Explicit**           | Prefers clear behavior to implicit tricks or surprises                      |
| **Simple > Complex**   | Strives to solve problems in the simplest reasonable way                    |
| **Elegant**            | Uses built-in features and idioms instead of over-engineering solutions     |
| **Consistent**         | Follows the conventions of [PEP 8](https://peps.python.org/pep-0008/)       |
| **Leverages Python's Standard Library** | Uses "batteries included" modules rather than reinventing the wheel   |

### Pythonic vs Unpythonic

```python
# Unpythonic: verbose and manual
squares = []
for i in range(10):
    squares.append(i * i)

# Pythonic: concise and readable
squares = [i * i for i in range(10)]
```

```python
# Unpythonic: using index unnecessarily
names = ["Alice", "Bob", "Charlie"]
for i in range(len(names)):
    print(names[i])

# Pythonic: iterate directly
for name in names:
    print(name)
```

### Key Tools That Help You Write Pythonically

- **List/Dict/Set comprehensions**
- **Context managers** (`with open(...) as f:`)
- **Unpacking** (`a, b = b, a`)
- **Enumerate and zip** instead of `range(len(...))`
- **Built-in functions** like `map()`, `filter()`, `sorted()`, `any()`, `all()`, `sum()`

### Zen of Python (by Tim Peters)
You can read it in the python interpreter by typing:
```bash
>>> import this
```

### In Practice

Pythonic code isn’t just about style — it reduces bugs, improves maintainability, and makes your programs a joy to work on for others and your future self. Aim for **elegance through simplicity**.

```{note}
If your code "feels right" to read aloud, you're probably on the right track.
```

## Basic Python Operators and Logic Table

| **Category**     | **Operator** | **Description**                              | **Example**                   |
|------------------|--------------|----------------------------------------------|-------------------------------|
| Arithmetic       | `+`          | Addition                                     | `5 + 2  # 7`                  |
|                  | `-`          | Subtraction                                  | `5 - 2  # 3`                  |
|                  | `*`          | Multiplication                               | `5 * 2  # 10`                 |
|                  | `/`          | Division (float result)                      | `5 / 2  # 2.5`                |
|                  | `//`         | Floor Division (rounds down)                 | `5 // 2  # 2`                 |
|                  | `%`          | Modulus (remainder)                          | `5 % 2  # 1`                  |
|                  | `**`         | Exponentiation                               | `2 ** 3  # 8`                 |
| Comparison       | `==`         | Equal to                                     | `5 == 5  # True`              |
|                  | `!=`         | Not equal to                                 | `5 != 3  # True`              |
|                  | `>`          | Greater than                                 | `5 > 3  # True`               |
|                  | `<`          | Less than                                    | `5 < 3  # False`              |
|                  | `>=`         | Greater than or equal to                     | `5 >= 5  # True`              |
|                  | `<=`         | Less than or equal to                        | `3 <= 5  # True`              |
| Logical          | `and`        | True if both are True                        | `True and False  # False`     |
|                  | `or`         | True if at least one is True                 | `True or False  # True`       |
|                  | `not`        | Inverts the Boolean value                    | `not True  # False`           |
| Membership       | `in`         | True if value is in a collection             | `'a' in 'cat'  # True`         |
|                  | `not in`     | True if value is not in a collection         | `'z' not in 'cat'  # True`     |
| Identity         | `is`         | True if same object in memory                | `a is b`                      |
|                  | `is not`     | True if not same object in memory            | `a is not b`                  |

```{note}
Tips:
- Comparison and logical operators are often used in `if` statements.
- Use parentheses `()` to control order of evaluation in complex expressions.

Resources:
- [Python Arithmetic Operators](https://docs.python.org/3/library/operator.html#arithmetic-operators)
- [Python Logical Operators](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)
```

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
- **Floating Point** (`float`): a floating decimal/point - for example, 3.5
- **Booleans** (`bool`): `True` or `False`
- **Lists** (`list`): ordered sequences
- **Tuples** (`tuple`): immutable sequences
- **Dictionaries** (`dict`): key–value mappings

Python automatically sets a data type when you assign a variable, for example:

```bash
python3
>>> x = 1 # int
>>> y = 1.5 # float
>>> x + y # python interprets 1 as 1.0
2.5
```
If you try to perform a type-restricted operation on the wrong type, you will receive a TypeError:

```bash
python3
>>> x = "1" # string
>>> y = 1 # int
>>> x + y # the character "1" + the number 1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```
When you are scraping raw data from a website, it will **ALWAYS** be collected as a `str`. If for example, you scrape a number that will be used in a calculation (i.e. scraping population data from  a table), you can specify the typing with constructors:

```python
x = "1"
x = x*5 # "11111"

x = int(x)
x = x*5 # 5

s1 = "GLOCAL"
s2 = "1234"
i = 1
f = 2.5
lst = ["GLOCAL", "Foundation", "of", "Canada"]
dictionary = {"Name": "Daniel", "Location": "Mars"}

# Convert string to integer
int_s2 = int(s2)  # 1234

# Convert integer to float
float_i = float(i)  # 1.0

# Convert float to integer (truncates)
int_f = int(f)  # 2

# Convert integer to string
str_i = str(i)  # "1"

# Convert list to string (joined with space)
joined_list = " ".join(lst)  # "GLOCAL Foundation of Canada"

# Convert string to list (of characters)
list_s1 = list(s1)  # ['G', 'L', 'O', 'C', 'A', 'L']

# Convert dictionary keys to list
dict_keys = list(dictionary.keys())  # ['Name', 'Location']

# Convert dictionary values to list
dict_values = list(dictionary.values())  # ['Daniel', 'Mars']

# Convert list of tuples to dictionary
tuples = [("a", 1), ("b", 2)]
tuple_to_dict = dict(tuples)  # {'a': 1, 'b': 2}

# Convert integer to binary, hexadecimal, and octal
bin_i = bin(i)  # '0b1'
hex_i = hex(i)  # '0x1'
oct_i = oct(i)  # '0o1'

# Evaluate string as Python expression (use with caution)
expr = "3 + 4"
eval_expr = eval(expr)  # 7

# Safely convert unknown type to string by creating a new variable
safe_str = str(dictionary)  # "{'Name': 'Daniel', 'Location': 'Mars'}"

# Convert string to list of words
sentence = "Web scraping is cool"
split_sentence = sentence.split() 
```

## Working with Strings

Data comes in all shapes and configurations on the web - you will inevitably run into cases where text isnt displayed in a format that matches the receiving database. Python’s string methods make this easy:

```python
text = "   Hello, World!  \n"
clean = text.strip()                   # "Hello, World!"
lower = clean.lower()                  # "hello, world!"
parts = clean.split(",")               # ["Hello", " World!"]
joined = "-".join([p.strip() for p in parts]) # "Hello-World!"    
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
## Text Editors

Now that you have had some time to play with python in the command line, it’s worth investing a little time in a dedicated text editor. A good editor gives you syntax highlighting, bracket matching, code snippets, and an integrated terminal or debugger—all of which save you from hunting through lines of plain text to spot errors. Instead of juggling multiple windows and copy‑pasting, you can write, test, and refactor your code in one place—making your workflow smoother and less error‑prone.


| Editor                   | Key Strengths                                                                 | Link                                     |
|--------------------------|-------------------------------------------------------------------------------|------------------------------------------|
| **Visual Studio Code**   | • Integrated terminal & debugger<br>• Rich extension ecosystem<br>• Built‑in Git support<br>• Best for beginners | [code.visualstudio.com](https://code.visualstudio.com/) |
| **Sublime Text**         | • Ultra‑lightweight & blazing fast<br>• “Goto Anything” fuzzy finder<br>• Powerful multi‑selection editing<br>• Package Control | [www.sublimetext.com](https://www.sublimetext.com/)   |
| **Atom**                 | • Highly hackable (built on Electron)<br>• GitHub integration<br>• Teletype for real‑time collaboration | [atom.io](https://atom.io/)             |
| **Vim**                  | • Modal, keyboard‑driven editing<br>• Very low resource usage<br>• Ubiquitous on Unix systems<br>• Steep learning curve but massive speed gains | [www.vim.org](https://www.vim.org/)     |
| **Emacs**                | • Infinitely extensible via Emacs Lisp<br>• Integrated shell, mail, calendar<br>• Org‑mode for notes/tasks | [www.gnu.org/software/emacs/](https://www.gnu.org/software/emacs/) |

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

### Match Statements (Structural Pattern Matching)

Introduced in Python 3.10, `match` is similar to switch/case in other languages but far more powerful. It allows for **pattern matching** on complex data types, not just integers or strings.

Example:

```python
def handle_response(code):
    match code:
        case 200:
            return "OK"
        case 404:
            return "Not Found"
        case 500:
            return "Server Error"
        case _:
            return "Unknown Status"
```

Patterns can even match dictionaries, tuples, or classes. Check out the full [`match` statement documentation](https://docs.python.org/3/reference/compound_stmts.html#the-match-statement) to see how deep the rabbit hole goes.

---

### Logging

```{admonition} 
:class: tip
Use logging instead of print for all diagnostics and runtime information that isn't meant for the user.
```

Python’s built-in [`logging`](https://docs.python.org/3/library/logging.html) module provides a flexible, standardized way to report messages from your application. Unlike `print()`, logging allows you to categorize messages by severity (e.g., DEBUG, INFO, WARNING, ERROR, CRITICAL), direct them to different outputs, and include timestamps or other metadata.

Here’s a simple example of idiomatic usage:

```python
# myapp.py
import logging
import mylib
logger = logging.getLogger(__name__)

def main():
    logging.basicConfig(filename='myapp.log', level=logging.INFO)
    logger.info('Started')
    mylib.do_something()
    logger.info('Finished')

if __name__ == '__main__':
    main()
```
Or in a request example:

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

Using `logger` instead of `print()` gives you more control and scales better for larger projects or debugging workflows.

---

### Another Cool Built-in: `enumerate()`

When looping through a list and you need both the **index** and the **item**, don’t use `range(len(...))`. Use `enumerate()`—it’s cleaner and more Pythonic.

```python
names = ["Alice", "Bob", "Charlie"]

for idx, name in enumerate(names, start=1):
    print(f"{idx}: {name}")
```

This makes your loops easier to read and removes the need to manually manage counters.

Read more in the [`enumerate()` documentation](https://docs.python.org/3/library/functions.html#enumerate).


With these Python fundamentals in place, you’re ready to dive into BeautifulSoup’s rich parsing API in the next chapter.