### getnameditem
```
*getnameditem(<item id>,"<name to inscribe>"|<char id>);
*getnameditem("<item name>","<name to inscribe>"|<char id>);
```

This function is equivalent to using 'getitem', however, it will not just give
the character an item object, but will also inscribe it with a specified
character's name. You may not inscribe items with arbitrary strings, only with
names of characters that actually exist. While this isn't said anywhere
specifically, apparently, named items may not have cards in them, slots or no -
these data slots are taken by the character ID who's name is inscribed. Only one
remains free and it's not quite clear if a card may be there.

This function will return 1 if an item was successfully created and 0 if it
wasn't for whatever reason. Like 'getitem', this function will also accept an
'english name' from the item database as an item name and will return 0 if no
such item exists.
