### failedremovecards
```
*failedremovecards <equipment slot>,<type>;
```

This command will remove all cards from the item found in the specified
equipment slot of the invoking character. 'type' determines what happens to the
item and the cards:

* 0 - will destroy both the item and the cards.
* 1 - will keep the item, but destroy the cards.
* 2 - will keep the cards, but destroy the item.

Whatever the type is, it will also show a failure effect on screen.
