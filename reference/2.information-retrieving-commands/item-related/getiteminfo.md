### getiteminfo
```
*getiteminfo(<item ID>,<type>)
```

This function will look up the item with the specified ID number in the database
and return the info set by TYPE argument.
It will return -1 if there is no such item.

```
Valid types are:
	0  - Buy Price
	1  - Sell Price
	2  - Type
	3  - maxchance (max drop chance of this item, e.g. 1 = 0.01%)
		 if = 0, then monsters don't drop it at all (rare or a quest item)
		 if = 10000, then this item is sold in NPC shops only
	4  - Gender
	5  - Loc
	6  - Weight
	7  - ATK
	8  - DEF
	9  - Range
	10 - Slot
	11 - View
	12 - eLV
	13 - wLV
	14 - SpriteID from 'db/item_avail.txt'
	15 - eLVMax
	16 - matk if RENEWAL is defined
```

See the sample in `doc/sample/getiteminfo.txt`.
