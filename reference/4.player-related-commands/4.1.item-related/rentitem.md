### rentitem
```
*rentitem <item id>,<time>{,<account_id>};
*rentitem "<item name>",<time>{,<account_id>};
```

Creates a rental item in the attached character's inventory. The item will expire
in `<time>` seconds and be automatically deleted. When receiving a rental item,
the character will receive a message in their chat window. The character will
also receive warning messages in their chat window before the item disappears.

When rentals expire it will call the OnUnequip Script of the item. This can be used
for special cases such as removing a status change or resetting a variable or state
of the player.

This command can not be used to rent stackable items. Rental items cannot be
dropped, traded, sold to NPCs, or placed in guild storage. (i.e. trade mask 75)
Note: 'delitem' in an NPC script can still remove rental items.
