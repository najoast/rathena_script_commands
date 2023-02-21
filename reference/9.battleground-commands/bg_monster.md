### bg_monster
```
*bg_monster <Battle Group>,"<map name>",<x>,<y>,"<name to show>",<mob id>,"<event label>";
*bg_monster(<Battle Group>,"<map name>",<x>,<y>,"<name to show>",<mob id>,"<event label>");
```

Similar to the 'monster' command.
Spawns a monster with allegiance to the given Battle Group.
Does not allow for the summoning of multiple monsters.
Monsters are similar to those in War of Emperium, in that the specified Battle Group is considered friendly.

Example:
```c
// It can be used in two different ways.
bg_monster $@TierraBG1_id2,"bat_a01",167,50,"Food Depot",1910,"Feed Depot#1::OnMyMobDead";
end;

// Alternatively, you can set an ID for the monster using "set".
// This becomes useful when used with the command below.
set $@Guardian_3, bg_monster($@TierraBG1_id2,"bat_a01",268,204,"Guardian",1949,"NPCNAME::OnMyMobDead");
end;
```
