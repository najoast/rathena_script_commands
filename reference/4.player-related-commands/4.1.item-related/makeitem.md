### makeitem
```
*makeitem <item id>,<amount>,"<map name>",<X>,<Y>;
*makeitem "<item name>",<amount>,"<map name>",<X>,<Y>;
```

This command will create an item on the specified cell of a map.

As with any dropped items, the items created with this command will disappear after
a period of time. Using an amount greater than 1 will create a single stack of the
given amount, not multiple stacks of 1.

Like 'getitem', it also accepts an 'english name' field from the database and creates
Apples if the name isn't found.
If the map name is given as "this", the map the invoking character is on will be used.
