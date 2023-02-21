### getrandgroupitem
```
*getrandgroupitem <group_id>{,<quantity>{,<sub_group>{,<identify>{,<char_id>}}}};
```

Similar to the above example, this command allows players to obtain the specified
quantity of a random item from the group `<group id>`. The different groups and
their group number are specified in db/(pre-)re/item_group_db.txt

If 'quantity' is not defined or 0, it will uses defined amount from Item Group list.

If 'sub_group' is not defined the value will be 1 (since random group is 1 ~ 5, and 0 is
'must' item group).

For item with type IT_WEAPON, IT_ARMOR, IT_PETARMOR, and IT_SHADOWGEAR will be given
as unidentified item (as defined by itemdb_isidentified in src/map/itemdb.cpp) except
if 'identify' is defined with value 1.

More info, see doc/item_group.txt.
