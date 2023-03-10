### initnpctimer
```
*initnpctimer{ "<NPC name>" {, <Attach Flag>} } |
             { "<NPC name>" | <Attach Flag> };
*stopnpctimer{ "<NPC name>" {, <Detach Flag>}  } |
             { "<NPC name>" | <Detach Flag> };
*startnpctimer{ "<NPC name>" {, <Attach Flag>} } |
              { "<NPC name>" | <Attach Flag> };
*setnpctimer <tick>{,"<NPC name>"};
*getnpctimer(<type of information>{,"<NPC name>"})
*attachnpctimer {"<character name>"};
*detachnpctimer {"<NPC name>"};
```

This set of commands and functions will create and manage an NPC-based timer.
The NPC name may be omitted, in which case the calling NPC is used as target.

Contrary to addtimer/deltimer commands which let you have many different timers
referencing different labels in the same NPC, each with their own countdown,
'initnpctimer' can only have one per NPC object. But it can trigger many labels
and let you know how many were triggered already and how many still remain.

This timer is counting up from 0 in ticks of 1/1000ths of a second each. Upon
creating this timer, the execution will not stop, but will happily continue
onward. The timer will then invoke new execution threads at labels
`OnTimer<time>:` in the NPC object it is attached to.

To create the timer, use the 'initnpctimer', which will start it running.
'stopnpctimer' will pause the timer, without clearing the current tick, while
'startnpctimer' will let the paused timer continue.

By default timers do not have a RID attached, which lets them continue even
if the player that started them logs off. To attach a RID to a timer, you can
either use the optional "attach flag" when using 'initnpctimer/startnpctimer',
or do it manually by using 'attachnpctimer'. Likewise, the optional flag of
stopnpctimer lets you detach any RID after stopping the timer, and by using
'detachnpctimer' you can detach a RID at any time.

Normally there is only a single timer per NPC, but as an exception, as long as
you attach a player to the timer, you can have multiple timers running at once,
because these will get stored on the players instead of the NPC.
NOTE: You need to attach the RID before the timer _before_ you start it to
get a player-attached timer. Otherwise it'll stay a NPC timer (no effect).

If the player that is attached to the npctimer logs out, the "OnTimerQuit:"
event label of that NPC will be triggered, so you can do the appropriate
cleanup (the player is still attached when this event is triggered).

The 'setnpctimer' command will explicitly set the timer to a given tick.
'getnpctimer' provides timer information. Its parameter defines what type:

 0 - Will return the current tick count of the timer.
 1 - Will return 1 if there are remaining `OnTimer<ticks>:` labels in the
     specified NPC waiting for execution.
 2 - Will return the number of times the timer has triggered and will trigger
     an `OnTimer<tick>:`  label in the specified NPC.

Example 1:

```c
<NPC Header> {
// We need to use attachnpctimer because the mes command below needs RID attach
    attachnpctimer;
    initnpctimer;
    npctalk "I cant talk right now, give me 10 seconds";
    end;
OnTimer5000:
    npctalk "Ok 5 seconds more";
    end;
OnTimer6000:
    npctalk "4";
    end;
OnTimer7000:
    npctalk "3";
    end;
OnTimer8000:
    npctalk "2";
    end;
OnTimer9000:
    npctalk "1";
    end;
OnTimer10000:
    stopnpctimer;
    mes "[Man]";
    mes "Ok we can talk now";
    detachnpctimer;
    // and remember attachnpctimer and detachnpctimer can only use while the NPC timer is not running !
}
```

Example 2:
```c
OnTimer15000:
    npctalk "Another 15 seconds have passed.";

    // You have to use 'initnpctimer' instead of 'setnpctimer 0'.
    // This is equal to 'setnpctimer 0' + 'startnpctimer'.
    // Alternatively, you can also insert another 'OnTimer15001' label so that the timer won't stop. */
    initnpctimer;
    end;

// This OnInit label will run when the script is loaded, so that the timer
// is initialized immediately as the server starts. It is dropped back to 0
// every time the NPC says something, so it will cycle continuously.
OnInit:
    initnpctimer;
    end;
```

Example 3:
```c
mes "[Man]";
mes "I have been waiting " + (getnpctimer(0)/1000) + " seconds for you.";
// We divide the timer returned by 1000 to convert milliseconds to seconds.
close;
```

Example 4:
```c
mes "[Man]";
mes "Ok, I will let you have 30 more seconds...";
close2;
setnpctimer (getnpctimer(0)-30000);
// Notice the 'close2'. If there were a 'next' there the timer would be
// changed only after the player pressed the 'next' button.
end;
```
