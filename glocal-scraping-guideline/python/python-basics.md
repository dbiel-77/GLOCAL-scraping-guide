# Python Foundations for Web Scraping

Python is an open‑source, interpreted language that combines clear, readable syntax with a “batteries‑included” standard library—everything from HTTP clients to JSON parsers is ready to go out of the box. It supports multiple paradigms (procedural, object‑oriented, functional), automatic memory management, and an enormous ecosystem of third‑party packages. That makes Python both easy to learn and incredibly powerful for tasks like web scraping, data analysis, automation, and more.

## **Installing Python & getting started**  
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

```{tip}
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

```{tip}
Tips:
- Comparison and logical operators are often used in `if` statements.
- Use parentheses `()` to control order of evaluation in complex expressions.
```

Further Reading:
- [Python Arithmetic Operators](https://docs.python.org/3/library/operator.html#arithmetic-operators)
- [Python Logical Operators](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)

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

>>> x + y # python interprets 1 as 1.0 || 1.0 + 1.5
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
sentence = "This is the first time that Daniel has written a book"

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
split_sentence = sentence.split()  #['This', 'is', 'the', 'first', 'time', 'that', 'Daniel', 'has', 'written', 'a', 'book']
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

## Data Structures

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

```{tip}
## Further Reading
- [Official Python Tutorial](https://docs.python.org/3/tutorial/)
- [Real Python: Python Basics](https://realpython.com/python-basics/)
- [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)
```