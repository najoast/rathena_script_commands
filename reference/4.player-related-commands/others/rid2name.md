### rid2name
```
*rid2name(<rid>)
```

Converts rid to name. Note: The player/monster/NPC must be online/enabled.
Good for PCKillEvent where you can convert 'killedrid' to the name of the player.

Note:
rid2name may not produce correct character names since rid = account id.  It will return the current online character of the account only.
