# Enumerate: `enumerate()`

When looping through a list and you need both the **index** and the **item**, don’t use `range(len(...))`. Use `enumerate()`—it’s cleaner and more Pythonic.

```python
names = ["Alice", "Bob", "Charlie"]

for idx, name in enumerate(names, start=1):
    print(f"{idx}: {name}")

############## OUTPUT ####################
1: Alice
2: Bob
3: Charlie
##########################################
```

This makes your loops easier to read and removes the need to manually manage counters.

Read more in the [`enumerate()` documentation](https://docs.python.org/3/library/functions.html#enumerate).