### eaclass
```
*eaclass({<job number>,<char_id>})
```

This commands returns the "eA job-number" corresponding to the given class, and
uses the invoking player's class if none is given. The eA job-number is also a
class number system, but it's one that comes with constants which make it easy
to convert among classes. The command will return -1 if you pass it a job number
which doesn't have an eA job-number equivalent.

```c
.@eac = eaclass();
if ((.@eac&EAJ_BASEMASK) == EAJ_SWORDMAN)
    mes "Your base job is Swordman.";
if (.@eac&EAJL_UPPER)
    mes "You are a rebirth job.";
if ((.@eac&EAJ_UPPERMASK) == EAJ_SWORDMAN)
    mes "You must be a Swordman, Baby Swordman or High Swordman.";
```

For more information on the eA Job System, see the docs/ea_job_system.txt file.
