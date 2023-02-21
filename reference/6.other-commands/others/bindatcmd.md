### bindatcmd
```
*bindatcmd "<command>","<NPC object name>::<event label>"{,<atcommand level>,<charcommand level>};
```

This command will bind a NPC event label to an atcommand. Upon execution of the
atcommand, the user will invoke the NPC event label. Each atcommand is only allowed
one binding. If you rebind, it will override the original binding.
Note: The default level for atcommand is 0 while the default level for charcommand is 100.

The following variables are set upon execution:
```
.@atcmd_command$       =  The name of the @command used.
.@atcmd_parameters$[]  =  Array containing the given parameters, starting from an index of 0.
.@atcmd_numparameters  =  The number of parameters defined.
```
Example:

	When a user types the command "@test", an angel effect will be shown.
```c
	-	script	atcmd_example	-1,{
	OnInit:
		bindatcmd "test",strnpcinfo(3) + "::OnAtcommand";
		end;
	OnAtcommand:
		specialeffect2 EF_ANGEL2;
		end;
	}
```
