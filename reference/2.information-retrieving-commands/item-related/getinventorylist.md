
### getinventorylist
```
*getinventorylist {<char_id>};
```

This command sets a bunch of arrays with a complete list of whatever the
invoking character has in their inventory, including all the data needed to
recreate these items perfectly if they are destroyed. Here's what you get:

```
@inventorylist_id[]                - array of item ids.
@inventorylist_amount[]            - their corresponding item amounts.
@inventorylist_equip[]             - on which position the item is equipped (see EQP_* constants)
                                     It will contain 0 if the item is not equipped.
@inventorylist_refine[]            - for how much it is refined.
@inventorylist_identify[]          - whether it is identified.
@inventorylist_attribute[]         - whether it is broken.
@inventorylist_card1[]             - These four arrays contain card data for the items.
@inventorylist_card2[]               These data slots are also used to store names
@inventorylist_card3[]               inscribed on the items, so you can explicitly check
@inventorylist_card4[]               if the character owns an item made by a specific
                                     craftsman.
@inventorylist_expire[]            - expire time (Unix time stamp). 0 means never expires.
@inventorylist_bound[]             - the bound type of the items (see BOUND_* constants)
@inventorylist_count               - the number of items in these lists.
@inventorylist_option_id1[]        - first array of random option IDs
@inventorylist_option_value1[]     - first array of random option values
@inventorylist_option_parameter1[] - first array of random option parameters
@inventorylist_option_id2[]        - second array of random option IDs
@inventorylist_option_value2[]     - second array of random option values
@inventorylist_option_parameter2[] - second array of random option parameters
@inventorylist_option_id3[]        - third array of random option IDs
@inventorylist_option_value3[]     - third array of random option values
@inventorylist_option_parameter3[] - third array of random option parameters
@inventorylist_option_id4[]        - fourth array of random option IDs
@inventorylist_option_value4[]     - fourth array of random option values
@inventorylist_option_parameter4[] - fourth array of random option parameters
@inventorylist_option_id5[]        - fifth array of random option IDs
@inventorylist_option_value5[]     - fifth array of random option values
@inventorylist_option_parameter5[] - fifth array of random option parameters
```

This could be handy to save/restore a character's inventory, since no other
command returns such a complete set of data, and could also be the only way to
correctly handle an NPC trader for carded and named items who could resell them
- since NPC objects cannot own items, so they have to store item data in
variables and recreate the items.

Notice that the variables this command generates are all temporary, attached to
the character, and integer.

Be sure to use @inventorylist_count to go through these arrays, and not
'getarraysize', because the arrays are not automatically cleared between runs
of 'getinventorylist'.
