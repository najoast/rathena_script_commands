### atoi
```
*atoi("<string>")
```
### axtoi
```
*axtoi("<string>")
```
### strtol
```
*strtol("<string>", base)
```

These commands are used to convert strings to numbers. 'atoi' will interpret
given string as a decimal number (base 10), while 'axtoi' interprets strings as
hexadecimal numbers (base 16). 'strtol' lets the user specify a base (valid range
is between 2 and 36 inclusive, or the special value0, which means auto-detection).

The 'atoi' and 'strtol' functions conform to the C functions with the same names,
and 'axtoi' is the same as strtol, with a base of 16. Results are clamped to signed
32 bit int range (INT_MIN ~ INT_MAX).

Examples:

```c
.@var = atoi("11");        // Sets .@var to 11
.@var = axtoi("FF");       // Sets .@var to 255
mes axtoi("11");           // Displays 17 (1 = 1, 10 = 16)
.@var = strtol("11", 10);  // Sets .@var to 11 (11 base 10)
.@var = strtol("11", 16);  // Sets .@var to 17 (11 base 16)
.@var = strtol("11", 0);   // Sets .@var to 11 (11 base 10, auto-detected)
.@var = strtol("0x11", 0); // Sets .@var to 17 (11 base 16, auto-detected because of the "0x" prefix)
.@var = strtol("011", 0);  // Sets .@var to 9 (11 base 8, auto-detected because of the "0" prefix)
.@var = strtol("11", 2);   // Sets .@var to 3 (binary 11)
```
