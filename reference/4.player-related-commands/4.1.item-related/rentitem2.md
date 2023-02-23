### rentitem2
```
*rentitem2 <item id>,<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account_id>};
*rentitem2 "<item name>",<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account_id>};
*rentitem3 <item id>,<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account_id>};
*rentitem3 "<item name>",<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account_id>};
```

Creates a rental item in the attached character's inventory. The item will expire
in `<time>` seconds and be automatically deleted. See 'rentitem' for further details.

See 'getitem2' for an explanation of the expanded parameters.

'rentitem3' is advance version of 'rentitem2' that also use Item Random Option as additional values.
* `<RandomIDArray>`    : Array variable of ID for item random option, see `db/[pre-]re/item_randomopt_db.txt`
* `<RandomValueArray>` : Array variable of item random option's value.
* `<RandomParamArray>` : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
```c
// +9 Crimson Dagger [2]
setarray .@OptID[0],RDMOPT_WEAPON_ATTR_TELEKINESIS;
setarray .@OptVal[0],0;
setarray .@OptParam[0],0;
rentitem3 28705,(24*60*60),1,9,0,0,0,0,0,.@OptID,.@OptVal,.@OptParam;
```
