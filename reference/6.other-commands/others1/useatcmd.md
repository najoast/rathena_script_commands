### useatcmd
```
*useatcmd "<command>";
```

This command will execute a script-bound atcommand for the attached RID. If the
supplied command is not bound to any script, this command will act like 'atcommand'
and attempt to execute a source-defined command.

The three `.@atcmd_*****` variables will NOT be set when invoking script-bound atcommands
in this way.
