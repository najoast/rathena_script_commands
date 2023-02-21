### disguise
```
*disguise <Monster ID>{,<char_id>};
```
### undisguise
```
*undisguise {<char_id>};
```

This command disguises the current player with a monster sprite.
The disguise lasts until 'undisguise' is issued or the player logs out.

Example:

```c
disguise 1002; // Disguise character as a Poring.
next;
undisguise; // Return to normal character sprite.
```
