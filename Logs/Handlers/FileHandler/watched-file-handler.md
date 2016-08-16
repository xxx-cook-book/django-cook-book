# WatchedFileHandler

## WatchedFileHandler

_New In Python 2.6_

The [`WatchedFileHandler`](https://docs.python.org/2/library/logging.handlers.html#logging.handlers.WatchedFileHandler) class, located in the [`logging.handlers`](https://docs.python.org/2/library/logging.handlers.html#module-logging.handlers) module, is a `FileHandler` which watches the file it is logging to. If the file changes, it is closed and reopened using the file name.

A file change can happen because of usage of programs such as *newsyslog* and *logrotate* which perform log file rotation. This handler, intended for use under Unix/Linux, watches the file to see if it has changed since the last emit. (A file is deemed to have changed if its device or inode have changed.) If the file has changed, the old file stream is closed, and the file opened to get a new stream.

This handler is not appropriate for use under Windows, because under Windows open log files cannot be moved or renamed - logging opens the files with exclusive locks - and so there is no need for such a handler. Furthermore, *ST_INO* is not supported under Windows; [`stat()`](https://docs.python.org/2/library/os.html#os.stat) always returns zero for this value.

```python
class logging.handlers.WatchedFileHandler(filename[, mode[, encoding[, delay]]])
```

## LOGGING

```python

```

## References

[1] Docs@Python, [15.9. logging.handlers — Logging handlers — 15.9.4. WatchedFileHandler](https://docs.python.org/2/library/logging.handlers.html#watchedfilehandler)