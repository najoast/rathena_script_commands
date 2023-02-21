### pcfollow
```
*pcfollow <id>,<target id>;
```
### pcstopfollow
```
*pcstopfollow <id>;
```

Makes a character follow or stop following someone. This command does the same
as the @follow command. The main difference is that @follow can use character
names, and this commands needs the account ID for the target.

Examples:
```c
// This will make Aaron follow Bullah, when both of these characters are online.
pcfollow getCharID(3,"Aaron"),getCharID(3,"Bullah");

// Makes Aaron stop following whoever he is following.
pcstopfollow getCharID(3,"Aaron");
```
