### getchildid
```c
*getchildid({<char_id>})
*getmotherid({<char_id>})
*getfatherid({<char_id>})
```

These functions return the character ID of the attached player's child,
mother, mother, or father, respectively. It returns 0 if no ID is found.

```c
if (getmotherid()) mes "Your mother's ID is: " + getmotherid();
```
