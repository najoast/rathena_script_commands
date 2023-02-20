### addrid
```
*addrid(<type>{,<flag>{,<parameters>}});
```

This command will attach other RIDs to the current script without detaching the
invoking RID. It returns 1 if successful and 0 upon failure.

`<type>` determines what RIDs are attached:
```
 0: All players in the server.
 1: All players in the map of the invoking player, or the invoking NPC if no player is attached.
 2: Party members of a specified party ID.
    [ Parameters: <party id> ]
 3: Guild members of a specified guild ID.
    [ Parameters: <guild id> ]
 4: All players in a specified area of the map of the invoking player (or NPC).
    [ Parameters: <x0>,<y0>,<x1>,<y1> ]
 5: All players in the map.
    [ Parameters: "<map name>" ]
```

 Account ID: If type is Account ID, attach the specified account ID.

`<flag>` can prevent certain players from being attached:
 0: Players are always attached. (default)
 1: Players currently running another script will not be attached.
