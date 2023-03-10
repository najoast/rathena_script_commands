### warpwaitingpc
```
*warpwaitingpc "<map name>",<x>,<y>{,<number of people>};
```

This command will warp the amount of characters equal to the trigger number of
the waiting room chat attached to the NPC object running this command to the
specified map and coordinates, kicking them out of the chat. Those waiting the
longest will get warped first. It can also do a random warp on the same map
("Random" instead of map name) and warp to the save point ("SavePoint").

The list of characters to warp is taken from the list of the chat room members.
Those not in the chat room will not be considered even if they are talking to
the NPC in question. If the number of people is given, exactly this much people
will be warped.

This command can also keep track of who just got warped. It does this by setting
special variables:

* `$@warpwaitingpc[]` is an array containing the account_id numbers of the characters who were just warped.
* `$@warpwaitingpcnum` contains the number of the character it just warped.

See also 'getpartymember' for advice on what to do with those variables.

The obvious way of using this effectively would be to set up a waiting room for
two characters to be warped onto a random PVP map for a one-on-one duel, for
example.
