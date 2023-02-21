### preg_match
```
*preg_match(<regular expression pattern>,<string>{,<offset>})
```

Searches a string for a match to the regular expression provided. The
offset parameter indicates the index of the string to start searching.
Returns offsets to captured substrings, or 0 if no match is found.

This command is only available if the server is compiled with the regular
expressions library enabled.
