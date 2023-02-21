### escape_sql
```
*escape_sql(<value>)
```

Converts the value to a string and escapes special characters so that it is safe to
use in query_sql(). Returns the escaped form of the given value.

Example:
```c
.@name$ = "John's Laptop";
.@esc_str$ = escape_sql(.@name$); // Escaped string: John\'s Laptop
```
