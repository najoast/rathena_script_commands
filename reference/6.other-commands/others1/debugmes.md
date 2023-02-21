### debugmes
```
*debugmes "<message>";
```

This command will send the message to the server console (map-server window). It
will not be displayed anywhere else.

```c
// Displays "NAME has clicked me!" in the map-server window.
debugmes strcharinfo(0) + " has clicked me!";
```
