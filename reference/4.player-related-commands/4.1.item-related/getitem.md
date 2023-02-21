### getitem
```
*getitem <item id>,<amount>{,<account ID>};
*getitem "<item name>",<amount>{,<account ID>};
```

This command will give an amount of specified items to the invoking character.
If an optional account ID is specified, and the target character is currently
online, items will be created in their inventory instead. If they are not
online, nothing will happen.

In the first and most commonly used version of this command, items are
referred to by their database ID number found inside 'db/(pre-)re/item_db.txt'.

```c
getitem 502,10 // The person will receive 10 apples
getitem 617,1  // The person will receive 1 Old Violet Box
```

This transaction is logged if the log script generated transactions option is
enabled.

You may also create an item by its name in the 'english name' field in the
item database:

```c
getitem "RED_POTION",10;
```

Which will do what you'd expect. If it can't find that name in the database,
apples will be created anyway. It is often a VERY GOOD IDEA to use it like this.

This is used in pretty much all NPC scripts that have to do with items and
quite a few item scripts. For more examples check just about any official script.
