
### sc_start
```
*sc_start <effect type>,<ticks>,<value 1>{,<rate>,<flag>{,<GID>}};
*sc_start2 <effect type>,<ticks>,<value 1>,<value 2>{,<rate>,<flag>{,<GID>}};
*sc_start4 <effect type>,<ticks>,<value 1>,<value 2>,<value 3>,<value 4>{,<rate>,<flag>{,<GID>}};
*sc_end <effect type>{,<GID>};
*sc_end_class {<char_id>{,<job_id>}};
```

These commands will bestow a status effect on a character.

The `<effect type>` determines which status is invoked. This can be either a number
or constant, with the common statuses (mostly negative) found in 'src/map/script_constants.hpp'
with the 'SC_' prefix. A full list is located in 'src/map/status.hpp', though
they are not currently documented.

The duration of the status is given in `<ticks>`, or milleseconds.
Use INFINITE_TICK for infinite duration.

Certain status changes take an additional parameter `<value 1>`, which typically
modifies player stats by the given number or percentage. This differs for each
status, and is sometimes zero.

Optional value `<rate>` is the chance that the status will be invoked (100 = 1%).
This is used primarily in item scripts. When used in an NPC script, a flag MUST
be defined for the rate to work.

Optional value `<flag>` is how the status change start will be handled (a bitmask).
* `SCSTART_NOAVOID`   : Status change cannot be avoided.
* `SCSTART_NOTICKDEF` : Tick cannot be reduced by stats (default).
* `SCSTART_LOADED`    : sc_data loaded, so no value will be altered.
* `SCSTART_NORATEDEF` : Rate cannot be reduced.
* `SCSTART_NOICON`    : Status icon won't be sent to client

If a `<GID>` is given, the status change will be invoked on the specified character
instead of the one attached to the script. This can only be defined after setting
a rate and flag.

'sc_start2' and 'sc_start4' allow extra parameters to be passed, and are used only
for effects that require them. The meaning of the extra values vary depending on the
effect type. For more infos, read status_change.txt containing a list of all Status Changes
and theirs val1, val2, val3, and val4 usage in source.

'sc_end' will remove a specified status effect. If SC_ALL (-1) is given, it will
perform a complete removal of all statuses (although permanent ones will re-apply).

'sc_end_class' works like 'sc_end' but will remove all status effects from any learned
skill on the invoking character. If `<job_id>` is provided it will end the effect for that job.

Examples:
```c
// This will poison the invoking character for 10 minutes at 50% chance.
sc_start SC_POISON,600000,0,5000;

// This will bestow the effect of Level 10 Blessing.
sc_start SC_BLESSING,240000,10;

// Adjust element resistance by percentage. Sample with Resist_Fire item script:
// val1: Water resistance
// val2: Earth resistance
// val3: Fire resistance
// val4: Wind resistance
sc_start4 SC_ARMOR_ELEMENT,1200000,-15,0,20,0;

// This will end the Freezing status for the invoking character.
sc_end SC_FREEZE;

// This will end the effect of any learned skill for the invoking character.
sc_end_class;

// This will end the effect of any learned skill for the character with the <char_id> 150000.
// val1: <char_id>
sc_end_class(150000);

// This will end the effect of any Arch Bishop skill for the invoking character.
// val1: <char_id>
// val2: <job_id> of Arch Bishop
sc_end_class(getcharid(0),Job_Arch_Bishop);
```

Note: to use SC_NOCHAT you should alter Manner
```c
set Manner, -5;	// Will mute a user for 5 minutes
set Manner, 0;	// Will unmute a user
set Manner, 5;	// Will unmute a user and prevent the next use of 'Manner'
```
