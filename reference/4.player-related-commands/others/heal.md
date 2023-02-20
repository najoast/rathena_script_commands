### heal
```
*heal <hp>,<sp>{,<char_id>};
```

This command will heal a set amount of HP and/or SP on the invoking character.

```c
heal 30000,0; // This will heal 30,000 HP
heal 0,30000; // This will heal 30,000 SP
heal 300,300; // This will heal 300 HP and 300 SP
```

This command just alters the hit points and spell points of the invoking
character and produces no other output whatsoever.
