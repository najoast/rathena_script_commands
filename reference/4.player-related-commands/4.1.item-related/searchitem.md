### searchitem
```
*searchitem <array name>,"<item name>";
```

This command will fill the given array with the ID of items whose name matches
the given one. It returns the number of items found. For performance reasons,
the results array is limited to 10 items.

```c
mes "What item are you looking for?";
input .@name$;
.@qty = searchitem(.@matches[0],.@name$);
mes "I found " + .@qty + " items:";
for (.@i = 0; .@i < .@qty; .@i++)
    // Display name (eg: "Apple[0]")
    mes getitemname(.@matches[.@i]) + "[" + getitemslots(.@matches[.@i]) + "]";
```
