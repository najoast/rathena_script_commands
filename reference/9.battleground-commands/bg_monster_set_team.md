### bg_monster_set_team
```
*bg_monster_set_team <GID>,<Battle Group>;
```

This command will change the allegiance if a monster in a battle ground.
GID can be set when spawning the monster via the 'bg_monster' command.

Example:

	end;

OnEnable:
	mapannounce "A guardian has been summoned for Battle Group 2!",bc_map,"0xFFCE00";
	set $@Guardian, bg_monster($@BG_2,"bat_a01",268,204,"Guardian",1949,"NPCNAME::OnMyMobDead");
	initnpctimer;
	end;

OnTimer1000:
	stopnpctimer;
	mapannounce "Erm, sorry about that! This monster was meant for Battle Group 1.",bc_map,"0xFFCE00";
	bg_monster_set_team $@Guardian, $@BG_1;
	end;
