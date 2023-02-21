### specialeffect2
```
*specialeffect2 <effect number>{,<send_target>{,"<Player Name>"}};
```

This command behaves identically to 'specialeffect', but the effect will be
centered on the invoking character's sprite.

`<Player name>` parameter will display `<effect number>` on another Player than the
one currently attached to the script. Like with specialeffect, when specifying
a player, `<send_target>` must be supplied, specifying AREA will retain the default
behavior of the command.
