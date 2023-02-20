### getpartnerid
```c
*getpartnerid({<char_id>})
```

This function returns the character ID of the invoking character's marriage
partner, if any. If the invoking character is not married, it will return 0,
which is a quick way to see if they are married:

```c
if (getpartnerid()) mes "I'm not going to be your girlfriend!";
if (getpartnerid()) mes "You're married already!";
```
