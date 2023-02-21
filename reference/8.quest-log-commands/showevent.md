### showevent
```
*showevent <icon>{,<mark color>{,<char_id>}}
```

Show an emotion on top of a NPC, and optionally,
a colored mark in the mini-map like "viewpoint".
This is used to indicate that a NPC has a quest or an event to
a certain player.

Available Icons:

```
Remove Icon		: QTYPE_NONE
! Quest Icon	: QTYPE_QUEST
? Quest Icon	: QTYPE_QUEST2
! Job Icon		: QTYPE_JOB
? Job Icon		: QTYPE_JOB2
! Event Icon	: QTYPE_EVENT
? Event Icon	: QTYPE_EVENT2
Warg			: QTYPE_WARG
Warg Face		: QTYPE_WARG2 (Only for packetver >= 20120410)
```

Mark Color:
* 0 - No Mark
* 1 - Yellow Mark
* 2 - Green Mark
* 3 - Purple Mark
