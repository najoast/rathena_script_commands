### monster
```
*monster     "<map name>",<x>,<y>,"<name to show>",<mob id>,<amount>{,"<event label>",<size>,<ai>};
```
### areamonster
```
*areamonster "<map name>",<x1>,<y1>,<x2>,<y2>,"<name to show>",<mob id>,<amount>{,"<event label>",<size>,<ai>};
```

This command will spawn a monster on the specified coordinates on the specified
map. If the script is invoked by a character, a special map name, "this", will
be recognized to mean the name of the map the invoking character is located at.
This command works fine in the item scripts.

The same command arguments mean the same things as described above in the
beginning of this document when talking about permanent monster spawns. Monsters
spawned in this manner will not respawn upon being killed.

Unlike the permanent monster spawns, if the mob id is -1, a random monster will
be picked from the entire database according to the rules configured in the
server for dead branches. This will work for all other kinds of non-permanent
monster spawns.

The only very special thing about this command is an event label, which is an
optional parameter. This label is written like `<NPC object name>::<label name>`
and upon the monster being killed, it will execute the script inside of the
specified NPC object starting from the label given. The RID of the player
attached at this execution will be the RID of the killing character.
The variable 'killedrid' is set to the Class (mob ID) of the monster killed.
The variable 'killedgid' is set to the ID (unique mob game ID) of the monster killed.

`<size>` can be:
* `Size_Small`	(0)		(default)
* `Size_Medium`	(1)
* `Size_Large`	(2)

`<ai>` can be:
* `AI_NONE`		(0)		(default)
* `AI_ATTACK`	(1)		(attack/friendly)
* `AI_SPHERE`	(2)		(Alchemist skill)
* `AI_FLORA`	(3)		(Alchemist skill)
* `AI_ZANZOU`	(4)		(Kagerou/Oboro skill)
* `AI_LEGION`	(5)		(Sera skill)
* `AI_FAW`		(6)		(Mechanic skill)

```c
monster "place",60,100,"Poring",1002,1,"NPCNAME::OnLabel";
```

The coordinates of 0,0 will spawn the monster on a random place on the map.

The 'areamonster' command works much like the 'monster' command and is not
significantly different, but spawns the monsters within a square defined by
x1/y1-x2/y2.

Returned value is an array with the game ID of the spawned monster(s) depending
on the amount spawned. Array is stored in $@mobid[].

Simple monster killing script:

Normal NPC object definition. Let's assume you called him NPCNAME.

```c
mes "[Summon Man]";
mes "Want to start the Poring hunt?";
next;
if (select("Yes.:No.") == 2) {
    mes "[Summon Man]";
    mes "Come back later.";
    close;
}

// Summon 10 Porings.
// Using coordinates 0,0 will spawn them in a random location.
monster "prontera",0,0,"Quest Poring",1002,10,"NPCNAME::OnPoringKilled";

mes "[Summon Man]";
mes "Now go and kill all the Porings I summoned.";
close;

OnPoringKilled:
$PoringKilled++;
if ($PoringKilled >= 10) {
    announce "Summon Man: Well done. All the Porings are dead!",3;
    $PoringKilled = 0;
}
end;
```

For more good examples see just about any official 2-1 or 2-2 job quest script.
