### cartcountitem
```
*cartcountitem(<item id>{,<accountID>})
*cartcountitem("<item name>"{,<accountID>})
*storagecountitem(<item id>{,<accountID>})
*storagecountitem("<item name>"{,<accountID>})
*guildstoragecountitem(<nameID>{,<accountID>})
*guildstoragecountitem("<item name>"{,<accountID>})
```

This command behaves identically to 'countitem', but counts items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartcountitem' will return -1.
If player is not in a guild or storage is open, 'guildstoragecountitem' will return -1.
