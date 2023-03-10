```
*npcspeed <speed value>;
*npcwalkto <x>,<y>;
*npcstop;
```

These commands will make the NPC object in question move around the map. As they
currently are, they are a bit buggy and are not useful for much more than making
an NPC move randomly around the map.

'npcspeed' will set the NPCs walking speed to a specified value. As in the
@speed GM command, 200 is the slowest possible speed while 0 is the fastest
possible (instant motion). 100 is the default character walking speed.

'npcwalkto' will start the NPC sprite moving towards the specified coordinates
on the same map it is currently on. The script proceeds immediately after the
NPC begins moving.

'npcstop' will stop the motion.

While in transit, the NPC will be clickable, but invoking it will cause it to
stop moving, which will make its coordinates different from what the client
computed based on the speed and motion coordinates. The effect is rather
unnerving.

Only a few NPC sprites have walking animations, and those that do, do not get
the animation invoked when moving the NPC, due to the problem in the NPC walking
code, which looks a bit silly. You might have better success by defining a job-
sprite based sprite id in 'db/mob_avail.txt' with this.
