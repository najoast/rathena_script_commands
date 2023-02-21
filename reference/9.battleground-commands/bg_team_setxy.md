### bg_team_setxy
```
*bg_team_setxy <Battle Group ID>,<x>,<y>;
```

Updates the respawn point of the given Battle Group to x,y on the same map. <Battle Group ID> can be retrieved using getcharid(4).

Example:
	bg_team_setxy getcharid(4),56,212;
	mapannounce "bat_a01", "Group [1] has taken the work shop, and will now respawn there.",bc_map,"0xFFCE00";
	end;
