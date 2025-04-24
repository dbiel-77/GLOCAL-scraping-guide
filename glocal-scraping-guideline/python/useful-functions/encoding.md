# Encoding

When opening text files, especially ones with non-English characters (like accents or symbols), it’s important to specify an **encoding**. Without it, Python may default to a system-specific encoding (like `cp1252` on Windows), which can cause weird characters or outright errors when reading or writing.

The safest, most universal choice? `encoding="utf-8"`

### Why `utf-8`?
- It supports **all Unicode characters** (i.e., most languages and symbols)
- It’s the **standard encoding for the web and modern systems**
- It avoids platform-specific issues

### Example

```python
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()
```