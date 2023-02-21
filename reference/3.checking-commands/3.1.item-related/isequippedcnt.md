### isequippedcnt
```
*isequippedcnt(<card id>{,<card id>{,<card id>{,<card id>}}})
```

This function is similar to 'isequipped', but instead of 1 or 0, it will return
the number of cards in the list given that were found on the invoking character.

If a given parameter is not a card, the function returns the amount of that
item equipped on the invoking character.

```c
if (isequippedcnt(4001,4005,4033,4196) == 4)
    mes "Finally got all four poring cards?";
```
