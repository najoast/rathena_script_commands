### consumeitem
```
*consumeitem <item id>{,<char_id>};
*consumeitem "<item name>"{,<char_id>};
```

This command will run the item script of the specified item on the invoking
character. The character does not need to possess the item, and the item will
not be deleted. While this command is intended for usable items, it will run
for any item type.

This command does not currently work with the 'itemskill' script command.
