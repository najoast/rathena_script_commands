### changebase
```
*changebase <job ID number>{,<account ID>};
```

This command will change a character's appearance to that of the specified job
class. Nothing but appearance will change.

The command will run for the invoking character unless an account ID is given.

```c
changebase Job_Novice; // Changes player to Novice sprite.
changebase Class; // Changes player back to default sprite.
```
