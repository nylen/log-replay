Parses log lines from Apache logs, including a mechanism to restart parsing
from a previous checkpoint.

Based on [`apache-log-parser`](https://github.com/rory/apache-log-parser).

Installation
============

Clone the repository to the same machine as the Apache log files.

Usage
=====

```
parse.py options

Options:
  -a, --after XYZ       Parse all recognized log entries after the given entry.
                        The value is a `continue_value` returned by this program
                        along with a previous entry, or 0 to parse all entries.

  -f, --files '*.log'   The log files to parse.  Make sure you quote this correctly
                        in your shell if it contains glob characters like *.
```

The program will send back log entries on standard output, each one formatted
as a JSON object on its own line.  Each entry has a `continue_value` key that
can be passed back to the program to retrieve all log entries **after** the
current entry.

Any errors will also be sent back on standard output, also formatted as JSON
objects.  Errors always have a truthy value (the error message) in the
`"error"` key of the object; non-errors never have this value.

Currently the only supported log format is the Apache2 `combined` log format
for access logs:

```
LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
```

Other formats are supported by the underlying library and can be added if needed.

Copyright
=========

The original `apache-log-parser` package is © 2013-2015 Rory McCann, released
under the terms of the GNU GPL v3 (or at your option a later version).

The new `log-replay` functions are copyright © 2019 James Nylen, also released
under the terms of the GNU GPL v3 (or at your option a later version).
