### instance_warpall
```
*instance_warpall "<map name>",<x>,<y>{,<instance id>};
```

Warps all players in the `<instance id>` to `<map name>` to the given coordinates.
If no ID is specified, the IM_PARTY instance the invoking player is attached
to is used. If that fails, the script will come to a halt.
