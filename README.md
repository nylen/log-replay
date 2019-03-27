Parses log lines from Apache logs, including a mechanism to restart parsing
from a previous checkpoint.

Based on [`apache-log-parser`](https://github.com/rory/apache-log-parser).

Installation
============

Clone the repository to the same machine as the Apache log files.

Usage
=====

Run `parse.py`.  It will wait for a command on standard input.  Commands are
JSON objects passed on their own line.

- `{"files":"/var/log/apache2/*.log*","after":0}` - parse all recognized Apache
  log entries.
- `{"files":"/var/log/apache2/*.log*","after":"continue_value"}` - parse all
  recognized Apache log entries after a given entry.  An opaque
  `continue_value` is returned with each log record, and this value can be
  passed back to the program in subsequent runs to parse only new records.

After receiving a valid command, the program will send back log entries on
standard output, each one formatted as a JSON object on its own line.  Any
errors will be sent back on standard error, also formatted as JSON objects.

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
