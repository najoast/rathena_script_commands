### marriage
```
*marriage("<spouse name>");
```

This function will marry two characters, the invoking character and the one
referred to by name given, together, setting them up as each other's marriage
partner. No second function call has to be issued (in current SVN at least) to
make sure the marriage works both ways. The function returns 1 upon success, or
0 if the marriage could not be completed, either because the other character
wasn't found or because one of the two characters is already married.

This will do nothing else for the marriage except setting up the spouse ID for
both of these characters. No rings will be given and no effects will be shown.
