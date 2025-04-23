# Beginner Soup

## Common BeautifulSoup Methods

| Method                       | Description                                    |
|------------------------------|------------------------------------------------|
| `soup.find(name, attrs)`     | First matching element                         |
| `soup.find_all(name, attrs)` | List of all matches                            |
| `soup.select(css_selector)`  | List of elements matching a CSS selector      |
| `element.get_text()`         | Extract inner text                             |
| `element.attrs`              | Dict of all attributes                         |
| `element["attr_name"]`       | Shortcut for a specific attribute (`href`, etc)|

## Example: Scraping Public Parks

Imagine a page at `https://example.gov/parks` lists parks in this structure:

```html
<div class="park-list">
    <div class="park">
        <h2>Riverside Park</h2>
        <p class="address">123 River Rd.</p>
        <p class="hours">Open: 6 AM–10 PM</p>
    </div>

    <div class="park">
        <h2>Lakeside Park</h2>
        <p class="address">123 Lake Rd.</p>
        <p class="hours">Open: 6 AM–10 PM</p>
    </div>

    <div class="park">
        <h2>Bay Park</h2>
        <p class="address">123 Bay Rd.</p>
        <p class="hours">Open: 6 AM–10 PM</p>
    </div>
</div>
```

A complete script to extract name, address, and hours from the hypothetical park example would look like this:

```python
import requests
from bs4 import BeautifulSoup
import json

BASE_URL = "https://example.gov/parks"

def scrape_parks(url):
resp = requests.get(url)
resp.raise_for_status()
soup = BeautifulSoup(resp.text, "html.parser")

parks = []
for div in soup.find_all("div", class_="park"):
    name    = div.find("h2").get_text(strip=True)
    address = div.find("p", class_="address").get_text(strip=True)
    hours   = div.find("p", class_="hours").get_text(strip=True)
    parks.append({"name": name, "address": address, "hours": hours})

return parks

def save_json(data, path="parks.json"):
with open(path, "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2, ensure_ascii=False)

if __name__ == "__main__":
data = scrape_parks(BASE_URL)
save_json(data)
```

- **`resp.raise_for_status()`** stops on HTTP errors.  
- We combine `find_all`, `find`, and `get_text()` for clean extraction.

## Unique & Advanced BeautifulSoup Features

Beyond basic searching and text extraction, BeautifulSoup offers powerful tools for navigating, filtering, and even modifying the parse tree.

### Navigating the Tree

- **`.parent` / `.parents`**  
    ```python
    tag = soup.find("span", class_="price")
    container = tag.parent            # direct parent
    for ancestor in tag.parents:      # walk up all ancestors
        print(ancestor.name)
    ```
- **`.next_sibling` / `.previous_sibling`**  
    ```python
    first = soup.find("h2")
    second = first.next_sibling       # might be a newline—use .find_next_sibling()
    second_heading = first.find_next_sibling("h2")
    ```
- **`.find_next()` / `.find_previous()`**  
    ```python
    # Find the next <p> after a header
    header = soup.find("h1")
    p = header.find_next("p")

    # Or in cases where there are multiple <p>
    section_paragraphs = []

    while p:
        section_paragraphs.append(p.get_text(strip=True))
        # move to the next sibling <p>; stops when next tag isn’t <p>
        p = p.find_next_sibling("p")

    # full section text is appended to section_paragraph list
    ```

## Unicode Normalization

When scraping data from the web, you might run into characters that *look* identical but are encoded differently under the hood. For example, `"é"` could be a single composed character (`U+00E9`) or two characters: `e` + `´` (accent mark). This can break comparisons, searches, or dataset joins.

To avoid these headaches, normalize your text using Python's built-in `unicodedata` module.

### Example

