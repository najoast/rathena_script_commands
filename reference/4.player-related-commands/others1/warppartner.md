### warppartner
```
*warppartner("<map name>",<x>,<y>);
```

This function will find the invoking character's marriage partner, if any, and
warp them to the map and coordinates given. It will return 1 upon success and
0 if the partner is not online, the character is not married, or if there's no
invoking character (no RID). 0,0 will, as usual, normally translate to random coordinates.
