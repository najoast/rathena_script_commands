
### getunitdata
```
*getunitdata <GID>,<arrayname>;
```
### setunitdata
```
*setunitdata <GID>,<parameter>,<new value>;
```

This is used to get and set special data related to the unit.
With getunitdata, the array given will be filled with the current data. In setunitdata
the indexes in the array would be used to set that data on the unit.

Both getunitdata and setunitdata will return -1 if the given GID does not exist.

Note: When adjusting a unit's stat (STR, AGI, etc) the unit's respective statuses are
      recalculated (HIT, FLEE, etc) automatically. Keep in mind that some stats don't
	  affect a unit's status and will have to directly be modified.

Parameters (indexes) for monsters are:
* UMOB_SIZE
* UMOB_LEVEL
* UMOB_HP
* UMOB_MAXHP
* UMOB_MASTERAID
* UMOB_MAPID
* UMOB_X
* UMOB_Y
* UMOB_SPEED
* UMOB_MODE
* UMOB_AI
* UMOB_SCOPTION
* UMOB_SEX
* UMOB_CLASS
* UMOB_HAIRSTYLE
* UMOB_HAIRCOLOR
* UMOB_HEADBOTTOM
* UMOB_HEADMIDDLE
* UMOB_HEADTOP
* UMOB_CLOTHCOLOR
* UMOB_SHIELD
* UMOB_WEAPON
* UMOB_LOOKDIR
* UMOB_CANMOVETICK
* UMOB_STR
* UMOB_AGI
* UMOB_VIT
* UMOB_INT
* UMOB_DEX
* UMOB_LUK
* UMOB_SLAVECPYMSTRMD
* UMOB_DMGIMMUNE
* UMOB_ATKRANGE
* UMOB_ATKMIN
* UMOB_ATKMAX
* UMOB_MATKMIN
* UMOB_MATKMAX
* UMOB_DEF
* UMOB_MDEF
* UMOB_HIT
* UMOB_FLEE
* UMOB_PDODGE
* UMOB_CRIT
* UMOB_RACE
* UMOB_ELETYPE
* UMOB_ELELEVEL
* UMOB_AMOTION
* UMOB_ADELAY
* UMOB_DMOTION
* UMOB_TARGETID

-----

Parameter (indexes) for homunculi are:
* UHOM_SIZE
* UHOM_LEVEL
* UHOM_HP
* UHOM_MAXHP
* UHOM_SP
* UHOM_MAXSP
* UHOM_MASTERCID
* UHOM_MAPID
* UHOM_X
* UHOM_Y
* UHOM_HUNGER
* UHOM_INTIMACY
* UHOM_SPEED
* UHOM_LOOKDIR
* UHOM_CANMOVETICK
* UHOM_STR
* UHOM_AGI
* UHOM_VIT
* UHOM_INT
* UHOM_DEX
* UHOM_LUK
* UHOM_DMGIMMUNE
* UHOM_ATKRANGE
* UHOM_ATKMIN
* UHOM_ATKMAX
* UHOM_MATKMIN
* UHOM_MATKMAX
* UHOM_DEF
* UHOM_MDEF
* UHOM_HIT
* UHOM_FLEE
* UHOM_PDODGE
* UHOM_CRIT
* UHOM_RACE
* UHOM_ELETYPE
* UHOM_ELELEVEL
* UHOM_AMOTION
* UHOM_ADELAY
* UHOM_DMOTION
* UHOM_TARGETID

-----

Parameter (indexes) for pets are:
* UPET_SIZE
* UPET_LEVEL
* UPET_HP
* UPET_MAXHP
* UPET_MASTERAID
* UPET_MAPID
* UPET_X
* UPET_Y
* UPET_HUNGER
* UPET_INTIMACY
* UPET_SPEED
* UPET_LOOKDIR
* UPET_CANMOVETICK
* UPET_STR
* UPET_AGI
* UPET_VIT
* UPET_INT
* UPET_DEX
* UPET_LUK
* UPET_DMGIMMUNE
* UPET_ATKRANGE
* UPET_ATKMIN
* UPET_ATKMAX
* UPET_MATKMIN
* UPET_MATKMAX
* UPET_DEF
* UPET_MDEF
* UPET_HIT
* UPET_FLEE
* UPET_PDODGE
* UPET_CRIT
* UPET_RACE
* UPET_ELETYPE
* UPET_ELELEVEL
* UPET_AMOTION
* UPET_ADELAY
* UPET_DMOTION

-----

