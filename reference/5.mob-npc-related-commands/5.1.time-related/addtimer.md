### addtimer
```
*addtimer <ticks>,"NPC::OnLabel";
```
### deltimer
```
*deltimer "NPC::OnLabel";
```
### addtimercount
```
*addtimercount <ticks>,"NPC::OnLabel";
```

These commands will create, destroy, and delay a countdown timer - 'addtimer' to
create, 'deltimer' to destroy and 'addtimercount' to delay it by the specified
number of ticks. For all three cases, the event label given is the identifier of
that timer. The timer runs on the character object that is attached to the script,
and can have multiple instances. When the label is run, it is run as if the player that
the timer runs on has clicked the NPC.

When this timer runs out, a new execution thread will start in the specified NPC
object at the specified label.

The ticks are given in 1/1000ths of a second.

One more thing. These timers are stored as part of player data. If the player
logs out, all of these get immediately deleted, without executing the script.
If this behavior is undesirable, use some other timer mechanism (like 'sleep').

Example:
```c
<NPC Header> {
	dispbottom "Starting a 5 second timer...";
	addtimer 5000, strnpcinfo(3) + "::On5secs";
	end;
On5secs:
	dispbottom "5 seconds have passed!";
	end;
}
```
