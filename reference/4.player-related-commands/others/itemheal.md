### itemheal
```
*itemheal <hp>,<sp>{,<char_id>};
```

This command heals relative amounts of HP and/or SP on the invoking character.
Unlike heal, this command is intended for use in item scripts. It applies
potion-related bonuses, such as alchemist ranking, cards, and status changes.
When used inside an NPC script, certain bonuses are omitted.

The command also applies a SP/VIT-related bonus:
```
heal = heal * [(100 + STATUS*2) / 100]
```

Example:
```c
// If the player has 50 vit and no bonuses, this will heal
// anything from 200 to 300 HP and 5 SP
itemheal rand(100,150),5;
```
