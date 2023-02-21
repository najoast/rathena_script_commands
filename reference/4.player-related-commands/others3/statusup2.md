### statusup2
```
*statusup2 <stat>,<amount>{,<char_id>};
```

This command will change a specified stat of the invoking character by the
specified amount permanently. The amount can be negative. See 'statusup'.

```c
// This will decrease a character's Vit forever.
statusup2 bVit,-1;
```
