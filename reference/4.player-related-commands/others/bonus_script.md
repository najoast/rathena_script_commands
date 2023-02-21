### bonus_script
```
*bonus_script "<script code>",<duration>{,<flag>{,<type>{,<status_icon>{,<char_id>}}}};
```

This command will attach a script to a player for a given duration, in seconds.
After that time, the script will automatically expire. The same bonus cannot be
stacked. By default, this bonus will be stored on `bonus_script` table when player
logs out.

Flags (bitmask):
* 1   : Remove when dead.
* 2   : Removable by Dispell.
* 4   : Removable by Clearance.
* 8   : Remove when player logs out.
* 16  : Removeable by Banishing Buster.
* 32  : Removable by Refresh.
* 64  : Removable by Lux Anima.
* 128 : Remove when Madogear is activated or deactivated.
* 256 : Remove when receive damage.
* 512 : Script is permanent, cannot be cleared by bonus_script_clear.
* 1024: Force to replace duplicated script by expanding the duration.
* 2048: Force to add duplicated script. This flag cannot be stacked with 1024, if both are defined, 1024 will be checked first and ignore this flag.

Types:
	This will be used to decide negative or positive buff for 'debuff_on_logout'.
* 0: Ignore the buff type and won't be removed if the flag is not &8 (Default)
* 1: Buff
* 2: Debuff

Status_icon: See "Status Icon" section in 'src/map/script_constants.hpp'. Default is SI_BLANK (-1).

Example:
```c
// Apple gives you +5 Str bonus for 1 minute when it's consumed.
512,Apple,Apple,0,15,,20,,,,,0xFFFFFFFF,63,2,,,,,,{ bonus_script "{ bonus bStr,5; }",60; },{},{}
```
