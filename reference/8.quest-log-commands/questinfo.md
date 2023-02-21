### questinfo
```
*questinfo <Quest ID>,<Icon>{,<Map Mark Color>{,<Job Class>}};
```

This is esentially a combination of checkquest and showevent. Use this only
in an OnInit label. For the Quest ID, specify the quest ID that you want
checked if it has been started yet.

For Icon, use one of the following:

```
No Icon			: QTYPE_NONE
! Quest Icon	: QTYPE_QUEST
? Quest Icon	: QTYPE_QUEST2
! Job Icon		: QTYPE_JOB
? Job Icon		: QTYPE_JOB2
! Event Icon	: QTYPE_EVENT
? Event Icon	: QTYPE_EVENT2
Warg			: QTYPE_WARG (Only for packetver < 20170315)
Warg Face		: QTYPE_WARG2 (Only for packetver >= 20120410 and < 20170315)
Click Me		: QTYPE_CLICKME (Only for packetver >= 20170315)
Daily Quest		: QTYPE_DAILYQUEST (Only for packetver >= 20170315)
! Event Icon	: QTYPE_EVENT3 (Only for packetver >= 20170315)
Job Quest		: QTYPE_JOBQUEST (Only for packetver >= 20170315)
Jumping Poring	: QTYPE_JUMPING_PORING (Only for packetver >= 20170315)
```

Map Mark Color, when used, creates a mark in the user's mini map on the position of the NPC,
the available color values are:

* 0 - No Marker (default)
* 1 - Yellow Marker
* 2 - Green Marker
* 3 - Purple Marker

When a user shows up on a map, each NPC is checked for questinfo that has been set.
If questinfo is present, it will check if the quest has been started, if it has not, the bubble will appear.

Optionally, you can also specify a Job Class if the quest bubble should only appear for a certain class.

Example
```c
izlude,100,100,4	script	Test	844,{
	mes "[Test]";
	mes "Hello World.";
	close;

	OnInit:
		questinfo 1001, QTYPE_QUEST, 0, Job_Novice;
		end;
}
```
