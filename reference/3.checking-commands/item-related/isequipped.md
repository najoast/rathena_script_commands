
### isequipped
```
*isequipped(<id>{,<id>{,<id>{,<id>}}})
```

This function will return 1 if the invoking character has all of the item
IDs given equipped (if card IDs are passed, then it checks if the cards are
inserted into slots in the equipment they are currently wearing). Theoretically
there is no limit to the number of items that may be tested for at the same time.
If even one of the items given is not equipped, 0 will be returned.

```c
// (Poring,Santa Poring,Poporing,Marin)
if (isequipped(4001,4005,4033,4196)) mes "Wow! You're wearing a full complement of possible poring cards!";
// (Poring)
if (isequipped(4001)) mes "A poring card is useful, don't you think?";
```

The function was meant for item scripts to support the cards released by Gravity
in February 2005, but it will work just fine in normal NPC scripts.
