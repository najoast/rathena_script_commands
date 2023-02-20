### setfalcon/ checkfalcon
```
*setfalcon {<flag>{,<char_id>}};
*checkfalcon({<char_id>});
```

If `<flag>` is 0 this command will remove the falcon from the character.
Otherwise it gives the invoking character a falcon. The falcon will be there
regardless of whether the character is a hunter or not. It will (probably) not
have any useful effects for non-hunters though.
Note: the character needs to have the skill HT_FALCON to gain a falcon

The accompanying function will return 1 if the invoking character has a falcon
and 0 if they don't.

```c
if (checkfalcon())
    mes "But you already have a falcon!";
```
