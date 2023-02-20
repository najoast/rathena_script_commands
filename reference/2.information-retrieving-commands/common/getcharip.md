### getcharip
```
*getcharip({"<character name>"|<account id>|<char id>})
```

This function will return the IP address of the invoking character, or, if a player
is specified, of that character. A blank string is returned if no player is attached.

Examples:

```c
// Outputs IP address of attached player.
mes "Your IP: " + getcharip();

// Outputs IP address of character "Silver".
mes "Silver's IP: " + getcharip("Silver");
```
