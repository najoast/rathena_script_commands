### npctalk
```
*npctalk "<message>"{,"<NPC name>","<flag>"};
```

This command will display a message as if the NPC object running it was a player
talking - that is, above their head and in the chat window.
The display name of the NPC won't get appended in front of the message.
If the `<NPC name>` option is given and not empty, then that NPC will display the message,
else the attached NPC will display the message.

Target for `<flag>`:
- `bc_all`: Broadcast message is sent server-wide (only in the chat window).
- `bc_map`: Message is sent to everyone in the same map as the source of the npc.
- `bc_area`: Message is sent to players in the vicinity of the source (default value).
- `bc_self`: Message is sent only to player attached.

```c
// This will make everyone in the area see the NPC greet the character
// who just invoked it.
npctalk "Hello " + strcharinfo(0) + ", how are you?";
```
