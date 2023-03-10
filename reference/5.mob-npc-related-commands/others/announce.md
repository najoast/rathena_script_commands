
### announce
```
*announce "<text>",<flag>{,<fontColor>{,<fontType>{,<fontSize>{,<fontAlign>{,<fontY>}}}}};
```

This command will broadcast a message to all or most players, similar to
@kami/@kamib GM commands.

```c
announce "This will be shown to everyone at all in yellow.",0;
```

The region the broadcast is heard in (target), source of the broadcast
and the color the message will come up as is determined by the flags.

The flag values are coded as constants in 'src/map/script_constants.hpp' to make them easier to use.

Target flags:
- bc_all: Broadcast message is sent server-wide (default).
- bc_map: Message is sent to everyone in the same map as the source of the broadcast (see below).
- bc_area: Message is sent to players in the vicinity of the source.
- bc_self: Message is sent only to current player.
You cannot use more than one target flag.

Source flags:
- bc_pc: Broadcast source is the attached player (default).
- bc_npc: Broadcast source is the NPC, not the player attached to the script
  (useful when a player is not attached or the message should be sent to those
  nearby the NPC).
You cannot use more than one source flag.

Special flags:
- bc_yellow: Broadcast will be displayed in yellow color (default).
- bc_blue: Broadcast will be displayed in blue color.
- bc_woe: Indicates that this broadcast is 'WoE Information' that can be disabled client-side.
Due to the way client handles broadcasts, it is impossible to set both bc_blue and bc_woe.

The optional parameters allow usage of broadcasts in custom colors, font-weights, sizes etc.
If any of the optional parameters is used, special flag is ignored.
Optional parameters may not work well (or at all) depending on a game client used.

The color parameter is a single number which can be in hexadecimal notation.
For example:
    announce "This will be shown to everyone at all in green.",bc_all,0x00FF00;
Will display a global announce in green. The color format is in RGB (0xRRGGBB).

In official scripts only two font-weights (types) are used:
 - normal (FW_NORMAL = 400, default),
 - bold (FW_BOLD = 700).

Default font size is 12.

Using this for private messages to players is probably not that good an idea,
but it can be used instead in NPCs to "preview" an announce.

```c
// This will be a private message to the player using the NPC that made the
// announcement
announce "This is my message just for you",bc_blue|bc_self;

// This will be shown on everyones screen that is in sight of the NPC.
announce "This is my message just for you people here",bc_npc|bc_area;
```
