### percentheal
```
*percentheal <hp>,<sp>{,<char_id>};
```

This command will heal the invoking character. It heals the character, but not
by a set value - it adds percent of their maximum HP/SP.

```c
percentheal 100,0; // This will heal 100% HP
percentheal 0,100; // This will heal 100% SP
percentheal 50,50; // This will heal 50% HP and 50% SP
```

So the amount that this will heal will depend on the total amount of HP or SP
you have maximum. Like 'heal', this will not call up any animations or effects.
