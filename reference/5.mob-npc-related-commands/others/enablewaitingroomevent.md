```
*enablewaitingroomevent {"<NPC object name>"};
*disablewaitingroomevent {"<NPC object name>"};
*enablearena;
*disablearena;
```

This will enable and disable triggering the waiting room event (see
'waitingroom') respectively. Optionally giving an NPC object name will do that
for a specified NPC object. The chat room will not disappear when triggering is
disabled and enabled in this manner and players will not be kicked out of it.
Enabling a chat room event will also cause it to immediately check whether the
number of users in it exceeded the trigger amount and trigger the event
accordingly.

Normally, whenever a waiting room was created to make sure that only one
character is, for example, trying to pass a job quest trial, and no other
characters are present in the room to mess up the script.

The 'enablearena'/'disablearena' commands are just aliases with no parameter.
These are supposedly left here for compatibility with official server scripts,
but no rAthena script uses these at the moment.
