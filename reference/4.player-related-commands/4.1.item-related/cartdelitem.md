### cartdelitem
```
*cartdelitem <item id>,<amount>{,<account ID>};
*cartdelitem "<item name>",<amount>{,<account ID>};
*storagedelitem <item id>,<amount>{,<account ID>};
*storagedelitem "<item name>",<amount>{,<account ID>};
*guildstoragedelitem <item id>,<amount>{,<account ID>};
*guildstoragedelitem "<item name>",<amount>{,<account ID>};
```

This command behaves identically to 'delitem', but deletes items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartdelitem' will return -1.
If player is not in a guild or storage is open, 'guildstoragedelitem' will return -1.
