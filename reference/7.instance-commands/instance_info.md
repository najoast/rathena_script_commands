### instance_info
```
*instance_info("<instance name>",<info type>{,<instance_db map index>});
```

Returns the specified `<info type>` of the given `<instance name>` from the instance database.
If the `<instance name>` is unknown or an invalid `<info type>` is supplied -1 will be returned.

Valid info types:
* IIT_ID: Instance database ID as integer.
* IIT_TIME_LIMIT: Instance database total life time as integer.
* IIT_IDLE_TIMEOUT: Instance database timeout time as integer.
* IIT_ENTER_MAP: Instance database enter map as string.
* IIT_ENTER_X: Instance database enter X location as integer.
* IIT_ENTER_Y: Instance database enter Y location as integer.
* IIT_MAPCOUNT: Instance database total maps as integer.
* IIT_MAP: Instance database map name from the given `<instance_db map index>` as string.  If the index is invalid an empty string will be returned.

Example:

```c
.@name$ = "Endless Tower";
mes .@name$ + " will be destroyed if no one is in the instance for " + instance_info(.@name$,IIT_IDLETIMEOUT) + " seconds.";
// Endless Tower will be destroyed if no one is in the instance for 300 seconds.
```
