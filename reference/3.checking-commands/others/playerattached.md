### playerattached
```
*playerattached()
```

Returns the ID of the player currently attached to the script. It will return
0 if no one is attached, or if the attached player no longer exists on the map
server. It is wise to check for the attached player in script functions that
deal with timers as there's no guarantee the player will still be logged on
when the timer triggers. Note that the ID of a player is actually their
account ID.
