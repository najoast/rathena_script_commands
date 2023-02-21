### specialeffect
```
*specialeffect <effect number>{,<send_target>{,"<NPC Name>"}};
```

This command will display special effect with the given number, centered on the
specified NPCs coordinates, if any. For a full list of special effect numbers
known see 'doc/effect_list.txt'. Some effect numbers are known not to work in
some client releases. (Notably, rain is absent from any client executables
released after April 2005.)

`<NPC name>` parameter will display `<effect number>` on another NPC. If the NPC
specified does not exist, the command will do nothing. When specifying an NPC,
`<send_target>` must be specified when specifying an `<NPC Name>`, specifying AREA
will retain the default behavior of the command.

```c
// this will make the NPC "John Doe#1"
// show the effect "EF_HIT1" specified by
// Jane Doe. I wonder what John did...
mes "[Jane Doe]";
mes "Well, I never!";
specialeffect EF_HIT1,AREA,"John Doe#1";
close;
```
