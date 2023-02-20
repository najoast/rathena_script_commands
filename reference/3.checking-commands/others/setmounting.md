### setmounting
```
*setmounting {<char_id>};
*ismounting({<char_id>});
```

The 'setmounting' function toggles cash mount for the invoking character.
It will return 1 if successful, 0 otherwise.

Note: Character must not be mounting a non-cash mount (eg. dragon, peco, wug, etc.)

The accompanying function will return 1 if the invoking character has a
cash mount and 0 if they don't.
