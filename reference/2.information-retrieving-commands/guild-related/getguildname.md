### getguildname
```
*getguildname(<guild id>)
```

This function returns a guild's name given an ID number. If there is no such
guild, "null" will be returned.

Example:
```c
mes "The guild " + getguildname(10007) + " are all nice people.";
```