Parameter (indexes) for mercenaries are:
* UMER_SIZE
* UMER_HP
* UMER_MAXHP
* UMER_MASTERCID
* UMER_MAPID
* UMER_X
* UMER_Y
* UMER_KILLCOUNT
* UMER_LIFETIME
* UMER_SPEED
* UMER_LOOKDIR
* UMER_CANMOVETICK
* UMER_STR
* UMER_AGI
* UMER_VIT
* UMER_INT
* UMER_DEX
* UMER_LUK
* UMER_DMGIMMUNE
* UMER_ATKRANGE
* UMER_ATKMIN
* UMER_ATKMAX
* UMER_MATKMIN
* UMER_MATKMAX
* UMER_DEF
* UMER_MDEF
* UMER_HIT
* UMER_FLEE
* UMER_PDODGE
* UMER_CRIT
* UMER_RACE
* UMER_ELETYPE
* UMER_ELELEVEL
* UMER_AMOTION
* UMER_ADELAY
* UMER_DMOTION
* UMER_TARGETID

-----

Parameter (indexes) for elementals are:
* UELE_SIZE
* UELE_HP
* UELE_MAXHP
* UELE_SP
* UELE_MAXSP
* UELE_MASTERCID
* UELE_MAPID
* UELE_X
* UELE_Y
* UELE_LIFETIME
* UELE_MODE
* UELE_SPEED
* UELE_LOOKDIR
* UELE_CANMOVETICK
* UELE_STR
* UELE_AGI
* UELE_VIT
* UELE_INT
* UELE_DEX
* UELE_LUK
* UELE_DMGIMMUNE
* UELE_ATKRANGE
* UELE_ATKMIN
* UELE_ATKMAX
* UELE_MATKMIN
* UELE_MATKMAX
* UELE_DEF
* UELE_MDEF
* UELE_HIT
* UELE_FLEE
* UELE_PDODGE
* UELE_CRIT
* UELE_RACE
* UELE_ELETYPE
* UELE_ELELEVEL
* UELE_AMOTION
* UELE_ADELAY
* UELE_DMOTION
* UELE_TARGETID

-----

Parameter (indexes) for NPCs are:
* UNPC_DISPLAY
* UNPC_LEVEL
* UNPC_HP
* UNPC_MAXHP
* UNPC_MAPID
* UNPC_X
* UNPC_Y
* UNPC_LOOKDIR
* UNPC_STR
* UNPC_AGI
* UNPC_VIT
* UNPC_INT
* UNPC_DEX
* UNPC_LUK
* UNPC_PLUSALLSTAT
* UNPC_DMGIMMUNE
* UNPC_ATKRANGE
* UNPC_ATKMIN
* UNPC_ATKMAX
* UNPC_MATKMIN
* UNPC_MATKMAX
* UNPC_DEF
* UNPC_MDEF
* UNPC_HIT
* UNPC_FLEE
* UNPC_PDODGE
* UNPC_CRIT
* UNPC_RACE
* UNPC_ELETYPE
* UNPC_ELELEVEL
* UNPC_AMOTION
* UNPC_ADELAY
* UNPC_DMOTION

### Notes
*Notes:
```
    - *_SIZE: small (0); medium (1); large (2)
    - *_MAPID: this refers to the map_data index (from src/map/map.cpp), not the mapindex_db index (from src/common/mapindex.cpp)
        -- For 'setunitdata', map name can also be passed in as a valid value instead of map ID
    - *_SPEED: 20 - 1000
    - *_MODE: see doc/mob_db_mode_list.txt
    - *_LOOKDIR: north (0), northwest (1), west (2), etc
    - *_CANMOVETICK: seconds * 1000 the unit will be unable to move
    - *_DMGIMMUNE: unit will be immune to damage (1), or will receive damage (0)
    - *_HUNGER: 0 - 100
    - *_INTIMACY: 0 - 1000
    - *_LIFETIME: seconds * 1000 the unit will be 'alive' for
    - *_AMOTION: see doc/mob_db.txt
    - *_ADELAY: see doc/mob_db.txt
    - *_DMOTION: see doc/mob_db.txt

    - UMOB_AI: none (0); attack (1); marine sphere (2); flora (3); zanzou (4); legion (5); faw (6)
    - UMOB_SCOPTION: see the 'Variables' section at the top of this document
    - UMOB_SLAVECPYMSTRMD: make the slave copy the master's mode (1), or not (0)

    - UNPC_PLUSALLSTAT: same as 'bAllStats'; increases/decreses all stats by given amount
```

Example:
```c
// Spawn some Porings and save the Game ID.
// - Keep in mind, when the 'monster' script command is used,
// - all the spawned monster GID's are stored in an array
// - called $@mobid[].
monster "prontera",149,190,"Poring",1002,10;
.GID = $@mobid[9]; // Store and modify the 10th Poring spawned to make him stronger!

// Save the strong Poring's mob data in the .@por_arr[] variable. (.@por_arr[1] being level, .@por_arr[13] being class, etc.)
// With this data we can have the NPC display or manipulate it how we want. This does not have to be ran before 'setunitdata'.
getunitdata .GID,.@por_arr;

// Set the max HP of the Poring to 1000 (current HP will also get updated to 1000).
setunitdata .GID,UMOB_MAXHP,1000;
```
