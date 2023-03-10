
### callsub
```
*callsub <label>{,<argument>,...<argument>};
*callsub(<label>{,<argument>,...<argument>})
```

This command will go to a specified label within the current script (do NOT use
quotes around it) coming in as if it were a 'callfunc' call, and pass it
arguments given, if any, which can be recovered there with 'getarg'. When done
there, you should use the 'return' command to go back to the point from where
this label was called. This is used when there is a specific thing the script
will do over and over, this lets you use the same bit of code as many times as
you like, to save space and time, without creating extra NPC objects which are
needed with 'callfunc'. A label is not callable in this manner from another
script.

Example 1: callsub for checking (if checks pass, return to script)
```c
	callsub S_CheckFull, "guild_vs2",50;
	switch( rand(4) ) {
		case 0:	warp "guild_vs2",9,50;	end;
		case 1:	warp "guild_vs2",49,90;	end;
		case 2:	warp "guild_vs2",90,50;	end;
		case 3:	warp "guild_vs2",49,9;	end;
	}
	...

S_CheckFull:
	if (getmapusers(getarg(0)) >= getarg(1)) {
		mes "I'm sorry, this arena is full.  Please try again later.";
		close;
	}
	return;
```

Example 2: callsub used repeatedly, with different arguments
```c
	// notice how the Zeny check/delete is reused, instead of copy-pasting for every warp
	switch(select("Abyss Lake:Amatsu Dungeon:Anthell:Ayothaya Dungeon:Beacon Island, Pharos") {
		case 1:	callsub S_DunWarp,"hu_fild05",192,207;
		case 2:	callsub S_DunWarp,"ama_in02",119,181;
		case 3:	callsub S_DunWarp,"moc_fild20",164,145;
		case 4:	callsub S_DunWarp,"ayo_fild02",279,150;
		case 5:	callsub S_DunWarp,"cmd_fild07",132,125;
		// etc
	}

	...

S_DunWarp:
	// getarg(0) = "map name"
	// getarg(1) = x
	// getarg(2) = y
	if (Zeny >= 100) {
		Zeny -= 100;
		warp getarg(0),getarg(1),getarg(2);
	} else {
		mes "Dungeon warp costs 100 Zeny.";
	}
	close;
```
