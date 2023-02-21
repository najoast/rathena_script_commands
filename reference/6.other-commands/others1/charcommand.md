### charcommand
```
*charcommand "<command>";
```

This command will run the given command line exactly as if it was typed in from
the keyboard from a character that belonged to an account which had GM level 99.

The commands can also run without an attached rid.

```c
// This would do the same as above, but now
// it doesn't need a player attached by default.
charcommand "#option 0 0 0 Roy";
```
