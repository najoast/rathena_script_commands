### getscrate
```
*getscrate(<effect type>,<base rate>{,<GID>})
```

This function will return the chance of a status effect affecting the invoking
character, in percent, modified by the their current defense against said
status. The 'base rate' is the base chance of the status effect being inflicted,
in percent.

```c
if (rand(100) > getscrate(Eff_Blind, 50))
    goto BlindHimNow;
```

You can see the full list of available effect types you can possibly inflict in
'db/const.txt' under 'Eff_'.
