### getnameditem
```
*getnameditem <item id>,<character name|character ID>;
*getnameditem "<item name>",<character name|character ID>;
```

Create an item signed with the given character's name.

The command returns 1 when the item is created successfully, or 0 if it fails.
Failure occurs when:
- There is no player attached.
- Item name or ID is not valid.
- The given character ID/name is offline.

Example:

```c
//This will give the currently attached player a Aaron's Apple (if Aaron is online).
getnameditem "Apple","Aaron";

//Self-explanatory (I hope).
if (getnameitem("Apple","Aaron")) {
    mes "You now have a Aaron's Apple!";
}
```
