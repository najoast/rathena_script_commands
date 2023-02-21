### atcommand
```
*atcommand "<command>";
```

This command will run the given command line exactly as if it was typed in from
the keyboard by the player connected to the invoking character, and that
character belonged to an account which had GM level 99.

```c
	// This will ask the invoker for a character name and then use the '@nuke'
	// GM command on them, killing them mercilessly.
	input .@player$;
	atcommand "@nuke " + .@player$;
```

Note that for atcommands bound using 'bindatcmd', this command will execute the
original atcommand, not the script-bound atcommand.
