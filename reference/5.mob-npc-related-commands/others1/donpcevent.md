### donpcevent
```
*donpcevent "<NPC object name>::<event label>";
```

This command invokes the event label code within an another NPC or NPCs. It
starts a separate instance of execution, and the invoking NPC will resume
execution its immediately.

If the supplied event label has the form "NpcName::OnLabel", then only given
NPC's event label will be invoked (much like 'goto' into another NPC). If the
form is "::OnLabel" (NPC name omitted), the event code of all NPCs with given
label will be invoked, one after another. In both cases the invoked script
will run without an attached RID, whether or not the invoking script was
attached to a player. The event label name is required to start with "On".

This command can be used to make other NPCs act, as if they were responding to
the invoking NPC's actions, such as using an emotion or talking.

```c
place,100,100,1%TAB%script%TAB%NPC1%TAB%53,{
    mes "NPC2 copies my actions!";
    close2;
    donpcevent "NPC2::OnEmote";
    end;
OnEmote:
    emotion rand(1,30);
    end;
}

place,102,100,1%TAB%script%TAB%NPC2%TAB%53,{
    mes "NPC1 copies my actions!";
    close2;
    donpcevent "NPC1::OnEmote";
    end;
OnEmote:
    emotion rand(1,30);
    end;
}
```

Whichever of the both NPCs is talked to, both will show a random emotion at the
same time.

As of r16564, command now returns 1 or 0 on success and failure.
A debug message also shows on the console when no events are triggered.
