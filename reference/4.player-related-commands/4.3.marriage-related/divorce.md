### divorce
```
*divorce({<char_id>})
```

This function will "un-marry" the invoking character from whoever they were
married to. Both will no longer be each other's marriage partner, (at least in
current SVN, which prevents the cases of multi-spouse problems). It will return
1 upon success or 0 if the character was not married at all.

This function will also destroy both wedding rings and send a message to both
players, telling them they are now divorced.
