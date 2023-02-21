```
*agitstart;
*agitend;
*agitstart2;
*agitend2;
*agitstart3;
*agitend3;
```

These commands will start and end War of Emperium FE, War of Emperium SE,
or War of Emperium TE.

This is a bit more complex than it sounds, since the commands themselves won't
actually do anything interesting, except causing all `OnAgitStart:` and
`OnAgitEnd:`, `OnAgitStart2:` and `OnAgitEnd2:`, or `OnAgitStart3:` and
`OnAgitEnd3:` in the case of latter two commands, events to run everywhere,
respectively. They are used as  simple triggers to run a lot of complex scripts
all across the server, and they, in turn, are triggered by clock with an
`OnClock<time>:` time-triggering label.
