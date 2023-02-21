### cartdelitem2
```
*cartdelitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*cartdelitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*storagedelitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*storagedelitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*guildstoragedelitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*guildstoragedelitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
```

This command behaves identically to 'delitem2', but deletes items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartdelitem2' will return -1.
If player is not in a guild or storage is open, 'guildstoragedelitem2' will return -1.
