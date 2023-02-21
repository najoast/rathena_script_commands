### needed_status_point
```
*needed_status_point(<type>,<val>{,<char id>});
```

Returns the number of stat points needed to change the specified stat `<type>` by `<val>`.
If `<val>` is negative, returns the number of stat points that would be needed to
raise the specified stat from (current value - `<val>`) to current value.
