### buyingstore
```
*buyingstore <slots>;
```

Invokes buying store preparation window like the skill 'Open Buying Store',
without the item requirement. Amount of slots is limited by the server to
a maximum of 5 slots by default.

Example:
```c
// Gives the player opportunity to buy 4 different kinds of items.
buyingstore 4;
```
