### getitembound2
```
*getitembound2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>{,<account ID>};
*getitembound2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>{,<account ID>};
*getitembound3 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
*getitembound3 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
```

This command behaves identically to 'getitem2', but the items created will be
bound to the target character as specified by the bound type. All items created
in this manner cannot be dropped, sold, vended, auctioned, or mailed, and in
some cases cannot be traded or stored.

For a list of bound types see 'getitembound'.

'getitembound3' is advance version of 'getitembound2' that also use Item Random Option as additional values.
* `<RandomIDArray>`    : Array variable of ID for item random option, see `db/[pre-]re/item_randomopt_db.txt`
* `<RandomValueArray>` : Array variable of item random option's value.
* `<RandomParamArray>` : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
```c
// +9 Crimson Dagger [2]
setarray .@OptID[0],RDMOPT_WEAPON_ATTR_TELEKINESIS;
setarray .@OptVal[0],0;
setarray .@OptParam[0],0;
getitembound3 28705,1,1,9,0,0,0,0,0,BOUND_CHAR,.@OptID,.@OptVal,.@OptParam;
```
