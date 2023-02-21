### groupranditem
```
*groupranditem <group id>{,<sub_group>};
```

Returns the item_id of a random item picked from the group specified. The
different groups and their group number are specified in 'db/(pre-)re/item_group_db.txt'.

When used in conjunction with other functions, you can get a random item. For
example, for a random pet lure:

getitem groupranditem(IG_Taming),1;

'sub_group' is used to get the available random items of item group from specified random
group. 0 for 'must' item group, and random item group is 1 until 5 (MAX_ITEMGROUP_RANDGROUP+1).

More info, see doc/item_group.txt.
