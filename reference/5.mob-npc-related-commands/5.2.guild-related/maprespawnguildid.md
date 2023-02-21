### maprespawnguildid
```
*maprespawnguildid "<map name>",<guild id>,<flag>;
```

This command goes through the specified map and for each player and monster
found there does stuff.

Flag is a bit-mask (add up numbers to get effects you want)
* 1 - warp all guild members to their save points.
* 2 - warp all non-guild members (including guildless players) to their save points.
* 4 - remove all monsters which are not guardian or Emperium.

Flag 7 will, therefore, mean 'wipe all mobs but guardians and the Emperium and
kick all characters out', which is what the official scripts do upon castle
surrender. Upon start of WoE, the scripts do 2 (warp all intruders out).

For examples, check the WoE scripts in the distribution.
