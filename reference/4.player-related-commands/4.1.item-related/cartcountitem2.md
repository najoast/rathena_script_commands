### cartcountitem2
```
*cartcountitem2(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
*cartcountitem2("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
*storagecountitem2(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
*storagecountitem2("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
*guildstoragecountitem2(<nameID>,<Identified>,<Refine>,<Attribute>,<Card0>,<Card1>,<Card2>,<Card3>{,<accountID>})
*guildstoragecountitem2("<item name>",<Identified>,<Refine>,<Attribute>,<Card0>,<Card1>,<Card2>,<Card3>{,<accountID>})
```

This command behaves identically to 'countitem2', but counts items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartcountitem2' will return -1.
If player is not in a guild or storage is open, 'guildstoragecountitem2' will return -1.
