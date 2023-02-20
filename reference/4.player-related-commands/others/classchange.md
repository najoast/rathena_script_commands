### classchange
```
*classchange(<view id>{,"<NPC name>","<flag>"});
```

This command is very ancient, its origins are clouded in mystery.
It will send a 'display id change' packet to everyone in the immediate area of
the NPC object, which will supposedly make the NPC look like a different sprite,
an NPC sprite ID, or a monster ID. This effect is not stored anywhere and will
not persist (Which is odd, cause it would be relatively easy to make it do so)
and most importantly, will not work at all since this command was broken with
the introduction of advanced classes. The code is written with the assumption
that the lowest sprite IDs are the job sprites and the anything beyond them is
monster and NPC sprites, but since the advanced classes rolled in, they got the
ID numbers on the other end of the number pool where monster sprites float.

As a result it is currently impossible to call this command with a valid view
id. It will do nothing whatsoever if the view ID is below 4047. Getting it to
run will actually just crash the client.

It could be a real gem if it can be gotten to actually do what it's supposed to
do, but this will only happen in a later SVN revision.

Empty `<NPC name>` means attached NPC.

Target for `<flag>`:
- bc_area : Sprite is sent to players in the vicinity of the source (default value).
- bc_self : Sprite is sent only to player attached.
