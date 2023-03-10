
### waitingroom
```
*waitingroom "<chatroom name>",<limit>{,"<event label>"{,<trigger>{,<required zeny>{,<min lvl>{,<max lvl>}}}}};
```

This command will create a chat room, owned by the NPC object running this
script and displayed above the NPC sprite.
The maximum length of a chat room name is 60 letters.

The limit is the maximum number of people allowed to enter the chat room.
The attached NPC is included in this count. If the optional event and trigger
parameters are given, the event label (`<NPC object name>::<label name>`)
will be invoked as if with a 'doevent' upon the number of people in the chat
room reaching the given triggering amount.

```c
// The NPC will just show a box above its head that says "Hello World", clicking
// it will do nothing, since the limit is zero.
waitingroom "Hello World",0;

// The NPC will have a box above its head, it will say "Disco - Waiting Room"
// and will have 8 waiting slots. Clicking this will enter the chat room, where
// the player will be able to wait until 7 players accumulate. Once this happens,
// it will cause the NPC "Bouncer" run the label "OnStart".
waitingroom "Disco - Waiting Room",8,"Bouncer::OnStart",7;

// The NPC will have a box above its head, it will say "Party - Waiting Room"
// and will have 8 waiting slots. Clicking this will allow a player who has
// 5000 zeny and lvl 50~99 to enter the chat room, where the player will be
// able to wait until 7 players accumulate. Once this happens, it will cause
// the NPC "Bouncer" run the label "OnStart".
waitingroom "Party - Waiting Room",8,"Bouncer::OnStart",7,5000,50,99;
```

Creating a waiting room does not stop the execution of the script and it will
continue to the next line.

For more examples see the 2-1 and 2-2 job quest scripts which make extensive use
of waiting rooms.
