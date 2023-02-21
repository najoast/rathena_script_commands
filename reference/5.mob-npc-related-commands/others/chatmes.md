### chatmes
```
*chatmes "<message>"{,"<NPC name>"};
```

This command will display a message in the waitingroom (chat) of the NPC.
If the `<NPC name>` option is given, then that NPC will display the message, else
the attached NPC will display the message.
If the NPC is not in a waitingroom, nothing happens.

```c
// Everyone in the waitingroom will see this message:
chatmes "Waiting 5 minutes until the next match will start";
```
