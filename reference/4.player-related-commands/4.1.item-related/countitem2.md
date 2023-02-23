### countitem2
```
*countitem2(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
*countitem2("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
*countitem3(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<accountID>})
*countitem3("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<accountID>})
```

Expanded version of 'countitem' function, used for created/carded/forged items.

This function will return the number of items for the specified item ID and
other parameters that the invoking character has in the inventory.
See 'getitem2' for an explanation of the expanded parameters.

'countitem3' is advance version of 'countitem2' that also use Item Random Option as criteria.
* `<RandomIDArray>`    : Array variable of ID for item random option, see `db/[pre-]re/item_randomopt_db.txt`
* `<RandomValueArray>` : Array variable of item random option's value.
* `<RandomParamArray>` : Array variable of item random option's param.