```python
import unicodedata

# Original character: 'é'
# It can appear in two forms:
# - Composed: U+00E9
# - Decomposed: U+0065 (e) + U+0301 ( ́)

# Example strings
composed = "Café"                         # Likely NFC: single codepoint for é
decomposed = "Cafe\u0301"                # NFD: e +  ́ (combining acute)

print("Raw equality:", composed == decomposed)  # False

# Normalize to NFC (composed form)
nfc_composed = unicodedata.normalize("NFC", decomposed)
print("NFC equality:", composed == nfc_composed)  # True

# Normalize to NFD (decomposed form)
nfd_decomposed = unicodedata.normalize("NFD", composed)
print("NFD example:", [c for c in nfd_decomposed])  # ['C', 'a', 'f', 'e', '\u0301']

# NFKC (Compatibility Composition)
# Transforms characters with formatting (e.g., subscript, superscript, Roman numerals)
compat_str = "① Ⅳ ²"  # Circled one, Roman numeral four, superscript two
nfkc = unicodedata.normalize("NFKC", compat_str)
print("NFKC:", nfkc)  # '1 IV 2'

# NFKD (Compatibility Decomposition)
# Breaks down characters and removes formatting
nfkd = unicodedata.normalize("NFKD", compat_str)
print("NFKD:", [c for c in nfkd])  # ['1', ' ', 'I', 'V', ' ', '2']
```

### Normalization Forms

| Form   | Description                                 |
|--------|---------------------------------------------|
| `NFC`  | Composes characters to a single code point  |
| `NFD`  | Decomposes into base characters + marks     |
| `NFKC` | Compatibility compose (also reformats text) |
| `NFKD` | Compatibility decompose                     |

In most scraping or NLP workflows, use `NFC` unless you have a specific reason to decompose.

### When to Use This

- Comparing scraped strings to reference datasets
- Cleaning data before database insertion
- Avoiding false negatives in string matching

### Reference

- [Unicode Normalization Forms – Unicode Consortium](https://unicode.org/reports/tr15/)
- [Python `unicodedata` Docs](https://docs.python.org/3/library/unicodedata.html)

---

**Output**
```
1: Some textwith invisible charsand other⁠weird stuff.
2: Some text with invisible chars and other⁠weird stuff.
3: Some textwith invisible charsand other⁠weird stuff.
4: Sometextwithinvisiblecharsand otherweirdstuff.
```

### Filtering & Search Strategies

- **`SoupStrainer`** (parse‑time filtering)  
    ```python
    from bs4 import SoupStrainer
    only_links = SoupStrainer("a")
    soup = BeautifulSoup(html, "lxml", parse_only=only_links)
    ```

### Modifying the Tree

- **Creating new tags & strings**  
    ```python
    new_div = soup.new_tag("div", **{"class": "note"})
    new_div.string = "This is a generated note."
    ```
- **Inserting & replacing**  
    ```python
    target = soup.find(id="main-content")
    target.insert_after(new_div)
    old_tag.replace_with(new_div)
    ```
- **Removing content**  
    ```python
    ad = soup.find("div", class_="advertisement")
    ad.decompose()    # remove tag and contents
    comment.extract() # remove but keep the Comment object
    ```
- **Unwrapping vs extracting**  
    ```python
    # Remove tag but keep its children
    wrapper = soup.find("span", class_="wrapper")
    wrapper.unwrap()
    ```

### Working with Text

- **`.stripped_strings`**  
    ```python
    for text in soup.get_text(strip=True).split():
        print(text)
    # or, per-element:
    for piece in tag.stripped_strings:
        print(piece)
    ```
- **`.get_text(separator="|")`**  
Control how nested text is joined:
    ```python
    all_text = tag.get_text(separator=" | ")
    ```

### Output & Encoding

    - **`.prettify()`**  
    ```python
    print(soup.prettify())
    ```
- **`.encode()` / `.decode()`**  
    ```python
    html_bytes = soup.encode("utf-8")
    html_unicode = html_bytes.decode("utf-8")
    ```

### Parser Choices

- **`"html.parser"`**: built‑in, no extra install.  
- **`"lxml"`**: very fast, requires `lxml` library.  
- **`"html5lib"`**: most forgiving, creates valid HTML5 tree.

    ```python
    soup         = BeautifulSoup(html, "html.parser")
    soup_lxml    = BeautifulSoup(html, "lxml")
    soup_html5   = BeautifulSoup(html, "html5lib")
    ```

---

## Further Reading
- [Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Real Python: Web Scraping With Beautiful Soup](https://realpython.com/beautiful-soup-web-scraper-python/)
- [Python Requests + BeautifulSoup Guide](https://realpython.com/python-requests/)