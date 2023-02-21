### bg_create
```
*bg_create("<map name>",<x>,<y>{,"<On Quit Event>","<On Death Event>"});
```

Creates an instance of battleground battle group that can be used with other battleground commands.

`<map name>,<x>,<y>` refer to where the "respawn" base is, where the player group will respawn when they die.
`<On Quit Event>` refers to an NPC label that attaches to the character and is run when they relog. (Optional)
`<On Death Event>` refers to an NPC label that attaches to the character and is run when they die. (Optional)

If "-" is supplied for `<map name>` then the player will not automatically respawn after the 1 second delay.
This allows for better manipulation of `<On Death Event>`. The player will have to be warped to desired location
at the end of `<On Death Event>`.

Returns battle group ID on success. Returns 0 on failure.
