### countbound
```
*countbound({<bound type>{,<char_id>}})
```

This function will return the number of bounded items in the character's
inventory, and sets an array @bound_items[] containing all item IDs of the
counted items. If a bound type is specified, only those items will be counted.

For a list of bound types see 'getitembound'.

Example:
```c
mes "You currently have " + countbound() + " bounded items.";
next;
mes "The list of bounded items include:";
for(.@i = 0; .@i < getarraysize(@bound_items); .@i++)
    mes getitemname(@bound_items[.@i]);
close;
```
