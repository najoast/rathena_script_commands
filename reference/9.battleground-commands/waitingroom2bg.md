### waitingroom2bg
```
*waitingroom2bg("<map name>",<x>,<y>,{"<On Quit Event>","<On Death Event>"{,"<NPC Name>"}});
```

`<map name>,<x>,<y>` refer to where the "respawn" base is, where the player group will respawn when they die.
`<On Quit Event>` refers to an NPC label that attaches to the character and is run when they relog. (Optional)
`<On Death Event>` refers to an NPC label that attaches to the character and is run when they die. (Optional)

If "-" is supplied for `<map name>` then the player will not automatically respawn after the 1 second delay.
This allows for better manipulation of `<On Death Event>`. The player will have to be warped to desired location
at the end of `<On Death Event>`.

Unlike the prior command, the latter will attach a GROUP in a waiting room to the battleground, and
sets the array $@arenamembers[0] where 0 holds the IDs of the first group, and 1 holds the IDs of the second.

If the optional NPC Name parameter is left out, the waiting room of the current NPC is used.

Example:
```c
// Battle Group will be referred to as $@KvM01BG_id1, and when they die, respawn at bat_c01,52,129.
set $@KvM01BG_id1, waitingroom2bg("bat_c01",52,129,"KvM01_BG::OnGuillaumeQuit","KvM01_BG::OnGuillaumeDie");
end;
```
