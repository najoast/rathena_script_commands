### getguildmaster
```
*getguildmaster(<guild id>)
```

This function return the name of the master of the guild which has the specified
ID number. If there is no such guild, "null" will be returned.

Example 1:
```c
// Prints the guild master of guild 10007, whoever that might be.
mes getguildmaster(10007) + " runs " + getguildname(10007);
```

Example 2:
```c
// Checks if the character is the guild master of the specified guild.
.@GID = getcharid(2);
if (.@GID == 0) {
    mes "Sorry, you are not in a guild.";
    close;
}
if (strcharinfo(0) != getguildmaster(.@GID)) {
    mes "Sorry, you don't own the guild you are in.";
    close;
}
mes "Welcome, guild master of " + getguildname(.@GID);
close;
```
