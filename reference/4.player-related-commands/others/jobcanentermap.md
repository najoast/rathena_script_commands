### jobcanentermap
```
*jobcanentermap("<mapname>"{,<JobID>});
```

Return true if player (decided by job) can enter the map, false otherwise.

For optional 'JobID', see constant of Job_*, or use player's Class, BaseJob,
and BaseClass. If no player is attached, this param must have a value.

See also `db/[pre-]re/job_noenter_map.txt`
