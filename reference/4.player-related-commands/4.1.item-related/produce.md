### produce
```
*produce <item level>;
```

This command will open a crafting window on the client connected to the invoking
character. The 'item level' is a number which determines what kind of a crafting
window will pop-up.

You can see the full list of such item levels in 'db/produce_db.txt' which determines
what can actually be produced. The window will not be empty only if the invoking
character can actually produce the items of that type and has the appropriate raw
materials in their inventory.

The success rate to produce the item is the same as the success rate of the skill
associated with the item level. If there is no skill id, the success rate will be 50%.

Valid item levels are:

* 1   - Level 1 Weapons
* 2   - Level 2 Weapons
* 3   - Level 3 Weapons
* 21  - Blacksmith's Stones and Metals
* 22  - Alchemist's Potions, Holy Water, Assassin Cross's Deadly Poison
* 23  - Elemental Converters
