### jobchange
```
*jobchange <job number>{,<upper flag>,<char_id>};
```

This command will change the job class of the invoking character.

```c
jobchange 1; // This would change your player into a Swordman
jobchange 4002; // This would change your player into a Swordman High
```

This command does work with numbers, but you can also use job names. The full
list of job names and the numbers they correspond to can be found in
'src/map/script_constants.hpp'.

```c
// This would change your player into a Swordman
jobchange Job_Swordman;
// This would change your player into a Swordman High
jobchange Job_Swordman_High;
```

'upper flag' can alternatively be used to specify the type of job one changes
to. For example, jobchange Job_Swordman,1; will change the character to a high
swordsman. The upper values are:
* -1 (or when omitted): preserves the current job type.
* 0: Normal/standard classes
* 1: High/Advanced classes
* 2: Baby classes

This command will also set a permanent character-based variable
'jobchange_level' which will contain the job level at the time right before
changing jobs, which can be checked for later in scripts.
