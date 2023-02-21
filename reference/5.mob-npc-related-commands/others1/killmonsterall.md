### killmonsterall
```
*killmonsterall "<map name>"{,<type>};
```

This command will kill all monsters on a specified map name, regardless of how
they were spawned or what they are. As of r12873, The behavior has changed slightly.
In light of a label behavior fix for mob spawning commands that will now allow the label to
trigger when there is no player, killmonsterall has also been modified to support this.

Using this the normal/old way means labels don't trigger when a player didn't
attack/kill a monster. This is because it breaks compatibility with older scripts if
forced to use the new method. However, if you wish to use the new label type with this
command, simply use 1 for type. Any other number won't be recognized.
