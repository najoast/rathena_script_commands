### delitem2/delitem3
```
*delitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*delitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*delitem3 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
*delitem3 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
```

This command will remove a specified amount of items from the invoking/target character.
See 'getitem2' for an explanation of the expanded parameters.

'delitem3' is advance version of 'delitem2' that also use Item Random Option as criteria.
* `<RandomIDArray>`    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
* `<RandomValueArray>` : Array variable of item random option's value.
* `<RandomParamArray>` : Array variable of item random option's param.
