### countitem
```
*countitem(<item id>{,<accountID>})
*countitem("<item name>"{,<accountID>})
```

This function will return the number of items for the specified item ID that the
invoking character has in the inventory.

```c
mes "[Item Checker]";
mes "Hmmm, it seems you have " + countitem(502) + " apples";
close;
```

Like 'getitem', this function will also accept an 'english name' from the
database as an argument.

If you want to state the number at the end of a sentence, you can do it by
adding up strings:

```c
mes "[Item Checker]";
mes "Hmmm, the total number of apples you are holding is " + countitem("APPLE");
close;
```
