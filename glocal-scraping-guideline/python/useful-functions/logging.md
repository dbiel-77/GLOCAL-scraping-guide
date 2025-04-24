# Logging

```{admonition} 
:class: tip
Use logging instead of print for all diagnostics and runtime information that isn't meant for the user.
```

Python’s built-in [`logging`](https://docs.python.org/3/library/logging.html) module provides a flexible, standardized way to report messages from your application. Unlike `print()`, logging allows you to categorize messages by severity (e.g., DEBUG, INFO, WARNING, ERROR, CRITICAL), direct them to different outputs, and include timestamps or other metadata.

Here’s a simple example of idiomatic usage:

```python
# mylib.py
import logging
logger = logging.getLogger(__name__)

def do_something():
    logger.info('Doing something')

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

############## OUTPUT ####################
INFO:__main__:Started
INFO:mylib:Doing something
INFO:__main__:Finished
##########################################
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

############## OUTPUT ####################
ERROR:__main__:Failed to fetch http://example.com: 404 Client Error: Not Found for url: 'http://example.com'
##########################################

```
Below is a quick overview of Python’s built-in logging levels. The `logging` module uses these constants to filter and route messages—each level has a numeric value and a standard meaning so you can control exactly what gets recorded or displayed.

| Level Name            | Value | Meaning                                                                                                                                             |
|-----------------------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `logging.NOTSET`      | 0     | Inherited from parent loggers; if still NOTSET, all events are logged. On handlers, all events are handled.                                         |
| `logging.DEBUG`       | 10    | Detailed diagnostic information, mainly useful when you’re troubleshooting or developing.                                                            |
| `logging.INFO`        | 20    | General confirmation that things are working as expected.                                                                                            |
| `logging.WARNING`     | 30    | Something unexpected happened, or might happen soon (e.g., “disk space low”). The program continues to run normally.                                 |
| `logging.ERROR`       | 40    | A more serious problem occurred and a function could not complete.                                                                                   |
| `logging.CRITICAL`    | 50    | A very serious error—often one that will prevent the program from continuing to run.                                                                 |

