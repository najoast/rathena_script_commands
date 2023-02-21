### delitem
```
*delitem <item id>,<amount>{,<account ID>};
*delitem "<item name>",<amount>{,<account ID>};
```

This command will remove a specified amount of items from the invoking/target character.
Like all the item commands, it uses the item ID found inside 'db/(pre-)re/item_db.txt'.

```c
delitem 502,10; // The person will lose 10 apples
delitem 617,1;  // The person will lose 1 Old Violet Box
```

It is always a good idea to check if the player actually has the items before you delete them.
If you try to delete more items that the player has, the player will lose the ones he/she has
and the script will terminate with an error.

Like 'getitem', this command will also accept an 'english name' field from the
database. If the name is not found, nothing will be deleted.
