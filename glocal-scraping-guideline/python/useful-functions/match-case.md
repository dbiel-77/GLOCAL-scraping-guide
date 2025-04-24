# Match Statements (Structural Pattern Matching)

Introduced in Python 3.10, `match` is similar to switch/case in other languages but far more powerful. It allows for **pattern matching** on complex data types, not just integers or strings.

## Example:

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
