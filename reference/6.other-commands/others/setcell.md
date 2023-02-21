### setcell
```
*setcell "<map name>",<x1>,<y1>,<x2>,<y2>,<type>,<flag>;
```

Each map cell has several 'flags' that specify the properties of that cell.
These include terrain properties (walkability, shootability, presence of water),
skills (basilica, land protector, ...) and other (NPC nearby, no vending, ...).
Each of these can be 'on' or 'off'. Together they define a cell's behavior.

This command lets you alter these flags for all map cells in the specified
(x1,y1)-(x2,y2) rectangle. The 'flag' can be 0 or 1 (0:clear flag, 1:set flag).
The 'type' defines which flag to modify. Possible options see 'src/map/script_constants.hpp'.

Example:

```c
setcell "arena",0,0,300,300,cell_basilica,1;
setcell "arena",140,140,160,160,cell_basilica,0;
setcell "arena",135,135,165,165,cell_walkable,0;
setcell "arena",140,140,160,160,cell_walkable,1;
```

This will add a makeshift ring into the center of the map. The ring will be
surrounded by a 5-cell wide 'gap' to prevent interference from outside, and
the rest of the map will be marked as 'basilica', preventing observers from
casting any offensive skills or fighting among themselves. Note that the wall
will not be shown nor known client-side, which may cause movement problems.

Another example:

```c
OnBarricadeDeploy:
	setcell "schg_cas05",114,51,125,51,cell_walkable,0;
	end;
OnBarricadeBreak:
	setcell "schg_cas05",114,51,125,51,cell_walkable,1;
	end;
```

This could be a part of the WoE:SE script, where attackers are not allowed
to proceed until all barricades are destroyed. This script would place and
remove a nonwalkable row of cells after the barricade mobs.
