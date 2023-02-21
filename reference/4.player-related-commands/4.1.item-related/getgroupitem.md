### getgroupitem
```
*getgroupitem <group_id>{,<identify>{,<char_id>}};
```

Gives item(s) to the attached player based on item group contents.
This is not working like 'getrandgroupitem' which only give 1 item for specified
item group & sub_group.

For item with type IT_WEAPON, IT_ARMOR, IT_PETARMOR, and IT_SHADOWGEAR will be given
as unidentified item (as defined by itemdb_isidentified in src/map/itemdb.cpp) except
if 'identify' is defined with value 1.

More info, see doc/item_group.txt.
