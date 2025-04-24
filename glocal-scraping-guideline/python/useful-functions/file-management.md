# Context Managers & File I/O

## Reading  
When reading the contents of a file—such as a CSV—it is best practice to do so within a `with` statement. Imagine that you want to read a book: you take it, open it to a page, read its contents. Do you leave it open on your desk when you're done? Probably not. The same goes for files. `with open() as file:` ensures that the file is **automatically closed** once the block is done, even if an error occurs. This is called using a **context manager**, and it's a clean, safe way to handle file input.

```python
with open("data.csv", "r", encoding="utf-8") as fp:
    for line in fp:
        print(line.strip())
```

## Writing  
Just as context managers help when reading, they’re equally valuable when writing. Without `with`, you'd need to remember to call `file.close()` yourself—which risks bugs or locked files if you forget. By using `with`, Python handles cleanup for you.

```python
with open("data.csv", "w", encoding="utf-8") as fp:
    writer = csv.writer(fp)
    for record in results:
        writer.writerow([record["name"], record["email"]])
```

Here, `"w"` opens the file for writing (overwriting if it already exists), and the context manager guarantees that all buffered data is flushed and the file is closed properly when the block finishes.

## File Open Modes: A Quick Reference

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

### Examples

- `open("file.txt", "r")`: open for reading text  
- `open("file.bin", "rb")`: open for reading binary  
- `open("file.txt", "w+")`: open for reading and writing (overwrites file)  
- `open("file.txt", "a")`: open for writing, appends to end if file exists  
