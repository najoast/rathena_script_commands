### checkweight
```
*checkweight(<item id>,<amount>{,<item id>,<amount>,<item id>,<amount>,...});
*checkweight("<item name>",<amount>{,"<item name>",<amount>,"<item name>",<amount>,...});
*checkweight2(<id_array>,<amount_array>);
```

These functions will compute and return 1 if the total weight of the specified
number of specific items does not exceed the invoking character's carrying
capacity, and 0 otherwise. It is important to see if a player can carry the
items you expect to give them, failing to do that may open your script up to
abuse or create some very unfair errors.

The second function will check an array of items and amounts, and also
returns 1 on success and 0 on failure.

The functions, in addition to checking to see if the player is capable of
holding a set amount of items, also ensure the player has room in their
inventory for the item(s) they will be receiving.

Like 'getitem', this function will also accept an 'english name' from the
database as an argument.

Example 1:
```c
if (checkweight(512,10)) {
    getitem 512,10;
} else {
    mes "Sorry, you cannot hold this amount of apples!";
}
```

Example 2:
```c
setarray .@item[0],512,513,514;
setarray .@amount[0],10,5,5;
if (!checkweight2(.@item,.@amount)) {
    mes "Sorry, you cannot hold this amount of fruit!";
}
```
