### getitemslots
```
*getitemslots(<item ID>)
```

This function will look up the item with the specified ID number in the database
and return the number of slots this kind of items has - 0 if they are not
slotted. It will also be 0 for all non-equippable items, naturally, unless
someone messed up the item database. It will return -1 if there is no such item.

Example:

```
//.@slots now has the amount of slots of the item with ID 1205.
.@slots = getitemslots(1205);
```
