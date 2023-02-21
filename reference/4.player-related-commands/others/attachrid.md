### attachrid
```
*attachrid(<account ID>{,force})
*detachrid;
```

These commands allow the manipulation of the script's currently attached player.
While 'attachrid' allows attaching of a different player by using its account id
for the parameter RID, 'detachrid' makes the following commands run as if the
script was never invoked by a player.

The command returns false if the player cannot be attached (if the account is offline
or does not exist), and true upon success.

By default the command is executed with force, which causes to attach the player
even if he is currently attached to another script. Since this is not always the
desired behavior you can also specify false to the command and it will only return 
true if the player is online and was not attached to another script.
