### query_sql
```
*query_sql("your MySQL query"{, <array variable>{, <array variable>{, ...}}});
```
### query_logsql
```
*query_logsql("your MySQL query"{, <array variable>{, <array variable>{, ...}}});
```

Executes an SQL query. A 'select' query can fill array variables with up to 2 billion rows of
values, and will return the number of rows (i.e. array size) or -1 on failure.

Note that 'query_sql' runs on the main database while 'query_logsql' runs on the log database.

Example:
```c
.@nb = query_sql("select name,fame from `char` ORDER BY fame DESC LIMIT 5", .@name$, .@fame);
mes "Hall Of Fame: TOP5";
mes "1." + .@name$[0] + "(" + .@fame[0] + ")"; // largest fame value.
mes "2." + .@name$[1] + "(" + .@fame[1] + ")";
mes "3." + .@name$[2] + "(" + .@fame[2] + ")";
mes "4." + .@name$[3] + "(" + .@fame[3] + ")";
mes "5." + .@name$[4] + "(" + .@fame[4] + ")";
```