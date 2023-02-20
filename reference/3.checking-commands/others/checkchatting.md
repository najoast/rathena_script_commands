### checkchatting
```
*checkchatting({"<Player Name>"})
```

Checks if the player is in a chatroom.
Name is optional, and defaults to the attached player if omitted.
Returns 1 if they are in a chat room, 0 if they are not.

Examples:
```c
//This will check if the attached player in a chat room or not.
if (checkchatting())
    mes "You are currently in a chat room!";
```
