### setdragon
```
*setdragon {<color>{,<char_id>}};
*checkdragon({<char_id>});
```

The 'setdragon' function toggles mounting a dragon for the invoking character.
It will return 1 if successful, 0 otherwise.

The available colors are:
```
1 - Green Dragon (default)
2 - Brown Dragon
3 - Gray Dragon
4 - Blue Dragon
5 - Red Dragon
```

Note: the character must be a Rune Knight and have the skill RK_DRAGONTRAINING to gain a mount

The accompanying function will return 1 if the invoking character is riding a
dragon and 0 if they aren't.
