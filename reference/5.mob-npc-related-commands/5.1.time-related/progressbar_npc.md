### progressbar_npc
```
*progressbar_npc "<color>",<seconds>{,<"NPC Name">};
```

This command works like progressbar, but displays a progress bar
above the head of the currently attached (or given) NPC. Once the
given amount of seconds passes, the script resumes. The color format
is in RGB (RRGGBB). The color is currently ignored by the client and
appears always green.

Note: If a player is attached to the NPC, they are detached from the NPC
as soon as the progress bar activates.
