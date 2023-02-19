[TOC]

This document is a reference manual for all the scripting commands and functions
available in rAthena. It is not a simple tutorial. When people tell you to
"Read The F***ing Manual", they mean this.

This is not a place to teach you basic programming. This document will not teach
you basic programming by itself. It's more of a reference for those who have at
least a vague idea of what they want to do and want to know what tools they have
available to do it. We've tried to keep it as simple as possible, but if you
don't understand it, getting a clear book on programming in general will help
better than yelling around the forum for help.

A little learning never caused anyone's head to explode.

# Structure

The script commands are listed in no particular order, but are grouped by
relative function.

## Name
*Name of the command and parameters (if any).

Descriptive text

	Small example if possible. Will usually be incomplete, it's there just to
	give you an idea of how it works in practice.

To find a specific command, use Ctrl+F, (or whatever keys call up a search
function in whatever you're reading this with) put an asterisk (*) followed by the command
name, and it should find the command description for you.

If you find anything missing, please let us know!

# Syntax

Throughout this document, wherever a command wants an argument, it is given in
<angle brackets>. This doesn't mean you should type the angle brackets. If an
argument of a command is optional, it is given in {curly brackets}. You've
doubtlessly seen this convention somewhere. If a command can optionally take
an unspecified number of arguments, you'll see a list like this:
```
command <argument>{,<argument>...<argument>}
```
This still means they will want to be separated by commas.

Where a command wants a string, it will be given in "quotes", if it's a number,
it will be given without them. Normally, you can put an expression, like a bunch
of functions or operators returning a value, in (round brackets) instead of most
numbers. Round brackets will not always be required, but they're often a good
idea.

Wherever you refer to a map name, it's always 'map name' (.gat suffix is deprecated).


# Script loading structure

Scripts are loaded by the map server as referenced in the 'conf/map_athena.conf'
configuration file, but in the default configuration, it doesn't load any script
files itself. Instead, it loads the file 'npc/(pre-)re/scripts_main.conf' which itself
contains references to other files. The actual scripts are loaded from txt
files, which are linked up like this:

```
npc: <path to a filename>
```

Any line like this, invoked, ultimately, by 'map_athena.conf' will load up the
script contained in this file, which will make the script available. No file
will get loaded twice to prevent possible errors.

Another configuration file option of relevance is:
```
delnpc: <path to a filename>
```
This will unload a specified script filename from memory, which, while
seemingly useless, may sometimes be required.

Whenever '//' is encountered in a line upon reading, everything beyond this on
that line is considered to be a comment and is ignored. This works wherever you
place it.
```
// This line will be ignored when processing the script.
```
Block comments can also be used, where you can place /* and */ between any text you
wish rAthena to ignore.

Example:
```
/* This text,
 * no matter which new line you start
 * is ignored, until the following
 * symbol is encountered: */
```

# Top-level commands

The asterisks (`*`) in front of each line is a personal preference and is not required.

Upon loading all the files, the server will execute all the top-level commands
in them. No variables exist yet at this point, no commands can be called other
than those given in this section. These commands set up the basic structure - create
NPC objects, spawn monster objects, set map flags, etc. No code is actually
executed at this point. The top-level commands are pretty confusing, since
they aren't structured like you would expect (command name first), but rather,
normally start with a map name.

What's more confusing about the top-level commands is that most of them use a
tab symbol to divide their arguments.

To prevent problems and confusion, the tab symbols are written as '%TAB%'
throughout this document, even though this makes the text a bit less readable.
Using an invisible symbol to denote arguments is one of the bad things about
this language.

Here is a list of valid top-level commands:
## Set a map flag
```
<map name>%TAB%mapflag%TAB%<flag>
```
This will, upon loading, set a specified map flag on a map you like. These are
normally in files inside 'npc/mapflag' and are loaded first, so by the time the
server's up, all the maps have the flags they should have. Map flags determine
the behavior of the map in various situations. For more details, see 'setmapflag'
and 'doc/mapflags.txt'.

## Create a permanent monster spawn:
```
<map name>{,<x>{,<y>{,<xs>{,<ys>}}}}%TAB%monster%TAB%<monster name>{,<monster level>}%TAB%<mob id>,<amount>{,<delay1>{,<delay2>{,<event>{,<mob size>{,<mob ai>}}}}}
```
Map name is the name of the map the monsters will spawn on. x,y are the
coordinates where the mob should spawn. If xs and ys are non-zero, they
specify the 'radius' of a spawn-rectangle area centered at x,y.
Putting zeros instead of these coordinates will spawn the monsters randomly.
Note this is only the initial spawn zone, as mobs random-walk, they are free
to move away from their specified spawn region.

Monster name is the name the monsters will have on screen, and has no relation
whatsoever to their names anywhere else. It's the mob id that counts, which
identifies monster record in 'mob_db.txt' database of monsters. If the mob name
is given as "--ja--", the 'japanese name' field from the monster database is
used, (which, in rAthena, actually contains an English name) if it's "--en--",
it's the 'english name' from the monster database (which contains an uppercase
name used to summon the monster with a GM command).

You can specify a custom level to use for the mob different from the one of
the database by adjoining the level after the name with a comma. eg:
"Poring,50" for a name will spawn a monster with name Poring and level 50.

Amount is the amount of monsters that will be spawned when this command is
executed, it is affected by spawn rates in 'battle_athena.conf'.

Delay1 and delay2 control monster respawn delays - the first one is the fixed
base respawn time, and the second is random variance on top of the base time.
Both values are given in milliseconds (1000 = 1 second).
Note that the server also enforces a minimum respawn delay of 5 seconds.

Event is a script event to be executed when the mob is killed. The event must
be in the form "NPCName::OnEventName" to execute, and the event name label
should start with "On". As with all events, if the NPC is an on-touch NPC, the
player who triggers the script must be within 'trigger' range for the event to
work.

There are two optional fields for monster size and AI.
```
<mob size> can be:
	Size_Small	(0)
	Size_Medium	(1)
	Size_Large	(2)

<mob ai> can be:
	AI_NONE		(0)		(default)
	AI_ATTACK	(1)		(attack/friendly)
	AI_SPHERE	(2)		(Alchemist skill)
	AI_FLORA	(3)		(Alchemist skill)
	AI_ZANZOU	(4)		(Kagerou/Oboro skill)
	AI_LEGION	(5)		(Sera skill)
	AI_FAW		(6)		(Mechanic skill)
```
Alternately, a monster spawned using 'boss_monster' instead of 'monster' is able
to be detected on the map with the `SC_BOSSMAPINFO` status (used by Convex Mirror).

## NPC names

/!\ WARNING: this applies to warps, NPCs, duplicates and shops /!\

NPC names are kinda special and are formatted this way:
```
<Display name>{::<Unique name>}
```
All NPCs need to have a unique name that is used for identification purposes.
When you have to identify a NPC by its name, you should use <Unique name>.
If <Unique name> is not provided, use <Display name> instead.

The client has a special feature when displaying names:
if the display name contains a '#' character, it hides that part of the name.
ex: if your NPC is named 'Hunter#hunter1', it will be displayed as 'Hunter'

<Display name> must be at most 24 characters in length.
<Unique name> must be at most 24 characters in length.

## Define a warp point
```
<from mapname>,<fromX>,<fromY>,<facing>%TAB%warp%TAB%<warp name>%TAB%<spanx>,<spany>,<to mapname>,<toX>,<toY>
<from mapname>,<fromX>,<fromY>,<facing>%TAB%warp2%TAB%<warp name>%TAB%<spanx>,<spany>,<to mapname>,<toX>,<toY>
```
This will define a warp NPC that will warp a player between maps, and while most
arguments of that are obvious, some deserve special mention.

SpanX and SpanY will make the warp sensitive to a character who didn't step
directly on it, but walked into a zone which is centered on the warp from
coordinates and is SpanX in each direction across the X axis and SpanY in each
direction across the Y axis.

Warp NPC objects also have a name, because you can use it to refer to them later
with 'enablenpc'/'disablenpc'

Facing of a warp object is irrelevant, it is not used in the code and all
current scripts have a zero in there.

Unlike 'warp', 'warp2' will also be triggered by hidden player.

## Define an NPC object.
```
<map name>,<x>,<y>,<facing>%TAB%script%TAB%<NPC Name>%TAB%<sprite id>,{<code>}
<map name>,<x>,<y>,<facing>%TAB%script%TAB%<NPC Name>%TAB%<sprite id>,<triggerX>,<triggerY>,{<code>}
```
This will place an NPC object on a specified map at the specified location, and
is a top-level command you will use the most in your custom scripting. The NPCs
are triggered by clicking on them, and/or by walking in their trigger area, if
defined, see that below.

Facing is a direction the NPC sprite will face in. Not all NPC sprites have
different images depending on the direction you look from, so for some facing
will be meaningless. Facings are counted counterclockwise in increments of 45
degrees, where 0 means facing towards the top of the map. (So to turn the sprite
towards the bottom of the map, you use facing 4, and to make it look southeast
it's facing 5.)

Sprite ID is the sprite number or constant used to display this particular NPC.
You may also use a monster's ID instead to display a monster sprite for this NPC.
It is possible to use a job sprite as well, but you must first define it as a
monster sprite in 'mob_avail.txt', a full description on how to do this is not
in the scope of this manual.
A '-1' Sprite ID will make the NPC invisible (and unclickable).
A '111' Sprite ID will make an NPC which does not have a sprite, but is still
clickable, which is useful if you want to make a clickable object of the 3D
terrain.

TriggerX and triggerY, if given, will define an area, centered on NPC and
spanning triggerX cells in every direction across X and triggerY in every
direction across Y. Walking into that area will trigger the NPC. If no
'OnTouch:' special label is present in the NPC code, the execution will start
from the beginning of the script, otherwise, it will start from the 'OnTouch:'
label. Monsters can also trigger the NPC, though the label 'OnTouchNPC:' is
used in this case.

The code part is the script code that will execute whenever the NPC is
triggered. It may contain commands and function calls, descriptions of which
compose most of this document. It has to be in curly brackets, unlike elsewhere
where we use curly brackets, these do NOT signify an optional parameter.

## Define a 'floating' NPC object.
```
-%TAB%script%TAB%<NPC Name>%TAB%-1,{<code>}
```
This will define an NPC object not triggerable by normal means. This would
normally mean it's pointless since it can't do anything, but there are
exceptions, mostly related to running scripts at specified time, which is what
these floating NPC objects are for. More on that below.

## Define a shop/cashshop/itemshop/pointshop NPC.
```
-%TAB%shop%TAB%<NPC Name>%TAB%<sprite id>,<itemid>:<price>{,<itemid>:<price>...}
<map name>,<x>,<y>,<facing>%TAB%shop%TAB%<NPC Name>%TAB%<sprite id>,<itemid>:<price>{,<itemid>:<price>...}

-%TAB%cashshop%TAB%<NPC Name>%TAB%<sprite id>,<itemid>:<price>{,<itemid>:<price>...}
<map name>,<x>,<y>,<facing>%TAB%cashshop%TAB%<NPC Name>%TAB%<sprite id>,<itemid>:<price>{,<itemid>:<price>...}

-%TAB%itemshop%TAB%<NPC Name>%TAB%<sprite id>,<costitemid>{:<discount>},<itemid>:<price>{,<itemid>:<price>...}
<map name>,<x>,<y>,<facing>%TAB%itemshop%TAB%<NPC Name>%TAB%<sprite id>,<costitemid>{:<discount>},<itemid>:<price>{,<itemid>:<price>...}

-%TAB%pointshop%TAB%<NPC Name>%TAB%<sprite id>,<costvariable>{:<discount>},<itemid>:<price>{,<itemid>:<price>...}
<map name>,<x>,<y>,<facing>%TAB%pointshop%TAB%<NPC Name>%TAB%<sprite id>,<costvariable>{:<discount>},<itemid>:<price>{,<itemid>:<price>...}

<map name>,<x>,<y>,<facing>%TAB%marketshop%TAB%<NPC Name>%TAB%<sprite id>,<itemid>:<price>:<quantity>{,<itemid>:<price>:<quantity>...}
```
This will define a shop NPC, which, when triggered (which can only be done by
clicking) will cause a shop window to come up. No code whatsoever runs in shop
NPCs and you can't change the prices otherwise than by editing the script
itself.

The Item ID is the number of item in the 'item_db.txt' database. If Price is set
to -1, the 'buy price' given in the item database will be used. Otherwise, the
price you gave will be used for this item, which is how you create differing
prices for items in different shops.

There are other types of shops available:
cashshop - use "cashshop" in place of "shop" to use the Cash Shop interface, allowing
you to buy items with special points that are stored as account variables
called  #CASHPOINTS and #KAFRAPOINTS. This type of shop will not allow you to sell
items at it, only make purchases. The layout used to define sale items still count, and
"<price>" refers to how many points will be spent purchasing the them.

"itemshop" and "pointshop" use the Shop interface, allowing you to buy items with a specific
item or special points from a variable. 'pointshop' only supports permanent character variables,
temporary character variables, permanent local account variables or permanent global account
variables. These variables must be of integer type, not string. 'discount' flag is an
optional value which makes the price at that shop become affected by discount skill.

## Define an warp/shop/cashshop/itemshop/pointshop/NPC duplicate.
```
warp/warp2: <map name>,<x>,<y>,<facing>%TAB%duplicate(<label>)%TAB%<NPC Name>%TAB%<spanx>,<spany>
shop/cashshop/itemshop/pointshop/npc: -%TAB%duplicate(<label>)%TAB%<NPC Name>%TAB%<sprite id>
shop/cashshop/itemshop/pointshop/npc: <map name>,<x>,<y>,<facing>%TAB%duplicate(<label>)%TAB%<NPC Name>%TAB%<sprite id>
npc: -%TAB%duplicate(<label>)%TAB%<NPC Name>%TAB%<sprite id>,<triggerX>,<triggerY>
npc: <map name>,<x>,<y>,<facing>%TAB%duplicate(<label>)%TAB%<NPC Name>%TAB%<sprite id>,<triggerX>,<triggerY>
```
This will duplicate an warp/shop/cashshop/itemshop/pointshop/NPC referred to by 'label'.
Warp duplicates inherit the target location.
Shop/cashshop/itemshop/pointshop duplicates inherit the item list.
NPC duplicates inherit the script code.
The rest (name, location, facing, sprite ID, span/trigger area)
is obtained from the definition of the duplicate (not inherited).

## Define a function object
```
function%TAB%script%TAB%<function name>%TAB%{<code>}
```
This will define a function object, callable with the 'callfunc' command (see
below). This object will load on every map server separately, so you can get at
it from anywhere. It's not possible to call the code in this object by
anything other than the 'callfunc' script command.

The code part is the script code that will execute whenever the function is
called with 'callfunc'. It has to be in curly brackets, unlike elsewhere where
we use curly brackets, these do NOT signify an optional parameter.

Once an object is defined which has a 'code' field to its definition, it
contains script commands which can actually be triggered and executed.

# ~ RID? GID? ~

## What a RID is and why do you need to know

Most scripting commands and functions will want to request data about a
character, store variables referenced to that character, send stuff to the
client connected to that specific character. Whenever a script is invoked by a
character, it is passed a so-called RID - this is the account ID number of a
character that caused the code to execute by clicking on it, walking into its
OnTouch zone, or otherwise.

If you are only writing common NPCs, you don't need to bother with it. However,
if you use functions, if you use timers, if you use clock-based script
activation, you need to be aware of all cases when a script execution can be
triggered without a RID attached. This will make a lot of commands and functions
unusable, since they want data from a specific character, want to send stuff to
a specific client, want to store variables specific to that character, and they
would not know what character to work on if there's no RID.

Unless you use 'attachrid' to explicitly attach a character to the script first.

Whenever we say 'invoking character', we mean 'the character who's RID is
attached to the running script. The script function "playerattached" can be
used to check which is the currently attached player to the script (it will
return 0 if the there is no player attached or the attached player no longer
is logged on to the map-server).

## But what about GID?

GID stands for the Game ID of something, this can either be the GID obtained
through mobspawn (mob control commands) or the account ID of a character.
Another way would be to right click on a mob, NPC or char as GM sprited char
to view the GID.

See also 'getpetinfo', 'getmercinfo', 'gethominfo', and 'geteleminfo'.

This is mostly used for the new version of skill and the mob control commands
implemented.

# Item and pet scripts

Each item in the item database has three special fields - Script , OnEquip_Script
and OnUnequip_Script. The first is script code run every time a character equips the item,
with the RID of the equipping character. Every time they unequip an item, all
temporary bonuses given by the script commands are cleared, and all the scripts
are executed once again to rebuild them. This also happens in several other
situations (like upon login) but the full list is currently unknown.

OnEquip_Script is a piece of script code run whenever the item is used by a character
by double-clicking on it. OnUnequip_Script runs whenever the
equipment is unequip by a character

Not all script commands work properly in the item scripts. Where commands and
functions are known to be meant specifically for use in item scripts, they are
described as such.

Every pet in the pet database has a PetScript field, which determines pet
behavior. It is invoked wherever a pet of the specified type is spawned.
(hatched from an egg, or loaded from the char server when a character who had
that pet following them connects) This may occur in some other situations as
well. Don't expect anything other than commands definitely marked as usable in
pet scripts to work in there reliably.

# Numbers

Beside the common decimal numbers, which are nothing special whatsoever (though
do not expect to use fractions, since ALL numbers are integer in this language),
the script engine also handles hexadecimal numbers, which are otherwise
identical. Writing a number like '0x<hex digits>' will make it recognized as a
hexadecimal value. Notice that 0x10 is equal to 16. Also notice that if you try
to 'mes 0x10' it will print '16'.

Number values can't exceed the limits of an integer variable: Any number
greater than INT_MAX (2147483647) or smaller than INT_MIN (-2147483648) will
be capped to those values and will cause a warning to be reported.

# Variables

## Basic
The meat of every programming language is variables - places where you store
data.

In the rAthena scripting language, variable names are not case sensitive.

Variables are divided into and uniquely identified by the combination of:
* prefix  - determines the scope and extent (or lifetime) of the variable
* name    - an identifier consisting of '_' and alphanumeric characters
* postfix - determines the type of the variable: integer or string

Scope can be:
* global    - global to all servers
* local     - local to the server
* account   - attached to the account of the character identified by RID
* character - attached to the character identified by RID
* npc       - attached to the NPC
* scope     - attached to the scope of the instance

Extent can be:
* permanent - They still exist when the server resets.
* temporary - They cease to exist when the server resets.

```
Prefix: scope and extent
nothing  - A permanent variable attached to the character, the default variable
           type. They are stored by char-server in the `char_reg_num` and
           `char_reg_str`.
"@"      - A temporary variable attached to the character.
           SVN versions before 2094 revision and RC5 version will also treat
           'l' as a temporary variable prefix, so beware of having variable
           names starting with 'l' if you want full backward compatibility.
"$"      - A global permanent variable.
           They are stored by map-server in database table `mapreg`.
"$@"     - A global temporary variable.
           This is important for scripts which are called with no RID
           attached, that is, not triggered by a specific character object.
"."      - A NPC variable.
           They exist in the NPC and disappear when the server restarts or the
           NPC is reloaded. Can be accessed from inside the NPC or by calling
           'getvariableofnpc'. Function objects can also have .variables which
           are accessible from inside the function, however 'getvariableofnpc'
           does NOT work on function objects.
".@"     - A scope variable.
           They are unique to the instance and scope. Each instance has its
           own scope that ends when the script ends. Calling a function with
           callsub/callfunc starts a new scope, returning from the function
           ends it. When a scope ends, its variables are converted to values
           ('return .@var;' returns a value, not a reference).
"'"      - An instance variable.
           These are used with the instancing system and are unique to each
           instance type. Can be accessed from inside the instance or by calling
           'getvariableofinstance'.
"#"      - A permanent local account variable.
           They are stored by char-server in the `acc_reg_num` table and
           `acc_reg_str`.
"##"     - A permanent global account variable stored by the login server.
           They are stored in the `global_acc_reg_num` table and
		   `global_acc_reg_str`.
           The only difference you will note from normal # variables is when
           you have multiple char-servers connected to the same login server.
           The # variables are unique to each char-server, while the ## variables
           are shared by all these char-servers.

Postfix: integer or string
nothing - integer variable, can store positive and negative numbers, but only
          whole numbers (so don't expect to do any fractional math)
'$'     - string variable, can store text

Examples:
  name  - permanent character integer variable
  name$ - permanent character string variable
 @name  - temporary character integer variable
 @name$ - temporary character string variable
 $name  - permanent global integer variable
 $name$ - permanent global string variable
$@name  - temporary global integer variable
$@name$ - temporary global string variable
 .name  - NPC integer variable
 .name$ - NPC string variable
.@name  - scope integer variable
.@name$ - scope string variable
 'name  - instance integer variable
 'name$ - instance string variable
 #name  - permanent local account integer variable
 #name$ - permanent local account string variable
##name  - permanent global account integer variable
##name$ - permanent global account string variable
```

If a variable was never set, it is considered to equal zero for integer
variables or an empty string ("", nothing between the quotes) for string
variables. Once you set it to that, the variable is as good as forgotten
forever, and no trace remains of it even if it was stored with character or
account data.

## Constants
Some variables are special, that is, they are already defined for you by the
scripting engine. You can see the full list in 'src/map/script_constants.hpp', which
is a file you should read, since it also allows you to replace lots of numbered
arguments for many commands with easier to read text. The special variables most
commonly used are all permanent character-based variables:

```
Zeny        - Amount of Zeny.
Hp          - Current amount of hit points.
MaxHp       - Maximum amount of hit points.
Sp          - Current spell points.
MaxSp       - Maximum amount of spell points.
StatusPoint - Amount of status points remaining.
SkillPoint  - Amount of skill points remaining.
BaseLevel   - Character's base level.
JobLevel    - Character's job level.
BaseExp     - Amount of base experience points.
JobExp      - Amount of job experience points.
NextBaseExp - Amount of base experience points needed to reach the next level.
NextJobExp  - Amount of job experience points needed to reach the next level.
Weight      - Amount of weight the character currently carries.
MaxWeight   - Maximum weight the character can carry.
Sex         - 0 if female, 1 if male.
Class       - Character's job.
Upper       - 0 if the character is a normal class, 1 if advanced, 2 if baby.
BaseClass   - The character's 1-1 'normal' job, regardless of Upper value.
              For example, this will return Job_Acolyte for Acolyte, Priest/Monk,
              High Priest/Champion, and Arch Bishop/Sura. If the character has not
              reached a 1-1 class, it will return Job_Novice.
BaseJob     - The character's 'normal' job, regardless of Upper value.
              For example, this will return Job_Acolyte for Acolyte,
              Baby Acolyte, and High Acolyte.
Karma       - The character's karma. Karma system is not fully functional, but
              this doesn't mean this doesn't work at all. Not tested.
Manner      - The character's manner rating. Becomes negative if the player
              utters words forbidden through the use of 'manner.txt' client-side
              file.
```

While these behave as variables, do not always expect to just set them - it is
not certain whether this will work for all of them. Whenever there is a command
or a function to set something, it's usually preferable to use that instead. The
notable exception is Zeny, which you can and often will address directly -
setting it will make the character own this number of Zeny.
If you try to set Zeny to a negative number, the script will be terminated with an error.

Some source-end constants can also be accessed in scripts. This list is located in
'src/map/script_constants.hpp', which contains constants such as server defines and status options:
```
	PACKETVER, MAX_LEVEL, MAX_STORAGE, MAX_INVENTORY, MAX_CART, MAX_ZENY, MAX_PARTY,
	MAX_GUILD, MAX_GUILDLEVEL, MAX_GUILD_STORAGE, MAX_BG_MEMBERS, MAX_CHAT_USERS,
	VIP_SCRIPT, MIN_STORAGE

	Option_Nothing, Option_Sight, Option_Hide, Option_Cloak, Option_Falcon, Option_Riding,
	Option_Invisible, Option_Orcish, Option_Wedding, Option_Chasewalk, Option_Flying,
	Option_Xmas, Option_Transform, Option_Summer, Option_Dragon1, Option_Wug,
	Option_Wugrider, Option_Madogear, Option_Dragon2, Option_Dragon3, Option_Dragon4,
	Option_Dragon5, Option_Hanbok, Option_Oktoberfest, Option_Dragon, Option_Costume
```

## Assigning variables

Variables can be accessed and modified much like in other programming languages.
```
	.@x = 100;
	.@x = .@y = 100;
```
Support for modifying variable values using 'set' is still supported (and required
to exist for this new method to work) so previous scripts will continue to work.

When assigning values, all operator methods are supported which exist in the below
'Operators' section. For instance:
```
	.@x += 100;
	.@x -= 100;
	.@x *= 2;
	.@x /= 2;
	.@x %= 5;
	.@x >>= 2;
	.@x <<= 2;
```
Will all work. For more information on available operators, see the Operators section
described below. All operators listed there may be placed in-front of the '=' sign
when modifying variables to perform the action as required.

Note:

 !! Currently the scripting engine does not support directly copying array variables.
 !! In order to copy arrays between variables the use of 'copyarray' function is still
 !! required.

## Strings

To include symbol '"' in a string you should use prefix '\"'


## Arrays

Arrays (in rAthena at least) are essentially a set of variables going under the
same name. You can tell between the specific variables of an array with an
'array index', a number of a variable in that array:
```
<variable name>[<array index>]
```
All variable types can be used as arrays.

Variables stored in this way, inside an array, are also called 'array elements'.
Arrays are specifically useful for storing a set of similar data (like several
item IDs for example) and then looping through it. You can address any array
variable as if it was a normal variable:
```
	set .@arrayofnumbers[0],1;
```
You can also do things like using a variable (or an expression, or even a
value from another array) to get at an array value:
```
	set .@x,100;
	set .@arrayofnumbers[.@x],10;
```
This will make .@arrayofnumbers[100] equal to 10.

Index numbering always starts with 0 and arrays can hold over 2 billion
variables. As such, the (guaranteed) allowed values for indices are in the
range 0 ~ 2147483647.

And array indexes probably can't be negative. Nobody tested what happens when
you try to get a negatively numbered variable from an array, but it's not going
to be pretty.

Arrays can naturally store strings:

.@menulines$[0] is the 0th element of the .@menulines$ array of strings. Notice
the '$', normally denoting a string variable, before the square brackets that
denotes an array index.

## Variable References

//##TODO



# Operators

Operators are things you can do to variables and numbers. They are either the
common mathematical operations or conditional operators
```
+ - will add two numbers. If you try to add two strings, the result will be a
    string glued together at the +. You can add a number to a string, and the
    result will be a string. No other math operators work with strings.
- - will subtract two numbers.
* - will multiply two numbers.
/ - will divide two numbers. Note that this is an integer division, i.e.
    7/2 is not equal 3.5, it's equal 3.
% - will give you the remainder of the division. 7%2 is equal to 1.
```
There are also conditional operators. This has to do with the conditional
command 'if' and they are meant to return either 1 if the condition is satisfied
and 0 if it isn't. (That's what they call 'boolean' variables. 0 means 'False'.
Anything except the zero is 'True' Odd as it is, -1 and -5 and anything below
zero will also be True.)

You can compare numbers to each other and you compare strings to each other, but
you can not compare numbers to strings.
```
 == - Is true if both sides are equal. For strings, it means they are the same.
 >=  - True if the first value is equal to, or greater than, the second value.
 <=  - True if the first value is equal to, or less than, the second value
 >   - True if the first value greater than the second value
 <   - True if the first value is less than the second value
 !=  - True if the first value IS NOT equal to the second one
```
Examples:
```
 1 == 1 is True.
 1<2 is True while 1>2 is False.
 .@x>2 is True if .@x is equal to 3. But it isn't true if .@x is 2.
```
Only ' == ' and '!=' have been tested for comparing strings. Since there's no way
to code a seriously complex data structure in this language, trying to sort
strings by alphabet would be pointless anyway.

Comparisons can be stacked in the same condition:

 && - Is True if and only if BOTH sides are true.
      ('1 == 1 && 2 == 2' is true. '2 == 1 && 1 == 1' is false.)
 || - Is True if either side of this expression is True.

 1 == 1 && 2 == 2 is True.
 1 == 1 && 2 == 1 is False.
 1 == 1 || 2 == 1 is True.

Logical bitwise operators work only on numbers, and they are the following:

 << - Left shift.
 >> - Right shift.
	Left shift moves the binary 1(s) of a number n positions to the left,
	which is the same as multiplying by 2, n times.
	In the other hand, Right shift moves the binary 1(s) of a number n positions
	to the right, which is the same as dividing by 2, n times.
		Example:
		set b,2;
		set a, b << 3;
		mes a;
		set a, a >> 2;
		mes a;
	The first mes command would display 16, which is the same as 2 x (2 x 2 x 2) = 16.
	The second mes command would display 4, which is the same as 16 / 2 = 8. 8 / 2 = 4.
 &  - And.
 |  - Or.
	The bitwise operator AND (&) is used to test two values against each other,
	and results in setting bits which are active in both arguments. This can
	be used for a few things, but in rAthena this operator is usually used to
	create bit-masks in scripts.

	The bitwise operator OR (|)sets to 1 a binary position if the binary position
	of one of the numbers is 1. This way a variable can hold several values we can check,
	known as bit-mask. A variable currently can hold up to 32 bit-masks (from position 0
	to position 1). This is a cheap(skate) and easy way to avoid using arrays to store several checks
	that a player can have.

	A bit-mask basically is (ab)using the variables bits to set various options in
	one variable. With the current limit if variables it is possible to store 32
	different options in one variable (by using the bits on position 0 to 31).

	Example(s):
	- Basic example of the & operator, bit example:
		10 & 2 = 2
	Why? :
		10 = 2^1 + 2^3 (2 + 8), so in bits, it would be 1010
		2 = 2^1 (2), so in bits (same size) it would be 0010
		The & (AND) operator sets bits which are active (1) in both arguments, so in the
		example 1010 & 0010, only the 2^1 bit is active (1) in both. Resulting in the bit
		0010, which is 2. 
	- Basic example of creating and using a bit-mask:
		set .@options,2|4|16; //(note: this is the same as 2+4+16, or 22)
		if (.@options & 1)	mes "Option 1 is activated";
		if (.@options & 2)	mes "Option 2 is activated";
		if (.@options & 4)	mes "Option 3 is activated";
		if (.@options & 8)	mes "Option 4 is activated";
		if (.@options & 16)	mes "Options 5 is activated";
	This would return the messages about option 2, 3 and 5 being shown (since we've set
	the 2,4 and 16 bit to 1).
 ^  - Xor.
	The bitwise operator XOR (eXclusive OR) sets a binary position to 0 if both
	numbers have the same value in the said position. On the other hand, it
	sets to 1 if they have different values in the said binary position.
	This is another way of setting and unsetting bits in bit-masks.

	Example:
	- First let's set the quests that are currently in progress:
		set inProgress,1|8|16; // quest 1,8 and 16 are in progress
	- After playing for a bit, the player starts another quest:
		if (inProgress&2 == 0) {
			// this will set the bit for quest 2 (inProgress has that bit set to 0)
			set inProgress,inProgress^2;
			mes "Quest 2: find a newbie and be helpful to him for an hour.";
			close;
		}
	- After spending some time reading info on Xor's, the player finally completes quest 1:
		if (inProgress&1 && isComplete) {
			// this will unset the bit for quest 1 (inProgress has that bit set to 1)
			set inProgress,inProgress^1;
			mes "Quest 1 complete!! You unlocked the secrets of the Xor dynasty, use them wisely.";
			close;
		}

Unary operators with only with a single number, which follows the operator, and
are following:

 -  - Negation.
	The sign of the number will be reversed. If the number was positive, it will
	become negative and vice versa.

	Example:
		set .@myvar,10;
		mes "Negative 10 is " + (-.@myvar);

 !  - Logical Not.
	Reverses the boolean result of an expression. True will become false and
	false will become true.

	Example:
		if (!callfunc("F_dosomething"))
		{
			mes "Doing something failed.";
			close;
		}

 ~  - Bitwise Not.
	Reverses each bit in a number, also known as one's complement. Cleared bits
	are set, and set bits are cleared.

	Example:
	- Ensure, that quest 2 is disabled, while keeping all other active, if they are.
		set inProgress,inProgress&(~2);  // same as set inProgress,inProgress&0xfffffffd

Ternary operators take three expressions (numbers, strings or boolean), and are
following:

 ?: - Conditional operator
	Very useful e.g. to replace

		if (Sex) mes "..."; else mes "...";

	clauses with simple

		mes "Welcome, " + (Sex?"Mr.":"Mrs.") + " " + strcharinfo(0);

	or to replace any other simple if-else clauses. It might be worth
	mentioning that ?: has low priority and has to be enclosed with
	parenthesis in most (if not all) cases.

Labels
------

Within executable script code, some lines can be labels:

<label name>:

Labels are points of reference in your script, which can be used to route
execution with 'goto', 'menu' and 'jump_zero' commands, invoked with 'doevent'
and 'donpcevent' commands and are otherwise essential. A label's name may not be
longer than 22 characters. (23rd is the ':'.) There is some confusion in the
source about whether it's 22, 23 or 24 all over the place, so keeping labels
under 22 characters could be wise. It may only contain alphanumeric characters
and underscore. In addition to labels you name yourself, there are also some
special labels which the script engine will start execution from if a special
event happens:

OnClock<hour><minute>:
OnMinute<minute>:
OnHour<hour>:
On<weekday><hour><minute>:
OnDay<month><day>:

This will execute when the server clock hits the specified date or time. Hours
and minutes are given in military time. ('0105' will mean 01:05 AM). Weekdays
are Sun,Mon,Tue,Wed,Thu,Fri,Sat. Months are 01 to 12, days are 01 to 31.
Remember the zero.

OnInit:
OnInterIfInit:
OnInterIfInitOnce:

OnInit will execute every time the scripts loading is complete, including when
they are reloaded with @reloadscript command. OnInterIfInit will execute when
the map server connects to a char server, OnInterIfInitOnce will only execute
once and will not execute if the map server reconnects to the char server later.

OnAgitStart:
OnAgitEnd:
OnAgitInit:
OnAgitStart2:
OnAgitEnd2:
OnAgitInit2:
OnAgitStart3:
OnAgitEnd3:
OnAgitInit3:

OnAgitStart will run whenever the server shifts into WoE mode, whether it is
done with @agitstart GM command or with 'AgitStart' script command. OnAgitEnd
will do likewise for the end of WoE.

OnAgitInit will run when data for all castles and all guilds that hold a castle
is received by map-server from the char-server after initial connect.

No RID will be attached while any of the above mentioned labels are triggered, so
no character or account-based variables will be accessible, until you attach a
RID with 'attachrid' (see below).

The above also applies to, the last three labels, the only difference is that
these labels are used exclusively for WoE SE, and are called independently.

OnInstanceInit:

This label will be executed when an instance is created and initialized through
the 'instance_create' command. It will run again if @reloadscript is used while
an instance is in progress.

OnInstanceDestroy:

This label will be executed when an instance is destroyed by a timeout, exceeding
the keepalive time or through the 'instance_destroy' command. It will be called
exactly before the instance will be destroyed and all other NPCs of the instance
will still be available at this point of time.

OnTouch:

This label will be executed if a trigger area is defined for the NPC object it's
in. If it isn't present, the execution will start from the beginning of the NPC
code. The RID of the triggering character object will be attached.

OnTouch_:

Similar to OnTouch, but will only run one instance. Another character is
chosen once the triggering character leaves the area.

OnTouchNPC:

Similar to OnTouch, but will only trigger for monsters. For this case, by using
'getattachedrid' will returns GID (ID that returned when use 'monster').

OnPCLoginEvent:
OnPCLogoutEvent:
OnPCBaseLvUpEvent:
OnPCJobLvUpEvent:

It's pretty obvious when these four special labels will be invoked.

OnPCDieEvent:

This special label triggers when a player dies. The variable 'killerrid' is
set to the ID of the killer.

OnPCKillEvent:

This special label triggers when a player kills another player. The variable
'killedrid' is set to the ID of the player killed.

OnNPCKillEvent:

This special label triggers when a player kills a monster without label.
The variable 'killedrid' is set to the Class (mob ID) of the monster killed.
The variable 'killedgid' is set to the ID (unique mob game ID) of the monster killed.

OnPCLoadMapEvent:

This special label triggers when a player steps in a map marked with the
'loadevent' mapflag and attaches its RID. The fact that this label requires a
mapflag for it to work is because, otherwise, it'd be server-wide and trigger
every time a player would change maps. Imagine the server load with 1,000 players
(oh the pain...)

OnPCStatCalcEvent:

This special label triggers when a player's stats are recalculated, such as when
changing stats, equipment, or maps, as well as when logging in, leveling up, and
mounting a job mount. This can be used to grant additional item bonuses to certain
player groups, for instance.

OnWhisperGlobal:

This special label triggers when a player whispers the NPC, and will run with the
player's RID attached. It can accept up to ten parameters, which will be stored
into separate temporary character string variables @whispervar0$ to @whispervar9$.
See 'doc/whisper_sys.txt' for further documentation.

Only the special labels which are not associated with any script command are
listed here. There are other kinds of labels which may be triggered in a similar
manner, but they are described with their associated commands.

On<label name>:

These special labels are used with Mob scripts mostly, and script commands
that requires you to point/link a command to a mob or another NPC, giving a label
name to start from. The label name can be any of your liking, but must be
started with "On".

Example:

monster "prontera",123,42,"Poringz0rd",2341,23,"Master::OnThisMobDeath";

amatsu,13,152,4	script	Master	767,{
	mes "Hi there";
	close;

OnThisMobDeath:
	announce "Hey, " + strcharinfo(0) + " just killed a Poringz0rd!",bc_blue|bc_all;
	end;
}

Each time you kill one, that announce will appear in blue to everyone.

"Global" labels

There's a catch with labels and doevent. If you call a label (using doevent)
and called label is in NPC that has trigger area, that label must end with
"Global" to work globally (i.e. if RID is outside of the trigger area, which
usually happens since otherwise there would be no point calling the label with
doevent, because OnTouch would do the job). For further reference look for
npc_event in npc.cpp.

Scripting commands and functions
--------------------------------

The commands and functions are listed here in no particular order. There's a
difference between commands and functions - commands leave no 'return value'
which might be used in a conditional statement, as a command argument, or stored
in a variable. Calling commands as if they were functions will sometimes work,
but is not advised, as this can lead to some hard to track errors. Calling
functions as if they were commands will mess up the stack, so 'return' command
will not return correctly after this happens in a particular script.

All commands must end with a ';'.

-------------------------


From here on, we will have the commands sorted as follow:

1.- Basic commands.
2.- Information-retrieving commands.
3.- Checking commands.
4.- Player-related commands.
5.- Mob / NPC -related commands.
6.- Other commands.
7.- Instance commands.
8.- Quest Log commands.
9.- Battleground commands.
10.- Pet commands.
10.1.- The Pet AI commands.
11.- Homunculus commands.
12.- Mercenary commands.
13.- Party commands.
14.- Channel commands.
15.- Achievement commands.


# 1. Basic commands

## mes
*mes "<string>"{,"<string>"{,...}};

This command will display a box on the screen for the invoking character, if no
such box is displayed already, and will print the string specified into that
box. There is normally no 'close' or 'next' button on this box, unless you
create one with 'close' or 'next', and while it's open the player can't do much
else, so it's important to create a button later. If the string is empty, it
will show up as an empty line.

	mes "Text that will appear in the box";

### Colors

Inside the string you may put color codes, which will alter the color of the
text printed after them. The color codes are all `'^<R><G><B>'` and contain three
hexadecimal numbers representing colors as if they were HTML colors - `^FF0000` is
bright red, `^00FF00` is bright green, `^0000FF` is bright blue, `^000000` is black.
^FF00FF is a pure magenta, but it's also a color that is considered transparent
whenever the client is drawing windows on screen, so printing text in that color
will have kind of a weird effect. Once you've set a text's color to something,
you have to set it back to black unless you want all the rest of the text be in
that color:
```
	mes "This is ^FF0000 red ^000000 and this is ^00FF00 green, ^000000 so.";
```
Notice that the text coloring is handled purely by the client. If you use non-
English characters, the color codes might get screwed if they stick to letters
with no intervening space. Separating them with spaces from the letters on
either side solves the problem.

### Multiple Lines

To display multiple lines of message while only using a single 'mes' command,
use the script command in the following format:
```
	mes "Line 1", "Line 2", "Line 3";
```
This will display 3 different lines while only consuming a single line in
the relevant script file.

### Navigation

For clients dated 2011-10-10aRagexe onwards, you can generate navigation links
using HTML-like labels:
```
	<NAVI>Display Name<INFO>mapname,x,y,0,000,flag</INFO></NAVI>
```

The "flag" parameter can be:
* 0: Do not open Navigation Window (default).
* 1: Open Navigation Window.

The example below will make the [Tool Shop] text clickable and begin navigation
to alberta (98,154) when clicked.
```
	mes "Have you checked out the <NAVI>[Tool Shop]<INFO>alberta,98,154,0,000,0</INFO></NAVI>?";
```
See also [navigateto](##navigateto), which can be used for certain NPC events.

### Items

You can refer to items by using HTML-like links to certain items:

	<ITEMLINK>Display Name<INFO>Item ID</INFO></ITEMLINK>

Where <Display Name> is the name that will be displayed for your link and
<Item ID> being the ID of the item you want to link to when clicked.

In 2015 the tag name was changed to <ITEM> resulting in the following syntax:

	<ITEM>Display Name<INFO>Item ID</INFO></ITEM>

The following sample will open a preview window for Red Potion:

	mes "Did you ever consume a <ITEMLINK>Red Potion<INFO>501</INFO></ITEMLINK>?";
	// Or in 2015:
	mes "Did you ever consume a <ITEM>Red Potion<INFO>501</INFO></ITEM>?";

NOTE: Be aware that item links are rendered incorrectly in 2015+ clients at the moment.

### URLs

Similarly, you can create links to websites that launch in a new window:

	<URL>Display Name<INFO>http://www.example.com/</INFO></URL>";

---------------------------------------

## next
*next;

This command will display a 'next' button in the message window for the
invoking character. Clicking on it will cause the window to clear and display
a new one. Used to segment NPC-talking, next is often used in combination with
'mes' and 'close'.

If no window is currently on screen, one will be created, but once the invoking
character clicks on it, a warning is thrown on the server console and the script
will terminate.

	mes "[Woman]";
	mes "This would appear on the page";
	next;
	// This is needed since it is a new page and the top will now be blank
	mes "[Woman]";
	mes "This would appear on the 2nd page";

---------------------------------------

## clear
*clear;

This command will clear the dialog text and continue the script without player interaction.

Example:
	mes "This is how the 'clear' script command works.";
	sleep2 3000;
	clear; // This will clear the dialog and continue to the next one.
	mes "I will show you again.";
	sleep2 3000;
	clear;
	mes "Bye!";
	close;

---------------------------------------

## close
*close;

This command will create a 'close' button in the message window for the invoking
character. If no window is currently on screen, the script execution will end. This is one
of the ways to end a speech from an NPC. Once the button is clicked, the NPC
script execution will end, and the message box will disappear.

	mes "[Woman]";
	mes "I am finished talking to you. Click the close button.";
	close;
	mes "This command will not run at all, since the script has ended.";

---------------------------------------

## close2
*close2;

This command will create a 'close' button in the message window for the invoking
character. WARNING: If no window is currently on screen, the script execution will halt
indefinitely! See 'close'. There is one important difference, though - even though
the message box will have closed, the script execution will not stop, and commands after
'close2' will still run, meaning an 'end' has to be used to stop the script, unless you
make it stop in some other manner.

	mes "[Woman]";
	mes "I will warp you now.";
	close2;
	warp "place",50,50;
	end;

Don't expect things to run smoothly if you don't make your scripts 'end'.

---------------------------------------

## end
*end;

This command will stop the execution for this particular script. The two
versions are perfectly equivalent. It is the normal way to end a script which
does not use 'mes'.

	if (BaseLevel <= 10)
		npctalk "Look at that you are still a n00b";
	else if (BaseLevel <= 20)
		npctalk "Look at that you are getting better, but still a n00b";
	else if (BaseLevel <= 30)
		npctalk "Look at that you are getting there, you are almost 2nd profession now right???";
	else if (BaseLevel <= 40)
		npctalk "Look at that you are almost 2nd profession";
	end;

Without the use of 'end' it would travel through the labels until the end of the
script. If you were lvl 10 or less, you would see all the speech lines, the use
of 'end' stops this, and ends the script.

---------------------------------------

## set
*set <variable>,<expression>{,<char_id>};
*set(<variable>,<expression>{,<char id>})

This command will set a variable to the value that the expression results in.
Variables may either be set through this command or directly, much like any
other programming language (refer to the "Assigning variables" section).

This is the most basic script command and is used a lot whenever you try to do
anything more advanced than just printing text into a message box.

	set .@x,100;

will make .@x equal 100.

	set .@x,1+5/8+9;

will compute 1+5/8+9 (which is, surprisingly, 10 - remember, all numbers are
integer in this language) and make .@x equal it.

Returns the variable reference (since trunk r12870).

---------------------------------------

## setd
*setd "<variable name>",<value>{,<char_id>};

Works almost identically as set, except the variable name is identified as a string
and can thus be constructed dynamically.

This command is equivalent to:
	set getd("variable name"),<value>;

Examples:

	setd ".@var$", "Poporing";
	mes .@var$; // Displays "Poporing".

	setd ".@" + .@var$ + "123$", "Poporing is cool";
	mes .@Poporing123$; // Displays "Poporing is cool".

NOTE:
	'char_id' only works for non-server variables.
	Player with Character ID 'char_id' must be online.

---------------------------------------

## getd
*getd("<variable name>")

Returns a reference to a variable, the name can be constructed dynamically.
Refer to 'setd' for usage.

This can also be used to set an array dynamically:
	setarray getd(".array[0]"), 1, 2, 3, 4, 5;

Examples:

	set getd("$varRefence"), 1;
	set .@i, getd("$" + "pikachu");

---------------------------------------

## getvariableofnpc
*getvariableofnpc(<variable>,"<npc name>")

Returns a reference to a NPC variable (. prefix) from the target NPC.
This can only be used to get . variables.

Examples:

//This will return the value of .var, note that this can't be used, since the value isn't caught.
	getvariableofnpc(.var,"TargetNPC");

//This will set the .v variable to the value of the TargetNPC's .var variable.
	set .v, getvariableofnpc(.var,"TargetNPC");

//This will set the .var variable of TargetNPC to 1.
	set getvariableofnpc(.var,"TargetNPC"), 1;

Note: even though function objects can have .variables,
getvariableofnpc will not work on them.

---------------------------------------

## getvar
*getvar <variable>,<char_id>;

Get variable value from the specified player. Only player/account variables
are allowed to be used (temporary character variable "@", permanent
character "", permanent local account "#", and permanent global account "##").

---------------------------------------

## goto
*goto <label>;

This command will make the script jump to a label, usually used in conjunction
with other command, such as "if", but often used on its own.

	...
	goto Label;

	mes "This will not be seen";
	end;
Label:
	mes "This will be seen";
	end;

This command should be avoided and only used if there is no other option.

---------------------------------------

## menu
*menu "<option_text>",<target_label>{,"<option_text>",<target_label>,...};

This command will create a selectable menu for the invoking character. Only one
menu can be on screen at the same time.

Depending on what the player picks from the menu, the script execution will
continue from the corresponding label. (it's string-label pairs, not label-
string)

Options can be grouped together, separated by the character ':'.

	menu "A:B",L_Wrong,"C",L_Right;

It also sets a special temporary character variable @menu, which contains the
number of option the player picked. (Numbering of options starts at 1.)
This number is consistent with empty options and grouped options.

	menu "A::B",L_Wrong,"",L_Impossible,"C",L_Right;

	L_Wrong:
		// If they click "A" or "B" they will end up here
		// @menu == 1 if "A"
		// @menu == 2 will never happen because the option is empty
		// @menu == 3 if "B"
	L_Impossible:
		// Empty options are not displayed and therefore can't be selected
		// this label will never be reached from the menu command
	L_Right:
		// If they click "C" they will end up here
		// @menu == 5

If a label is '-', the script execution will continue right after the menu
command if that option is selected, this can be used to save you time, and
optimize big scripts.

	menu "A::B:",-,"C",L_Right;
		// If they click "A" or "B" they will end up here
		// @menu == 1 if "A"
		// @menu == 3 if "B"
	L_Right:
		// If they click "C" they will end up here
		// @menu == 5

Both these examples will perform the exact same task.

If you give an empty string as a menu item, the item will not display. This
can effectively be used to script dynamic menus by using empty string for
entries that should be unavailable at that time.

You can do it by using arrays, but watch carefully - this trick isn't high
wizardry, but minor magic at least. You can't expect to easily duplicate it
until you understand how it works.

Create a temporary array of strings to contain your menu items, and populate it
with the strings that should go into the menu at this execution, making sure not
to leave any gaps. Normally, you do it with a loop and an extra counter, like
this:
```
	setarray .@possiblemenuitems$[0],<list of potential menu items>;
	.@j = 0; // That's the menu lines counter.

	// We loop through the list of possible menu items.
	// .@i is our loop counter.
	for( .@i = 0; .@i < getarraysize(.@possiblemenuitems$); .@i++ )
	{
		// That 'condition' is whatever condition that determines whether
		// a menu item number .@i actually goes into the menu or not.

		if (<condition>)
		{
			// We record the option into the list of options actually available.

			.@menulist$[@j] = .@possiblemenuitems$[@i];

			// We just copied the string, we do need its number for later
			// though, so we record it as well.

			.@menureference[@j] = .@i;

			// Since we've just added a menu item into the list, we increment
			// the menu lines counter.

			.@j++;
		}

		// We go on to the next possible menu item.
	}
```

This will create you an array .@menulist$ which contains the text of all items
that should actually go into the menu based on your condition, and an array
.@menureference, which contains their numbers in the list of possible menu items.
(Remember, arrays start with 0.) There's less of them than the possible menu
items you've defined, but the menu command can handle the empty lines - only if
they are last in the list, and if it's made this way, they are. Now comes a
dirty trick:

	// X is whatever the most menu items you expect to handle.
	menu .@menulist$[0],-,.@menulist$[1],-,...,.@menulist$[<X>],-;

This calls up a menu of all your items. Since you didn't copy some of the
possible menu items into the list, its end is empty and so no menu items will
show up past the end. But this menu call doesn't jump anywhere, it just
continues execution right after the menu command. (And it's a good thing it
doesn't, cause you can only explicitly define labels to jump to, and how do you
know which ones to define if you don't know beforehand which options will end up
where in your menu?)
But how do you figure out which option the user picked? Enter the @menu.

@menu contains the number of option that the user selected from the list,
starting with 1 for the first option. You know now which option the user picked
and which number in your real list of possible menu items it translated to:

    mes "You selected " + .@possiblemenuitems$[.@menureference[@menu-1]] + "!";

@menu is the number of option the user picked.
@menu-1 is the array index for the list of actually used menu items that we
made.
.@menureference[@menu-1] is the number of the item in the array of possible menu
items that we've saved just for this purpose.

And .@possiblemenuitems$[.@menureference[@menu-1]] is the string that we used to
display the menu line the user picked. (Yes, it's a handful, but it works.)

You can set up a bunch of 'if (.@menureference[@menu-1] == X) goto Y' statements to
route your execution based on the line selected and still generate a different
menu every time, which is handy when you want to, for example, make users select
items in any specific order before proceeding, or make a randomly shuffled menu.

Kafra code bundled with the standard distribution uses a similar array-based
menu technique for teleport lists, but it's much simpler and doesn't use @menu,
probably since that wasn't documented anywhere.

See also 'select', which is probably better in this particular case. Instead of
menu, you could use 'select' like this:

    .@dummy = select(.@menulist$[0],.@menulist$[1],...,.@menulist$[<X>]);

For the purposes of the technique described above these two statements are
perfectly equivalent.

---------------------------------------

## select
*select("<option>"{,"<option>",...})
## prompt
*prompt("<option>"{,"<option>",...})

This function is a handy replacement for 'menu' for some specific cases where
you don't want a complex label structure - like, for example, asking simple yes-
no questions. It will return the number of menu option picked, starting with 1.
Like 'menu', it will also set the variable @menu to contain the option the user
picked.

    if (select("Yes:No" ) == 1)
		mes "You said yes, I know.";

And like 'menu', the selected option is consistent with grouped options
and empty options.

'prompt' works almost the same as select, except that when a character clicks
the Cancel button, this function will return 255 instead.

---------------------------------------

## input
*input(<variable>{,<min>{,<max>}})

This command will make an input box pop up on the client connected to the
invoking character, to allow entering of a number or a string. This has many
uses, one example would be a guessing game, also making use of the 'rand'
function:

	mes "[Woman]";
	mes "Try and guess the number I am thinking of.";
	mes "The number will be between 1 and 10.";
	next;
	.@number = rand(1,10);
	input .@guess;
	if (.@guess == .@number) {
		mes "[Woman]";
		mes "Well done, that was the number I was thinking of!";
		close;
	} else {
		mes "[Woman]";
		mes "Sorry, that wasn't the number I was thinking of.";
		close;
	}

If you give the input command a string variable to put the input in, it will
allow the player to enter text. Otherwise, only numbers will be allowed.

	mes "[Woman]";
	mes "Please say HELLO";
	next;
	input .@var$;
	if (.@var$ == "HELLO") {
		mes "[Woman]";
		mes "Well done, you typed it correctly.";
		close;
	} else {
		mes "[Woman]";
		mes "Sorry, you got it wrong.";
		close;
	}

Normally you may not input a negative number with this command.
This is done to prevent exploits in badly written scripts, which would
let people, for example, put negative amounts of Zeny into a bank script and
receive free Zeny as a result.

Since trunk r12192 the command has two optional arguments and a return value.
The default value of 'min' and 'max' can be set with 'input_min_value' and
'input_max_value' in script_athena.conf.
For numeric inputs the value is capped to the range [min,max]. Returns 1 if
the value was higher than 'max', -1 if lower than 'min' and 0 otherwise.
For string inputs it returns 1 if the string was longer than 'max', -1 is
shorter than 'min' and 0 otherwise.

---------------------------------------

## callfunc
*callfunc "<function>"{,<argument>,...<argument>};
*callfunc("<function>"{,<argument>,...<argument>})

This command lets you call up a function NPC. A function NPC can be called from
any script on any map server. Using the 'return' command it will come back to
the place that called it.

```
	place,50,50,6%TAB%script%TAB%Woman%TAB%115,{
		mes "[Woman]"
		mes "Let's see if you win...";
		callfunc "funcNPC";
		mes "Well done, you have won!";
		close;
	}
	function%TAB%script%TAB%funcNPC%TAB%{
		.@win = rand(2);
		if (.@win == 0)
			return;
		mes "Sorry, you lost.";
		close;
	}
```

You can pass arguments to your function - values telling it what exactly to do -
which will be available there with getarg() (see 'getarg')
Notice that returning is not mandatory, you can end execution right there.

If you want to return a real value from inside your function NPC, it is better
to write it in the function form, which will also work and will make the script
generally cleaner:

```
	place,50,50,6%TAB%script%TAB%Man%TAB%115,{
		mes "[Man]"
		mes "Gimme a number!";
		next;
		input .@number;
		if (callfunc("OddFunc",.@number)) mes "It's Odd!";
		close;
	}
	function%TAB%script%TAB%OddFunc%TAB%{
		if (getarg(0)%2 == 0)
			return 0;// it's even
		return 1;// it's odd
	}
```

Alternately, as of rAthena revision 15979 and 15981, user-defined functions
may be called directly without the use of the 'callfunc' script command.

```
	function<tab>script<tab>SayHello<tab>{
		mes "Hello " + getarg(0);
		return 0;
	}

	place,50,50,6<tab>script<tab>Man<tab>115,{
		mes "[Man]";
		SayHello strcharinfo(0);
		close;
	}
```

Note:

 !! A user-defined function must be declared /before/ a script attempts to
 !! call it. That is to say, any functions should be placed above scripts or NPCs
 !! (or loaded in a separate file first) before attempting to call them directly.

---------------------------------------

## callsub
*callsub <label>{,<argument>,...<argument>};
*callsub(<label>{,<argument>,...<argument>})

This command will go to a specified label within the current script (do NOT use
quotes around it) coming in as if it were a 'callfunc' call, and pass it
arguments given, if any, which can be recovered there with 'getarg'. When done
there, you should use the 'return' command to go back to the point from where
this label was called. This is used when there is a specific thing the script
will do over and over, this lets you use the same bit of code as many times as
you like, to save space and time, without creating extra NPC objects which are
needed with 'callfunc'. A label is not callable in this manner from another
script.

Example 1: callsub for checking (if checks pass, return to script)
```
	callsub S_CheckFull, "guild_vs2",50;
	switch( rand(4) ) {
		case 0:	warp "guild_vs2",9,50;	end;
		case 1:	warp "guild_vs2",49,90;	end;
		case 2:	warp "guild_vs2",90,50;	end;
		case 3:	warp "guild_vs2",49,9;	end;
	}
...

S_CheckFull:
	if (getmapusers(getarg(0)) >= getarg(1)) {
		mes "I'm sorry, this arena is full.  Please try again later.";
		close;
	}
	return;
```

Example 2: callsub used repeatedly, with different arguments
```
// notice how the Zeny check/delete is reused, instead of copy-pasting for every warp
	switch(select("Abyss Lake:Amatsu Dungeon:Anthell:Ayothaya Dungeon:Beacon Island, Pharos") {
		case 1:	callsub S_DunWarp,"hu_fild05",192,207;
		case 2:	callsub S_DunWarp,"ama_in02",119,181;
		case 3:	callsub S_DunWarp,"moc_fild20",164,145;
		case 4:	callsub S_DunWarp,"ayo_fild02",279,150;
		case 5:	callsub S_DunWarp,"cmd_fild07",132,125;
		// etc
	}

...

S_DunWarp:
// getarg(0) = "map name"
// getarg(1) = x
// getarg(2) = y
	if (Zeny >= 100) {
		Zeny -= 100;
		warp getarg(0),getarg(1),getarg(2);
	} else {
		mes "Dungeon warp costs 100 Zeny.";
	}
	close;
```

---------------------------------------

## getarg
*getarg(<index>{,<default_value>})

This function is used when you use the 'callsub' or 'callfunc' commands. In the
call you can specify variables that will make that call different from another
one. This function will return an argument the function or subroutine was
called with, and is the normal way to get them.
This is another thing that can let you use the same code more than once.

Argument numbering starts with 0, i.e. the first argument you gave is number 0.
If no such argument was given, a zero is returned.

```
	place,50,50,6%TAB%script%TAB%Woman1%TAB%115,{
		mes "[Woman]";
		mes "Let's see if you win...";
		callfunc "funcNPC",2;
		mes "Well done, you have won!";
		close;
	}

	place,52,50,6%TAB%script%TAB%Woman2%TAB%115,{
		mes "[Woman]";
		mes "Let's see if you win...";
		callfunc "funcNPC",5;
		mes "Well done, you have won!";
		close;
	}

	function%TAB%script%TAB%funcNPC%TAB%{
		.@win = rand(getarg(0));
		if (.@win == 0) return;
		mes "Sorry, you lost.";
		close;
	}
```

"woman1" NPC object calls the funcNPC. The argument it gives in this call is
stated as 2, so when the random number is generated by the 'rand' function, it
can only be 0 or 1. Whereas "woman2" gives 5 as the argument number 0 when
calling the function, so the random number could be 0, 1, 2, 3 or 4, this makes
"woman2" less likely to say the player won.

You can pass multiple arguments in a function call:

	callfunc "funcNPC",5,4,3;

getarg(0) would be 5, getarg(1) would be 4 and getarg(2) would be 3.

'getarg' has an optional argument since trunk r10773 and stable r10958.
If the target argument exists, it is returned.
Otherwise, if <default_value> is present it is returned instead,
if not the script terminates immediately.

In the previous example getarg(2,-1) would be 3 and getarg(3,-1) would be -1.

---------------------------------------

## getargcount
*getargcount()

This function is used when you use the 'callsub' or 'callfunc' commands. In the
call you can specify arguments. This function will return the number of arguments
provided.

Example:
	callfunc "funcNPC",5,4,3;
	...
	function%TAB%script%TAB%funcNPC%TAB%{
		.@count = getargcount(); // 3
		...
	}

---------------------------------------

## return
*return {<value>};

This command causes the script execution to leave previously called function
with callfunc or script with callsub and return to the location, where the call
originated from. Optionally a return value can be supplied, when the call was
done using the function form.

Using this command outside of functions or scripts referenced by callsub will
result in error and termination of the script.

	callfunc "<your function>";// when nothing is returned
	set <variable>,callfunc("<your function>");// when a value is being returned

---------------------------------------

## function
```
*function <function name>;
*<function name>{(<argument>,...<argument>)};
*function <function name> {
    <code>
}
```

This works like callfunc, and is used for cleaner and faster scripting. The function
must be defined and used within a script, and works like a label with arguments.
Note that the name may only contain alphanumeric characters and underscore.

Usage:

    1. Declare the function.
	function <function name>;
    2. Call the function anywhere within the script.
       It can also return a value when used with parentheses.
	<function name>;
    3. Define the function within the script.
	<function name> {<code>}

Example:
```
prontera,154,189,4	script	Item Seller	767,{
	/* Function declaration */
	function SF_Selling;

	if (Zeny > 50) {
		mes "Welcome!";
		/* Function call */
		SF_Selling;
	}
	else mes "You need 50z, sorry!";
	close;

	/* Function definition */
	function SF_Selling {
		mes "Would you like to buy a phracon for 50z?";
		next;
		if (select("Yes","No, thanks") == 1) {
			Zeny -= Zeny;
			getitem 1010,1;
			mes "Thank you!";
		}
		return;
	}
}
```

Example with parameters and return value:
```
prontera,150,150,0	script	TestNPC	123,{
	/* Function declaration */
	function MyAdd;

	mes "Enter two numbers.";
	next;
	input .@a;
	input .@b;
	/* Function call */
	mes .@a + " + " + .@b + " = " + MyAdd(.@a,.@b);
	close;

	/* Function definition */
	function MyAdd {
		return getarg(0)+getarg(1);
	}
}
```

---------------------------------------

## is_function
*is_function("<function name>")

This command checks whether a function exists.
It returns 1 if function is found, or 0 if it isn't.

Example:
```
	function	script	try	{
		dothat;
	}

	-	script	test	-1,{
		.@try = is_function("try"); // 1
		.@not = is_function("not"); // 0
	}
```

## if
*if (<condition>) <statement>;

This is the basic conditional statement command, and just about the only one
available in this scripting language.

The condition can be any expression. All expressions resulting in a non-zero
value will be considered True, including negative values. All expressions
resulting in a zero are false.

If the expression results in True, the statement will be executed. If it isn't
true, nothing happens and we move on to the next line of the script.

    if (1)  mes "This will always print.";
    if (0)  mes "And this will never print.";
    if (5)  mes "This will also always print.";
    if (-1) mes "Funny as it is, this will also print just fine.";

For more information on conditional operators see the operators section above.
Anything that is returned by a function can be used in a condition check without
bothering to store it in a specific variable:

    if (strcharinfo(0) == "Daniel Jackson") mes "It is true, you are Daniel!";

More examples of using the 'if' command in the real world:

Example 1:
```
	.@answer = 1;
	input .@input;
	if (.@input == .@answer)
		close;
	mes "Sorry, your answer is incorrect.";
	close;
```
Example 2:
```
	.@answer = 1;
	input .@input;
	if (.@input != .@answer)
		mes "Sorry, your answer is incorrect.";
	close;
```
Notice that examples 1 and 2 have the same effect.

Example 3:
```
	.@count++;
	mes "[Forgetful Man]";
	if (.@count == 1) mes "This is the first time you have talked to me.";
	if (.@count == 2) mes "This is the second time you have talked to me.";
	if (.@count == 3) mes "This is the third time you have talked to me.";
	if (.@count == 4) {
		mes "This is the fourth time you have talked to me.";
		mes "I think I am getting amnesia, I have forgotten about you...";
		.@count = 0;
	}
	close;
```

Example 4:
```
	mes "[Quest Person]";
	if (countitem(512) < 1) {  // 512 is the item ID for Apple, found in item_db
		mes "Can you please bring me an apple?";
		close;
	}
	mes "Oh, you brought an Apple!";
	mes "I didn't want it, I just wanted to see one.";
	close;
```

Example 5:
```
	mes "[Person Checker]";
	if ($@name$ == "") {  // global variable not yet set
		mes "Please tell me someones name";
		next;
		input $@name$;
		$@name2$ = strcharinfo(0);
		mes "[Person Checker]";
		mes "Thank you.";
		close;
	}
	if ($@name$ == strcharinfo(0)) {  // player name matches $@name$
		mes "You are the person that " + $@name2$ + " just mentioned.";
		mes "Nice to meet you!";

		// reset the global variables
		$@name$ = "";
		$@name2$ = "";

		close;
	}
	mes "You are not the person that " + $name2$ + " mentioned.";
	close;
```
See 'strcharinfo' for an explanation of what this function does.

Example 6: Using complex conditions.
```
	mes "[Multiple Checks]";
	if (@queststarted == 1 && countitem(512) >= 5) {
		mes "Well done, you have started the quest and brought me 5 Apples.";
		@queststarted = 0;
		delitem 512,5;
		close;
	}
	mes "Please bring me 5 apples.";
	@queststarted = 1;
	close;
```
The script engine also supports nested 'if' statements:
```
	if (<condition>)
		dothis;
	else
		dothat;
```
If the condition isn't met, it'll do the action following the 'else'.
We can also group several actions depending on a condition:
```
	if (<condition>) {
		dothis1;
		dothis2;
	} else {
		dothat1;
		dothat2;
		dothat3;
	}
```
Remember that if you plan to do several actions upon the condition being false, and
you forget to use the curly braces (the { } ), the second action will be executed regardless
the output of the condition, unless of course, you stop the execution of the script if the
condition is true (that is, in the first grouping using a return; , and end; or a close; )

Also, you can have multiple conditions nested or chained.
```
	if (<condition 1>)
		dothis;
	else if (<condition 2>) {
		dothat;
		end;
	} else
		dothis;
```
---------------------------------------

## jump_zero
*jump_zero (<condition>),<label>;

This command works kinda like an 'if'+'goto' combination in one go. (See 'if').
If the condition is false (equal to zero) this command will immediately jump to
the specified label like in 'goto'. While 'if' is more generally useful, for
some cases this could be an optimization.

The main reason for this command is that other control statements, like
'switch', 'for' or 'while', are disassembled into simple expressions together
with this command when a script is parsed.

---------------------------------------

## switch
*switch (expression);

The switch statement is similar to a series of if statements on the same expression.
In many occasions, you may want to compare the same variable (or expression)
with many different values, and execute a different piece of code depending
on which value it equals to. This is exactly what the switch statement is for.

It is important to understand how the switch statement is executed in order
to avoid mistakes. The switch statement executes line by line (actually, statement by statement).
In the beginning, no code is executed. Only when a case statement is found
with a value that matches the value of the switch expression the case statement(s)
will to executed. The parser continues to execute the statements until the end
of the switch block, or the first time it sees a break statement. If you don't
write a break statement at the end of a case's statement list, the parser will
go on executing the statements of the following case (fall-through).

Example 1:
```
	switch(select("Yes:No")) {
		case 1:
			mes "You said yes!";
			break;
		case 2:
			mes "Aww, why?";
			break;
	}
	close;
```
The example above would work like a menu and would go to the first case if
the user selects option, otherwise, would go to the second one.

Example 2:
```
	switch(getgroupid()) {
		case 1:
			mes "Wow, you're super!";
			break;
		case 2:
			mes "A helping hand!";
			break;
		case 3:
			mes "10001010010011";
			break;
		case 4:
			mes "Yes, milord?";
			break;
		default:
			mes "Hello there!";
			break;
	}
```
The example above would print a message depending on the player's groupid.
If there is no statement declared for the corresponding groupid, the script
would use the 'default' statement that applies to rest of possible values,
similar to 'else' in the if-else statement.

---------------------------------------

## while
*while (<condition>) <statement>;

This is probably the simplest and most frequently used loop structure. The 'while'
statement can be interpreted as "while <condition> is true, perform <statement>".
It is a pretest loop, meaning the conditional expression is tested before any of the
statements in the body of the loop are performed. If the condition evaluates to
false, the statement(s) in the body of the loop is/are never executed. If the
condition evaluates to true, the statement(s) are executed, then control transfers
back to the conditional expression, which is reevaluated and the cycle continues.

Multiple statements can be grouped with { }, curly braces, just like with the 'if' statement.

Example 1:
```
	while (switch(select("Yes:No") == 2 ))
		mes "You picked no.";
	close;
```

Example 2: multiple statements
```
	while (switch(select("Yes:No") == 2 )) {
		mes "Why did you pick no?";
		mes "You should pick yes instead!";
	}
	close;
```

Example 3: counter-controlled loop
```
	.@i = 1;
	while (.@i <= 5) {
		mes "This line will print 5 times.";
		.@i += 1;
	}
	close;
```

Example 4: sentinel-controlled loop
```
	mes "Input 0 to stop";
	input .@num;
	while (.@num != 0) {
		mes "You entered " + .@num;
		input .@num;
	}
	close;
```


## for
*for (<variable initialization>; <condition>; <variable update>) <statement>;

Another pretest looping structure is the 'for' statement. It is considered a
specialized form of the 'while' statement, and is usually associated with counter-
controlled loops. Here are the steps of the 'for' statement: the initialize
statement is executed first and only once. The condition test is performed.
When the condition evaluates to false, the rest of the for statement is skipped.
When the condition evaluates to true, the body of the loop is executed, then the
update statement is executed (this usually involves incrementing a variable).
Then the condition is reevaluated and the cycle continues.

Example 1:
	for( .@i = 1; .@i <= 5; .@i++ )
		mes "This line will print 5 times.";

Example 2:
	mes "This will print the numbers 1 - 5.";
	for( .@i = 1; .@i <= 5; .@i++ )
		mes "Number: " + .@i;

---------------------------------------

## do
*do { <statement>; } while (<condition>);

The 'do...while' is the only post-test loop structure available in this script
language. With a post-test, the statements are executed once before the condition
is tested. When the condition is true, the statement(s) are repeated. When the
condition is false, control is transferred to the statement following the
'do...while' loop expression.

Example 1: sentinel-controlled loop
	mes "This menu will keep appearing until you pick Cancel";
	do {
		.@menu = select("One:Two:Three:Cancel");
	} while (.@menu != 4);

Example 2: counter-controlled loop
	mes "This will countdown from 10 to 1.";
	.@i = 10;
	do {
		mes .@i;
		.@i -= 1;
	} while (.@i > 0);

---------------------------------------

## freeloop
*freeloop({<toggle>})

Toggling this to enabled (1) allows the script instance to bypass the infinite loop
protection, allowing your script to loop as much as it may need. Disabling (0) will
warn you if an infinite loop is detected.

The command will return the state of freeloop for the attached script, even if no
argument is provided.

Example:
	freeloop(1); // enable script to loop freely

	// be careful with what you do here
	for ( .@i = 0; .@i < .@bigloop; .@i++ ) {
		dothis;
		// will sleep the script for 1ms when detect an infinity loop to
		// let rAthena do what it needs to do (socket, timer, process, etc.)
	}

	freeloop(0); // disable freeloop

	for ( .@i = 0; .@i < .@bigloop; .@i++ ) {
		dothis;
		// throw an infinity loop error
	}

---------------------------------------

## setarray
*setarray <array name>[<first value>],<value>{,<value>...<value>};

This command will allow you to quickly fill up an array in one go. Check the
Kafra scripts in the distribution to see this used a lot.

    setarray .@array[0], 100, 200, 300, 400, 500, 600;

First value is the index of the first element of the array to alter. For
example:

    setarray .@array[0],200,200,200;
    setarray .@array[1],300,150;

will produce:

 .@array[0]=200
 .@array[1]=300
 .@array[2]=150

---------------------------------------

## cleararray
*cleararray <array name>[<first value to alter>],<value>,<number of values to set>;

This command will change many array values at the same time to the same value.

    setarray .@array[0], 100, 200, 300, 400, 500, 600;
    // This will make all 6 values 0
    cleararray .@array[0],0,6;
    // This will make array element 0 change to 245
    cleararray .@array[0],245,1;
    // This will make elements 1 and 2 change to 345
    cleararray .@array[1],345,2;

See 'setarray'.

---------------------------------------

## copyarray
*copyarray <destination array>[<first value>],<source array>[<first value>],<amount of data to copy>;

This command lets you quickly shuffle a lot of data between arrays, which is in
some cases invaluable.

    setarray .@array[0], 100, 200, 300, 400, 500, 600;
    // So we have made .@array[]
    copyarray .@array2[0],@array[2],2;

    // Now, .@array2[0] will be equal to .@array[2] (300) and
    // .@array2[1] will be equal to .@array[3].

So using the examples above:
 .@array[0] = 100
 .@array[1] = 200
 .@array[2] = 300
 .@array[3] = 400
 .@array[4] = 500
 .@array[5] = 600

New Array:
 .@array2[0] = 300
 .@array2[1] = 400
 .@array2[2] = 0
 .@array2[3] = 0

Notice that .@array[4] and .@array[5] won't be copied to the second array, and it will return a
0.

---------------------------------------

## deletearray
*deletearray <array name>[<first value>],<how much to delete>;

This command will delete a specified number of array elements totally from an
array, shifting all the elements beyond this towards the beginning.

    // This will delete array element 0, and move all the other array elements
    // up one place.
    deletearray .@array[0],1

// This would delete array elements numbered 1, 2 and 3, leave element 0 in its
// place, and move the other elements ups, so there are no gaps.

    deletearray .@array[1],3

---------------------------------------

## inarray
*inarray <array name>,<value>;

This command returns the index of the first matching value found in the array.
It will return -1 if the value is not found.

	setarray .@array[0], 100, 200, 300, 400, 500, 600, 100;
	
	inarray(.@array[0], 200);
	//return 1 because 200 is in index 1
	//another way to say it that .@array[1] == 200
	
	.@index = inarray(.@array[0], 600);
	//.@index is now 5 because .@array[5] == 600
	
	inarray(.@array[0], 100);
	//while index 6 is also 100, the command will return the first instance it finds
	//return 0 because .@array[0] == 100

	inarray(.@array[0], 800);
	//return -1 because 800 is not an element of the array .@array

For more details, see the sample in 'doc/sample/inarray.txt'.

---------------------------------------

## countinarray
*countinarray <array name>{[<start index>]},<array name>{[<start index>]};

This command will check for matches between the array values and return the number of matches.
While being optional, if [<start index>] is supplied, the search will begin from the given index value.

	setarray .@array[0], 100, 200, 300, 400, 500, 600;
	
	.@variable = 100;
	if(countinarray(.@array[0], .@variable))
		mes "The number 100 was found in the array .@array";
	
	countinarray(.@array[0], .@variable);
	//return 1 because the number 100 is an element of the array .@array
	
	setarray .@array2[0],100,500;
	countinarray(.@array[0], .@array2[0]);
	//return 2 because the numbers 100 and 500 are elements of the array .@array
	
	setarray .@array3[0],100,700;
	countinarray(.@array[0], .@array3[0]);
	//return 1 because the number 100 is an element of the array .@array
	//but the number 700 is not an element of the array .@array

	//also you can change the position between the arrays in the command
	if(countinarray(.@array[0], .@array3[0]) == countinarray(.@array3[0], .@array[0]))
		//This is true

For more details, see the sample in 'doc/sample/inarray.txt'.

---------------------------------------


# 2. Information-retrieving commands

## strcharinfo
*strcharinfo(<type>{,<char_id>})

This function will return either the name, party name or guild name for the
invoking character. Whatever it returns is determined by type.

 0 - Character's name.
 1 - The name of the party they're in if any.
 2 - The name of the guild they're in if any.
 3 - The name of the map the character is in.

If a character is not a member of any party or guild, an empty string will be
returned when requesting that information.

---------------------------------------

## convertpcinfo
*convertpcinfo(<char_id>,<type>)
*convertpcinfo(<account_id>,<type>)
*convertpcinfo(<player_name>,<type>)

This function will return the information <type> for the
specified character. Whatever it returns is determined by type.

 CPC_NAME    - Character's name.
 CPC_CHAR    - Character ID.
 CPC_ACCOUNT - Account ID.

If a character is not found (or not online) when requesting that information,
an empty string will be returned for CPC_NAME, 0 for other <type>.

---------------------------------------

## strnpcinfo
*strnpcinfo(<type>)

This function will return the various parts of the name of the calling NPC.
Whatever it returns is determined by type.

 0 - The NPC's display name (visible#hidden)
 1 - The visible part of the NPC's display name
 2 - The hidden part of the NPC's display name
 3 - The NPC's unique name (::name)
 4 - The name of the map the NPC is in.

---------------------------------------

## getarraysize
*getarraysize(<array name>)

This function returns highest index of the array that is filled.
Notice that zeros and empty strings at the end of this array are not
counted towards this number.

For example:

    setarray .@array[0], 100, 200, 300, 400, 500, 600;
    set .@arraysize,getarraysize(.@array);

This will make .@arraysize == 6. But if you try this:

    setarray .@array[0], 100, 200, 300, 400, 500, 600, 0;
    set .@arraysize,getarraysize(.@array);

.@arraysize will still equal 6, even though you've set 7 values.

---------------------------------------

## getelementofarray
*getelementofarray(<array name>,<index>)

This command retrieves the value of the element of given array at given index.
This is equivalent to using:

    <array name>[<index>]

The reason for this is, that this short form is internally converted into a call
to getelementofarray, when the script is loaded.

Also useful when passing arrays to functions or accessing another npc's arrays:
    getelementofarray(getarg(0),<index>)
    getelementofarray(getvariableofnpc(.var, "testNPC"),<index>)

---------------------------------------

## readparam
*readparam(<parameter number>{,"<character name>"})
*readparam(<parameter number>{,<char_id>})

This function will return the specified stat of the invoking character, or, if a
character name or character id is specified, of that player. The stat can either
be a number or parameter name, defined in 'src/map/script_constants.hpp'.

Some example parameters:

StatusPoint, BaseLevel, SkillPoint, Class, Upper, Zeny, Sex, Weight, MaxWeight,
JobLevel, BaseExp, JobExp, NextBaseExp, NextJobExp, Hp, MaxHp, Sp, MaxSp,
BaseJob, Karma, Manner, bVit, bDex, bAgi, bStr, bInt, bLuk

All of these also behave as variables, but don't expect to be able to just 'set'
them - some will not work for various internal reasons.

Example 1:

    // Returns how many status points you haven't spent yet.
    mes "Unused status points: " + readparam(9);

Using this particular information as a function call is not required. Typing this
will return the same result:

    mes "Unused status points: " + StatusPoint;

Example 2:

You can also use this command to get stat values.

    if (readparam(bVit) > 77)
        mes "Only people with over 77 Vit are reading this!";

---------------------------------------

## getcharid
*getcharid(<type>{,"<character name>"})

This function will return a unique ID number of the invoking character, or, if a
character name is specified, of that player.

Type is the kind of associated ID number required:

* 0 - Character ID
* 1 - Party ID
* 2 - Guild ID
* 3 - Account ID
* 4 - Battle Ground ID
* 5 - Clan ID

For most purposes other than printing it, a number is better to have than a name
(people do horrifying things to their character names).

If the character is not in a party or not in a guild, the function will return 0
if guild or party number is requested. If a name is specified and the character
is not found, 0 is returned.

If getcharid(0) returns a zero, the script got called not by a character and
doesn't have an attached RID. Note that this will cause the map server to
print "player not attached!" error messages, so it is preferred to use
"playerattached" to check for the character attached to the script.

if (getcharid(2) == 0)
	mes "Only members of a guild are allowed here!";

---------------------------------------

## getnpcid
*getnpcid(<type>{,"<npc name>"});

Retrieves IDs of the currently invoked NPC. If a unique npc name is
given, IDs of that NPC are retrieved instead. Type specifies what ID
to retrieve and can be one of the following:

    0 - NPC Game ID

If an invalid type is given or the NPC does not exist, 0 is returned.

---------------------------------------

## getchildid
*getchildid({<char_id>})
*getmotherid({<char_id>})
*getfatherid({<char_id>})

These functions return the character ID of the attached player's child,
mother, mother, or father, respectively. It returns 0 if no ID is found.

    if (getmotherid()) mes "Your mother's ID is: " + getmotherid();

---------------------------------------

## ispartneron
*ispartneron({<char_id>})

This function returns 1 if the invoking character's marriage partner is
currently online and 0 if they are not or if the character has no partner.

---------------------------------------

## getpartnerid
*getpartnerid({<char_id>})

This function returns the character ID of the invoking character's marriage
partner, if any. If the invoking character is not married, it will return 0,
which is a quick way to see if they are married:

    if (getpartnerid()) mes "I'm not going to be your girlfriend!";
    if (getpartnerid()) mes "You're married already!";

---------------------------------------

## getlook
*getlook(<type>{,<char_id>})

This function will return the number for the current character look value
specified by type. See 'setlook' for valid look types.

This can be used to make a certain script behave differently for characters
dressed in black.

---------------------------------------

## getsavepoint
*getsavepoint(<information type>{,<char_id>})

This function will return information about the invoking character's save point.
You can use it to let a character swap between several recorded save points.
Available information types are:

* 0 - Map name (a string)
* 1 - X coordinate
* 2 - Y coordinate

---------------------------------------

## getcharip
*getcharip({"<character name>"|<account id>|<char id>})

This function will return the IP address of the invoking character, or, if a player
is specified, of that character. A blank string is returned if no player is attached.

Examples:

// Outputs IP address of attached player.
	mes "Your IP: " + getcharip();

// Outputs IP address of character "Silver".
	mes "Silver's IP: " + getcharip("Silver");

---------------------------------------

## vip_status
*vip_status(<type>,{"<character name>"})

Returns various information about a player's VIP status.

Valid types:
 VIP_STATUS_ACTIVE - VIP status: true if the player is a VIP or false if not
 VIP_STATUS_EXPIRE - VIP expire timestamp if the player is VIP or 0 if not
 VIP_STATUS_REMAINING - VIP time remaining in seconds

NOTE: This command is only available if the VIP System is enabled.

---------------------------------------

## vip_time
*vip_time <time>,{"<character name>"};

Changes a player's VIP time (in minutes). A positive value will increase time, and a
negative value will decrease time.

NOTE: This command is only available if the VIP System is enabled.

---------------------------------------

## addspiritball
*addspiritball <count>,<duration>{,<char_id>};

Adds spirit ball to player for 'duration' in milisecond.

---------------------------------------

## delspiritball
*delspiritball <count>{,<char_id>};

Deletes the spirit ball(s) from player.

---------------------------------------

## countspiritball
*countspiritball {<char_id>};

Counts the spirit ball that player has.

---------------------------------------

## ignoretimeout
*ignoretimeout <flag>{,<char_id>};

Disables the SECURE_NPCTIMEOUT function on the character invoking the script,
or by the given character ID/character name.

Valid flag:
 0 - Enabled SECURE_NPCTIMEOUT.
 1 - Disable SECURE_NPCTIMEOUT.

Note: SECURE_NPCTIMEOUT must be enabled for this to work.

---------------------------------------
\\
2,2 Item-related commands
\\
---------------------------------------

## getequipid
*getequipid({<equipment slot>,<char_id>})

This function returns the item ID of the item slot that calls the script
on the invoking character or the specified equipment slot. If nothing is
equipped there, it returns -1.
Valid equipment slots are:

```
EQI_COMPOUND_ON (-1)      - Item slot that calls this script (In context of item script)
EQI_ACC_L (0)             - Accessory 1
EQI_ACC_R (1)             - Accessory 2
EQI_SHOES (2)             - Footgear (shoes, boots)
EQI_GARMENT (3)           - Garment (mufflers, hoods, manteaux)
EQI_HEAD_LOW (4)          - Lower Headgear (beards, some masks)
EQI_HEAD_MID (5)          - Middle Headgear (masks, glasses)
EQI_HEAD_TOP (6)          - Upper Headgear
EQI_ARMOR (7)             - Armor (jackets, robes)
EQI_HAND_L (8)            - Left hand (weapons, shields)
EQI_HAND_R (9)            - Right hand (weapons)
EQI_COSTUME_HEAD_TOP (10) - Upper Costume Headgear
EQI_COSTUME_HEAD_MID (11) - Middle Costume Headgear
EQI_COSTUME_HEAD_LOW (12) - Lower Costume Headgear
EQI_COSTUME_GARMENT (13)  - Costume Garment
EQI_AMMO (14)    		  - Arrow/Ammunition
EQI_SHADOW_ARMOR (15)     - Shadow Armor
EQI_SHADOW_WEAPON (16)    - Shadow Weapon
EQI_SHADOW_SHIELD (17)    - Shadow Shield
EQI_SHADOW_SHOES (18)     - Shadow Shoes
EQI_SHADOW_ACC_R (19)     - Shadow Accessory 2
EQI_SHADOW_ACC_L (20)     - Shadow Accessory 1
```

Notice that a few items occupy several equipment slots, and if the character is
wearing such an item, 'getequipid' will return its ID number for either slot.

Can be used to check if you have something equipped, or if you haven't got
something equipped:
```
	if (getequipid(EQI_HEAD_TOP) == 2234)
		mes "What a lovely Tiara you have on";
	else
		mes "Come back when you have a Tiara on";
	close;
```

You can also use it to make sure people don't pass a point before removing an
item totally from them. Let's say you don't want people to wear Legion Plate
armor, but also don't want them to equip if after the check, you would do this:
```
	if (getequipid(EQI_ARMOR) == 2341 || getequipid(EQI_ARMOR) == 2342) {
		mes "You are wearing some Legion Plate Armor, please drop that in your stash before continuing";
		close;
	}
	// the || is used as an or argument, there is 2341 and 2342 cause there are
	// two different legion plate armors, one with a slot one without.

	if (countitem(2341) > 0 || countitem(2432) > 0) {
		mes "You have some Legion Plate Armor in your inventory, please drop that in your stash before continuing";
		close;
	}
	mes "I will lets you pass.";
	close2;
	warp "place",50,50;
	end;
```

---------------------------------------

## getequipuniqueid
*getequipuniqueid(<equipment slot>{,<char_id>})

This function returns the unique ID (as a string) of the item equipped in the equipment slot
specified on the invoking character. If nothing is equipped there, it returns an empty string.
See 'getequipid' for a full list of valid equipment slots.

---------------------------------------

## getequipname
*getequipname(<equipment slot>{,<char_id>})

Returns the jname of the item equipped in the specified equipment slot on the
invoking character, or an empty string if nothing is equipped in that position.
Does the same thing as getitemname(getequipid()). Useful for an NPC to state
what your are wearing, or maybe saving as a string variable.
See 'getequipid' for a full list of valid equipment slots.

        if ( getequipname(EQI_HEAD_TOP) != "" )
	        mes "So you are wearing a " + getequipname(EQI_HEAD_TOP) + " on your head";
	else
	        mes "You are not wearing any head gear";

---------------------------------------

## getitemname
*getitemname(<item id>)

Given the database ID number of an item, this function will return the text
stored in the 'japanese name' field (which, in rAthena, stores an English name
the players would normally see on screen.)

---------------------------------------

## getbrokenid
*getbrokenid(<number>{,<char_id>})

This function will search the invoking character's inventory for any broken
items, and will return their item ID numbers. Since the character may have
several broken items, 1 given as an argument will return the first one found, 2
will return the second one, etc. Will return 0 if no such item is found.
```
	// Let's see if they have anything broken:
	if (getbrokenid(1) == 0)
		mes "You don't have anything broken, quit bothering me.";
	else
	// They do, so let's print the name of the first broken item:
		mes "Oh, I see you have a broken " + getitemname(getbrokenid(1)) + " here!";
	end;
```

---------------------------------------

## getequipisequiped
*getequipisequiped(<equipment slot>{,<char_id>})

This functions will return 1 if there is an equipment placed on the specified
equipment slot and 0 otherwise. For a list of equipment slots
see 'getequipid'. Function originally used by the refining NPCs:
```
    if (getequipisequiped(EQI_HEAD_TOP)) {
        mes "[Refiner]";
        mes "That's a fine hat you are wearing there...";
        close;
	} else {
		mes "[Refiner]";
		mes "Do you want me to refine your dumb head?";
		close;
	}
```

---------------------------------------

## getequipisenableref
*getequipisenableref(<equipment slot>{,<char_id>})

Will return 1 if the item equipped on the invoking character in the specified
equipment slot is refinable, and 0 if it isn't. For a list of equipment slots
see 'getequipid'.
```
	if (getequipisenableref(EQI_HEAD_TOP)) {
		mes "[Refiner]";
		mes "Ok I can refine this";
		close;
	} else {
		mes "[Refiner]";
		mes "I can't refine this hat!...";
		close;
	}
```
---------------------------------------

## getequiprefinerycnt
*getequiprefinerycnt(<equipment slot>{,<char_id>})

Returns the current number of pluses for the item in the specified equipment
slot. For a list of equipment slots see 'getequipid'.

Can be used to check if you have reached a maximum refine value, default for
this is +10:
```
	if (getequiprefinerycnt(EQI_HEAD_TOP) < 10)
		mes "I will now upgrade your " + getequipname(EQI_HEAD_TOP);
	else
		mes "Sorry, it's not possible to refine hats better than +10";
	close;
```

---------------------------------------

## getequipweaponlv
*getequipweaponlv(<equipment slot>{,<char_id>})

This function returns the weapon level for the weapon equipped in the specified
equipment slot on the invoking character. For a list of equipment slots see
'getequipid'.

Only EQI_HAND_L and EQI_HAND_R normally make sense, since only weapons have
a weapon level. You can, however, probably, use this field for other equippable
custom items as a flag or something.

If no item is equipped in this slot, or if it doesn't have a weapon level
according to the database, 0 will be returned.

Examples:
```
    // Right hand can only contain a weapon.
    switch (getequipweaponlv(EQI_HAND_R)) {
      case 1: mes "You are holding a lvl 1 weapon."; break;
      case 2: mes "You are holding a lvl 2 weapon."; break;
      case 3: mes "You are holding a lvl 3 weapon."; break;
      case 4: mes "You are holding a lvl 4 weapon."; break;
      case 5: mes "You are holding a lvl 5 weapon, hm, must be a custom design..."; break;
      default: mes "Seems you don't have a weapon on."; break;
    }

    // Left hand can hold either a weapon or shield.
    if (getequipid(EQI_HAND_R) == 0) {
        mes "Seems you have nothing equipped here.";
        close;
    }
    switch (getequipweaponlv(EQI_HAND_L)) {
      case 0: mes "You are holding a shield, so it doesn't have a level."; break;
      case 1: mes "You are holding a lvl 1 weapon."; break;
      case 2: mes "You are holding a lvl 2 weapon."; break;
      case 3: mes "You are holding a lvl 3 weapon."; break;
      case 4: mes "You are holding a lvl 4 weapon."; break;
      case 5: mes "You are holding a lvl 5 weapon, hm, must be a custom design..."; break;
    }
```

---------------------------------------

## getequippercentrefinery
*getequippercentrefinery(<equipment slot>{,<enriched>,<char_id>})

This function calculates and returns the percent value chance to successfully
refine the item found in the specified equipment slot of the invoking character
by +1. There is no actual formula, the success rate for a given weapon level of
a certain refine level is found in the db/(pre-)re/refine_db.yml file. For a list of
equipment slots see 'getequipid'.

If enriched parameter is set to true, chance to successfully refine the item with
enriched material is returned instead.

These values can be displayed for the player to see, or used to calculate the
random change of a refine succeeding or failing and then going through with it
(which is what the official NPC refinery scripts use it for)

// This will find a random number from 0 - 99 and if that is equal to or more
// than the value recovered by this command it will go to L_Fail
    if (getequippercentrefinery(EQI_HAND_L)<=rand(100)) goto L_Fail;

---------------------------------------

## getequiprefinecost
*getequiprefinecost(<equipment slot>,<type>,<information>{,<char id>})

This function returns refine cost for equipment in <equipment slot> based on
passed arguments <type> and <information>.

Valid cost types are:
```
REFINE_COST_NORMAL     - For normal refining
REFINE_COST_OVER10     - For refining over +10
REFINE_COST_HD         - For refining with HD ores
REFINE_COST_ENRICHED   - For refining with enriched ores
REFINE_COST_OVER10_HD  - For refining over +10 with HD ores
REFINE_COST_HOLINK     - For refining at Holink in Malangdo
REFINE_COST_WAGJAK     - For refining at Refining Machine Wagjak in the Novice Academy
```

This function will return required cost for refining based on <information> argument.

Valid information types are:

REFINE_ZENY_COST       - Zeny
REFINE_MATERIAL_ID     - Material Item ID

This function will return -1 on failure. The function fails if the cost type
is invalid or if there is no item in the equipment slot. 

---------------------------------------

## getareadropitem
*getareadropitem("<map name>",<x1>,<y1>,<x2>,<y2>,<item>)

This function will count all the items with the specified ID number lying on the
ground on the specified map within the x1/y1-x2/y2 square on it and return that
number.

This is the only function around where a parameter may be either a string or a
number! If it's a number, it means that only the items with that item ID number
will be counted. If it is a string, it is assumed to mean the 'english name'
field from the item database. If you give it an empty string, or something that
isn't found from the item database, it will count items number '512' (apples).

---------------------------------------

## getequipcardcnt
*getequipcardcnt(<equipment slot>)

This function will return the number of cards that have been compounded onto a
specific equipped item for the invoking character. See 'getequipid' for a list
of possible equipment slots.

---------------------------------------

## getinventorylist
*getinventorylist {<char_id>};

This command sets a bunch of arrays with a complete list of whatever the
invoking character has in their inventory, including all the data needed to
recreate these items perfectly if they are destroyed. Here's what you get:

```
@inventorylist_id[]                - array of item ids.
@inventorylist_amount[]            - their corresponding item amounts.
@inventorylist_equip[]             - on which position the item is equipped (see EQP_* constants)
                                     It will contain 0 if the item is not equipped.
@inventorylist_refine[]            - for how much it is refined.
@inventorylist_identify[]          - whether it is identified.
@inventorylist_attribute[]         - whether it is broken.
@inventorylist_card1[]             - These four arrays contain card data for the items.
@inventorylist_card2[]               These data slots are also used to store names
@inventorylist_card3[]               inscribed on the items, so you can explicitly check
@inventorylist_card4[]               if the character owns an item made by a specific
                                     craftsman.
@inventorylist_expire[]            - expire time (Unix time stamp). 0 means never expires.
@inventorylist_bound[]             - the bound type of the items (see BOUND_* constants)
@inventorylist_count               - the number of items in these lists.
@inventorylist_option_id1[]        - first array of random option IDs
@inventorylist_option_value1[]     - first array of random option values
@inventorylist_option_parameter1[] - first array of random option parameters
@inventorylist_option_id2[]        - second array of random option IDs
@inventorylist_option_value2[]     - second array of random option values
@inventorylist_option_parameter2[] - second array of random option parameters
@inventorylist_option_id3[]        - third array of random option IDs
@inventorylist_option_value3[]     - third array of random option values
@inventorylist_option_parameter3[] - third array of random option parameters
@inventorylist_option_id4[]        - fourth array of random option IDs
@inventorylist_option_value4[]     - fourth array of random option values
@inventorylist_option_parameter4[] - fourth array of random option parameters
@inventorylist_option_id5[]        - fifth array of random option IDs
@inventorylist_option_value5[]     - fifth array of random option values
@inventorylist_option_parameter5[] - fifth array of random option parameters
```

This could be handy to save/restore a character's inventory, since no other
command returns such a complete set of data, and could also be the only way to
correctly handle an NPC trader for carded and named items who could resell them
- since NPC objects cannot own items, so they have to store item data in
variables and recreate the items.

Notice that the variables this command generates are all temporary, attached to
the character, and integer.

Be sure to use @inventorylist_count to go through these arrays, and not
'getarraysize', because the arrays are not automatically cleared between runs
of 'getinventorylist'.

---------------------------------------

## cardscnt
*cardscnt()

This function will return the number of cards inserted into the equipment
from which the function is called.

This function is intended for use in item scripts.

---------------------------------------

## getrefine
*getrefine()

This function will return the refine count of the equipment from which the
function is called.

This function is intended for use in item scripts.

---------------------------------------

## getnameditem
*getnameditem(<item id>,"<name to inscribe>"|<char id>);
*getnameditem("<item name>","<name to inscribe>"|<char id>);

This function is equivalent to using 'getitem', however, it will not just give
the character an item object, but will also inscribe it with a specified
character's name. You may not inscribe items with arbitrary strings, only with
names of characters that actually exist. While this isn't said anywhere
specifically, apparently, named items may not have cards in them, slots or no -
these data slots are taken by the character ID who's name is inscribed. Only one
remains free and it's not quite clear if a card may be there.

This function will return 1 if an item was successfully created and 0 if it
wasn't for whatever reason. Like 'getitem', this function will also accept an
'english name' from the item database as an item name and will return 0 if no
such item exists.

---------------------------------------

## getitemslots
*getitemslots(<item ID>)

This function will look up the item with the specified ID number in the database
and return the number of slots this kind of items has - 0 if they are not
slotted. It will also be 0 for all non-equippable items, naturally, unless
someone messed up the item database. It will return -1 if there is no such item.

Example:

//.@slots now has the amount of slots of the item with ID 1205.
	.@slots = getitemslots(1205);

---------------------------------------

## getiteminfo
*getiteminfo(<item ID>,<type>)

This function will look up the item with the specified ID number in the database
and return the info set by TYPE argument.
It will return -1 if there is no such item.

```
Valid types are:
	0  - Buy Price
	1  - Sell Price
	2  - Type
	3  - maxchance (max drop chance of this item, e.g. 1 = 0.01%)
		 if = 0, then monsters don't drop it at all (rare or a quest item)
		 if = 10000, then this item is sold in NPC shops only
	4  - Gender
	5  - Loc
	6  - Weight
	7  - ATK
	8  - DEF
	9  - Range
	10 - Slot
	11 - View
	12 - eLV
	13 - wLV
	14 - SpriteID from 'db/item_avail.txt'
	15 - eLVMax
	16 - matk if RENEWAL is defined
```

See the sample in `doc/sample/getiteminfo.txt`.

---------------------------------------

## getequipcardid
*getequipcardid(<equipment slot>,<card slot>)

Returns value from equipped item slot in the indicated slot (0, 1, 2, or 3).

This function returns CARD ID, CARD0_FORGE, CARD0_CREATE, or CARD0_PET (for card 0, if the item is produced).
It's useful for when you want to check whether an item contains cards or if it's signed.

---------------------------------------

## mergeitem
*mergeitem({,<char_id>});

Open merge item window to merge available item can be merged.

Examples
1. See the NPC 'npc/re/other/merge_item.txt'.
2. Simple usage:
    mes "Let's check if any item can be merged.";
    close2;
    mergeitem;
    end;

---------------------------------------

## mergeitem2
*mergeitem2({<item_id>{,<char_id>}});
*mergeitem2({"<item name>"{,<char_id>}});

Merge all stackable items that separated by GUID flags
(either by flag 4 item_flag.txt or GUID  in item_group).
If no item ID/name given, all possible items in player's inventory will be merged.

---------------------------------------

## getequiptradability
*getequiptradability(<equipment slot>{,<char id>});

Returns true if the item in <equipment slot> is tradable.
Returns false otherwise.

---------------------------------------

## identifyall
*identifyall({<type>{,<account_id>}});

Returns the count of unidentified items in the player inventory.
If <type> is true the command will identify all the unidentified items as well (default).
If <type> is false the command only returns the count of unidentified items.

---------------------------------------
//
2,1.- End of item-related commands.
//
---------------------------------------

## getmapxy
*getmapxy("<variable for map name>",<variable for x>,<variable for y>{,<type>,"<search value>"})

This function will locate a character object, NPC object or pet's coordinates
and place their coordinates into the variables specified when calling it. It
will return 0 if the search was successful, and -1 if the parameters given were
not variables or the search was not successful.

Type is the type of object to search for:

	BL_PC   - Character object (default)
	BL_NPC  - NPC object
	BL_PET  - Pet object
	BL_HOM  - Homunculus object
	BL_MER  - Mercenary object
	BL_ELEM - Elemental object

The search value is optional. If it is not specified, the location of the
invoking character will always be returned for types BL_PC and BL_PET,
the location of the NPC running this function for type BL_NPC.

If a search value is specified, for types BL_PC and BL_NPC, the
character or NPC with the specified name or GID will be located.

If type is BL_PET/BL_HOM/BL_MER/BL_ELEM, the search
will locate the current object of the character who's name/GID is given in the
search value, it will NOT locate the object by name.

Example:
```
    prontera,164,301,3%TAB%script%TAB%Meh%TAB%730,{
        mes "My name is Meh. I'm here so that Nyah can find me.";
        close;
    }

    prontera,164,299,3%TAB%script%TAB%Nyah%TAB%730,{
        mes "My name is Nyah.";
        mes "I will now search for Meh all across the world!";
        if (getmapxy(.@mapname$, .@mapx, .@mapy, BL_NPC, "Meh") != 0) {
                mes "I can't seem to find Meh anywhere!";
                close;
        }
        mes "And I found him on map " + .@mapname$ + " at X:" + .@mapx + " Y:" + .@mapy + " !";
        close;
   }
```

Notice that NPC objects disabled with 'disablenpc' will still be located.

---------------------------------------

## mapid2name
*mapid2name(<map ID>)

Returns the map name of the given map ID. Returns an empty string if given
map ID doesn't exist.

---------------------------------------

## getgmlevel
*getgmlevel({<char_id>})

This function will return the (GM) level associated with the player group to which
the invoking character belongs. If this is somehow executed from a console command,
99 will be returned, and 0 will be returned if the account has no GM level.

This allows you to make NPC's only accessible for certain GM levels, or behave
specially when talked to by GMs.

   if (getgmlevel()) mes "What is your command, your godhood?";

---------------------------------------

## getgroupid
*getgroupid({<char_id>})

This function will return the group id to which the invoking player belongs.

---------------------------------------

## gettimetick
*gettimetick(<tick type>)

This function will return a tick depending on <tick type>:
 0: The server's tick, a measurement in milliseconds used by the server's timer
    system. This tick is an unsigned int which loops every ~50 days.
 1: The time, in seconds, since the start of the current day.
 2: The system time in UNIX epoch time, or the number of seconds elapsed since
    January 1st, 1970. Useful for reliably measuring time intervals.

---------------------------------------

## gettime
*gettime(<type>)

This function will return specified information about the current system time.

DT_SECOND - Seconds (of the current minute)
DT_MINUTE - Minutes (of the current hour)
DT_HOUR - Hour (of the current day)
DT_DAYOFWEEK - Week day (constants for MONDAY to SUNDAY are available)
DT_DAYOFMONTH - Day of the current month
DT_MONTH - Month (constants for JANUARY to DECEMBER are available)
DT_YEAR - Year
DT_DAYOFYEAR - Day of the year
DT_YYYYMMDD - current date in the form YYYYMMDD

It will only return numbers. If another type is supplied -1 will be returned.

	if (gettime(DT_DAYOFWEEK) == SATURDAY) mes "It's a Saturday. I don't work on Saturdays.";

---------------------------------------

## gettimestr
*gettimestr(<"time format">,<max length>{,<time_tick>})

This function will return a string containing time data as specified by the
time format.

This uses the C function 'strfmtime', which obeys special format characters. For
a full description see, for example, the description of 'strfmtime' at
http://www.delorie.com/gnu/docs/glibc/libc_437.html
All the format characters given in there should properly work.
Max length is the maximum length of a time string to generate.

The example given in rAthena sample scripts works like this:

  mes gettimestr("%Y-%m/%d %H:%M:%S",21);

The example above will print the current date and time like 'YYYY-MM/DD HH:MM:SS'.
The following example will print the date and time when the player's VIP status
expires by the given <time_tick>:

  mes gettimestr("%Y-%m/%d %H:%M:%S",21,vip_status(VIP_STATUS_EXPIRE));

---------------------------------------

## getusers
*getusers(<type>)

This function will return a number of users on a map or the whole server. What
it returns is specified by Type.

Type can be one of the following values, which control what will be returned:

    0 - Count of all characters on the map of the invoking character.
    1 - Count of all characters in the entire server.
    8 - Count of all characters on the map of the NPC the script is
        running in.

---------------------------------------

## getmapusers
*getmapusers("<map name>")

This function will return the number of users currently located on the specified
map.

This is used officially in PVP scripts to check whether a room is filled to capacity.

---------------------------------------

## getareausers
*getareausers("<map name>",<x1>,<y1>,<x2>,<y2>)

This function will return the count of connected characters which are located
within the specified area - an x1/y1-x2/y2 square on the specified map.

This is useful for maps that are split into many buildings, such as all the
"*_in" maps, due to all the shops and houses.

---------------------------------------

## getunits/getmapunits/getareaunits
*getunits(<type>{,<array_variable>[<first value>]})
*getmapunits(<type>,<"map name">{,<array_variable>[<first value>]})
*getareaunits(<type>,<"map name">,<x1>,<y1>,<x2>,<y2>{,<array_variable>[<first value>]})

The 'getunits' command will return the number of <type> objects active on the server.

The 'getmapunits' command will return the number of <type> objects active on the
specified <"map name">.

The 'getareaunits' command will return the number of <type> objects actively located
within the specified area where <x1>, <y1>, <x2>, <y2> form the area.

Type is the type of object to search for:

	BL_PC   - Character objects
	BL_MOB  - Monster objects
	BL_PET  - Pet objects
	BL_HOM  - Homunculus objects
	BL_MER  - Mercenary objects
	BL_NPC  - NPC objects
	BL_ELEM - Elemental objects

If <array_variable> is provided:
	- An int variable will return the list of GID.
	- A string variable will return the list of names.

Example 1:
```
	// getting the players count and building a string array of the names.
	.@num = getunits(BL_PC,.@array$[0]);

	mes "the number of Users Connected to the server is " + .@num + " .";
	mes "list of Players names :";
	freeloop(1);	// for if the list was too big.
	for(.@i=0;.@i<getarraysize(.@array$);.@i++)
		mes (.@i + 1) + " " + .@array$[.@i];
	freeloop(0);
	end;
```

Example 2:
```
	// getting the npc count in Prontera and building a string array of the names.
	.@num = getmapunits(BL_NPC,"prontera",.@array$[0]);

	mes "the number of NPCs in Prontera is " + .@num + " .";
	mes "list of NPCs name :";
	freeloop(1);	// for if the list was too big.
	for(.@i=0;.@i<getarraysize(.@array$);.@i++)
		mes (.@i + 1) + " " + .@array$[.@i];
	freeloop(0);
	end;
```

Example 3:
```
	// getting the monster count in Prontera with specific coordinates and building a int array of the GIDs.
	.@num = getareaunits(BL_MOB,"prontera",154,186,159,182,.@array[0]);

	mes "the number of Monsters in Prontera in that Coordinates is " + .@num + " .";
	mes "list of Monsters GID :";
	freeloop(1);	// for if the list was too big.
	for(.@i=0;.@i<getarraysize(.@array);.@i++)
		mes (.@i + 1) + " " + .@array[.@i];
	freeloop(0);
	end;
```

---------------------------------------
\\
2,2.- Guild-related commands
\\
---------------------------------------

## getguildname
*getguildname(<guild id>)

This function returns a guild's name given an ID number. If there is no such
guild, "null" will be returned.

Example:
	mes "The guild " + getguildname(10007) + " are all nice people.";

---------------------------------------

## getguildmember
*getguildmember <guild id>{,<type>{,<array_variable>}};

This command will find all members of a specified guild and returns their names
(or character id or account id depending on the value of "type") into an array
of temporary global variables.

Upon executing this,

$@guildmembername$[] is a global temporary string array which contains all the
                     names of these guild members.
                     (only set when type is 0 or not specified)

$@guildmembercid[]   is a global temporary number array which contains the
                     character id of these guild members.
                     (only set when type is 1)

$@guildmemberaid[]   is a global temporary number array which contains the
                     account id of these guild members.
                     (only set when type is 2)

$@guildmembercount   is the number of guild members that were found.

The guild members will be found regardless of whether they are online or offline.
Note that the names come in no particular order.

Be sure to use $@guildmembercount to go through this array, and not
'getarraysize', because it is not cleared between runs of 'getguildmember'.

If 'array_variable' is set, the result will be stored to that variable instead
using global variable.

For usage examples, see 'getpartymember'.

---------------------------------------

## getguildmaster
*getguildmaster(<guild id>)

This function return the name of the master of the guild which has the specified
ID number. If there is no such guild, "null" will be returned.

Example 1:
	// Prints the guild master of guild 10007, whoever that might be.
	mes getguildmaster(10007) + " runs " + getguildname(10007);

Example 2:
	// Checks if the character is the guild master of the specified guild.
	.@GID = getcharid(2);
	if (.@GID == 0) {
		mes "Sorry, you are not in a guild.";
		close;
	}
	if (strcharinfo(0) != getguildmaster(.@GID)) {
		mes "Sorry, you don't own the guild you are in.";
		close;
	}
	mes "Welcome, guild master of " + getguildname(.@GID);
	close;

---------------------------------------

## getguildmasterid
*getguildmasterid(<guild id>)

This function will return the character ID number of the guild master of the
guild specified by the ID. 0 if the character is not a guild master of any guild.

---------------------------------------

## is_guild_leader
*is_guild_leader({<guild ID>})

This command will return true if the player attached to the script is the leader
of his/her guild, or, if a guild ID is specified, of that guild.

---------------------------------------

## getcastlename
*getcastlename("<map name>")

This function returns the name of the castle when given the map name for that
castle. The data is read from 'db/castle_db.txt'.

---------------------------------------

## getcastledata
*getcastledata("<map name>",<type of data>)
## setcastledata
*setcastledata "<map name>",<type of data>,<value>;

This function returns the castle ownership information for the castle referred
to by its map name. Castle information is stored in `guild_castle` SQL table.

Types of data correspond to `guild_castle` table columns:

CD_GUILD_ID          - Guild ID.
CD_CURRENT_ECONOMY   - Castle Economy score.
CD_CURRENT_DEFENSE   - Castle Defense score.
CD_INVESTED_ECONOMY  - Number of times the economy was invested in today.
CD_INVESTED_DEFENSE  - Number of times the defense was invested in today.
CD_NEXT_TIME         - unused
CD_PAY_TIME          - unused
CD_CREATE_TIME       - unused
CD_ENABLED_KAFRA     - Is 1 if a Kafra was hired for this castle, 0 otherwise.
CD_ENABLED_GUARDIAN0 - Is 1 if the 1st guardian is present (Soldier Guardian)
CD_ENABLED_GUARDIAN1 - Is 1 if the 2nd guardian is present (Soldier Guardian)
CD_ENABLED_GUARDIAN2 - Is 1 if the 3rd guardian is present (Soldier Guardian)
CD_ENABLED_GUARDIAN3 - Is 1 if the 4th guardian is present (Archer Guardian)
CD_ENABLED_GUARDIAN4 - Is 1 if the 5th guardian is present (Archer Guardian)
CD_ENABLED_GUARDIAN5 - Is 1 if the 6th guardian is present (Knight Guardian)
CD_ENABLED_GUARDIAN6 - Is 1 if the 7th guardian is present (Knight Guardian)
CD_ENABLED_GUARDIAN7 - Is 1 if the 8th guardian is present (Knight Guardian)

All types of data have their meaning determined by War of Emperium scripts,
with exception of:
 - CD_GUILD_ID that is always considered ID of the guild that owns the castle,
 - CD_CURRENT_DEFENSE that is used in Guardians & Emperium HP calculations,
 - CD_ENABLED_GUARDIANX that is always considered to hold guardian presence bits.

The 'setcastledata' command will behave identically, but instead of returning
values for the specified types of accessible data, it will alter them and cause
them to be sent to the char-server for storage.

Changing Guild ID or Castle Defense will trigger additional actions, like
recalculating guardians' HP.

---------------------------------------

## getgdskilllv
*getgdskilllv(<guild id>,<skill id>)
## getgdskilllv
*getgdskilllv(<guild id>,"<skill name>")

This function returns the level of the skill <skill id> of the guild <guild id>.
If the guild does not have that skill, 0 is returned.
If the guild does not exist, -1 is returned.
Refer to 'db/(pre-)re/skill_db.txt' for the full list of skills. (GD_* are guild skills)

---------------------------------------

## requestguildinfo
*requestguildinfo <guild id>{,"<event label>"};

This command requests the guild data from the char server and merrily continues
with the execution. Whenever the guild information becomes available (which
happens instantly if the guild information is already in memory, or later, if it
isn't and the map server has to wait for the char server to reply) it will run
the specified event as in a 'donpcevent' call.

---------------------------------------

## getmapguildusers
*getmapguildusers("<map name>",<guild id>)

Returns the amount of characters from the specified guild on the given map.

Example:

mes "You have " + getMapGuildUsers("prontera",getcharid(2)) + " guild members in Prontera.";

---------------------------------------
//
2,2.- End of guild-related commands
//
---------------------------------------

## getskilllv
*getskilllv(<skill id>)
## getskilllv
*getskilllv("<skill name>")

This function returns the level of the specified skill that the invoking
character has. If they don't have the skill, 0 will be returned. The full list
of character skills is available in 'db/(pre-)re/skill_db.txt'.

There are two main uses for this function, it can check whether the character
has a skill or not, and it can tell you if the level is high enough.

Example 1:
	if (getskilllv(152))
		mes "You have got the skill Throw Stone";
	else
		mes "You don't have Throw Stone";
	close;

Example 2:
	if (getskilllv(28) >= 5)
		mes "Your heal lvl is 5 or more";
	else if (getskilllv(28) == 10)
		mes "Your heal lvl has been maxed";
	else
		mes "You heal skill is below lvl 5";
	close;

---------------------------------------

## getskilllist
*getskilllist({<char_id>});

This command sets a bunch of arrays with a complete list of skills the
invoking character has. Here's what you get:

@skilllist_id[]   - skill ids.
@skilllist_lv[]   - skill levels.
@skilllist_flag[] - see 'skill' for the meaning of skill flags.
@skilllist_count  - number of skills in the above arrays.

While 'getskillv' is probably more useful for most situations, this is the
easiest way to store all the skills and make the character something else for a
while. Advanced job for a day? This could also be useful to see how many
skills a character has.

This command does not count skills which are set as flag 4 (permament granted) (ALL_BUYING_STORE/ALL_INCCARRY)

---------------------------------------

## getmonsterinfo
*getmonsterinfo(<mob ID>,<type>)

This function will look up the monster with the specified ID number in the
mob database and return the info set by TYPE argument.
It will return -1 if there is no such monster (or the type value is invalid),
or "null" if you requested the monster's name.

Valid types are:
	MOB_NAME - monster's name, if there is no such monster "null" is returned
	MOB_LV - monster's level
	MOB_MAXHP - monster's maximum hp
	MOB_BASEEXP - monster's base experience
	MOB_JOBEXP - monster's job experience
	MOB_ATK1 - monster's atk
	MOB_ATK2 - monster's atk2
	MOB_DEF - monster's def
	MOB_MDEF - monster's mdef
	MOB_STR - monster's str
	MOB_AGI - monster's agi
	MOB_VIT - monster's vit
	MOB_INT - monster's int
	MOB_DEX - monster's dex
	MOB_LUK - monster's luk
	MOB_RANGE - monster's range
	MOB_RANGE2 - monster's range2
	MOB_RANGE3 - monster's range3
	MOB_SIZE - monster's size
	MOB_RACE - monster's race
	MOB_ELEMENT - monster's element(doesn't return the element level, only the element ID)
	MOB_MODE - monster's mode
	MOB_MVPEXP - monster's mvp experience

For more details, see the sample in 'doc/sample/getmonsterinfo.txt'.

---------------------------------------

## getmobdrops
*getmobdrops(<mob id>)

This command will find all drops of the specified mob and return the item IDs
and drop percentages into arrays of temporary global variables.
'getmobdrops' returns 1 if successful and 0 if the mob ID doesn't exist.

Upon executing this,

$@MobDrop_item[] is a global temporary number array which contains the
                 item IDs of the monster's drops.

$@MobDrop_rate[] is a global temporary number array which contains the
                 drop percentages of each item. (1 = .01%)

$@MobDrop_count is the number of item drops found.

Be sure to use $@MobDrop_count to go through the arrays, and not
'getarraysize', because the temporary global arrays are not cleared between
runs of 'getmobdrops'. If a mob with 7 item drops is looked up, the arrays would
have 7 elements. But if another mob is looked up and it only has 5 item drops,
the server will not clear the arrays for you, overwriting the values instead. So
in addition to returning the 5 item drops, the 6th and 7th elements from the
last call remain, and you will get 5+2 item drops, of which the last 2 don't
belong to the new mob. $@MobDrop_count will always contain the correct number
(5), unlike 'getarraysize()' which would return 7 in this case.

Example:

	// get a Mob ID from the user
	input .@mob_id;

	if (getmobdrops(.@mob_id)) {	// 'getmobdrops' returns 1 on success
		// immediately copy global temporary variables into scope variables,
		// since we don't know when 'getmobdrops' will get called again for
		// another mob, overwriting your global temporary variables
		.@count = $@MobDrop_count;
		copyarray .@item[0],$@MobDrop_item[0],.@count;
		copyarray .@rate[0],$@MobDrop_rate[0],.@count;

		mes getmonsterinfo(.@mob_id,MOB_NAME) + " - " + .@count + " drops found:";
		for( .@i = 0; .@i < .@count; .@i++ ) {
			mes .@item[.@i] + " (" + getitemname(.@item[.@i]) + ") " + .@rate[.@i]/100 + ((.@rate[.@i]%100 < 10) ? ".0":".") + .@rate[.@i]%100 + "%";
		}
	} else {
		mes "Unknown monster ID.";
	}
	close;

---------------------------------------

## skillpointcount
*skillpointcount({<char_id>})

Returns the total amount of skill points a character possesses (SkillPoint+SP's used in skills)
This command can be used to check the currently attached characters total amount of skill points.
This means the skill points used in skill are counted, and added to SkillPoints (number of skill points not used).
This command does not count skills which are set as flag 4 (permament granted) (ALL_BUYING_STORE/ALL_INCCARRY)

Example 1:
	.@skillPoints = skillpointcount();
	mes "You have " + .@skillPoints + " skill points in total!";

Example 2:
	if (skillpointcount() > 20)
		mes "Wow, you have more then 20 Skill Points in total!";

---------------------------------------

## getscrate
*getscrate(<effect type>,<base rate>{,<GID>})

This function will return the chance of a status effect affecting the invoking
character, in percent, modified by the their current defense against said
status. The 'base rate' is the base chance of the status effect being inflicted,
in percent.

    if (rand(100) > getscrate(Eff_Blind, 50)) goto BlindHimNow;

You can see the full list of available effect types you can possibly inflict in
'db/const.txt' under 'Eff_'.

---------------------------------------


# 3. Checking commands

## playerattached
*playerattached()

Returns the ID of the player currently attached to the script. It will return
0 if no one is attached, or if the attached player no longer exists on the map
server. It is wise to check for the attached player in script functions that
deal with timers as there's no guarantee the player will still be logged on
when the timer triggers. Note that the ID of a player is actually their
account ID.

---------------------------------------

## getattachedrid
*getattachedrid();

Returns RID from running script. Script may not be attached to any RID like
a floating script or function and will return 0.

---------------------------------------

## isloggedin
*isloggedin(<account id>{,<char id>})

This function returns 1 if the specified account is logged in and 0 if they
aren't. You can also pass the char id to check for both account and char id.

---------------------------------------

## checkweight
*checkweight(<item id>,<amount>{,<item id>,<amount>,<item id>,<amount>,...});
## checkweight
*checkweight("<item name>",<amount>{,"<item name>",<amount>,"<item name>",<amount>,...});
## checkweight2
*checkweight2(<id_array>,<amount_array>);

These functions will compute and return 1 if the total weight of the specified
number of specific items does not exceed the invoking character's carrying
capacity, and 0 otherwise. It is important to see if a player can carry the
items you expect to give them, failing to do that may open your script up to
abuse or create some very unfair errors.

The second function will check an array of items and amounts, and also
returns 1 on success and 0 on failure.

The functions, in addition to checking to see if the player is capable of
holding a set amount of items, also ensure the player has room in their
inventory for the item(s) they will be receiving.

Like 'getitem', this function will also accept an 'english name' from the
database as an argument.

Example 1:

	if (checkweight(512,10)) {
		getitem 512,10;
	} else {
		mes "Sorry, you cannot hold this amount of apples!";
	}

Example 2:

	setarray .@item[0],512,513,514;
	setarray .@amount[0],10,5,5;
	if (!checkweight2(.@item,.@amount)) {
		mes "Sorry, you cannot hold this amount of fruit!";
	}

---------------------------------------

## basicskillcheck
*basicskillcheck()

This function will return the state of the configuration option
'basic_skill_check' in 'battle_athena.conf'. It returns 1 if the option is
enabled and 0 if it isn't. If the 'basic_skill_check' option is enabled, which
it is by default, characters must have a certain number of basic skill levels to
sit, request a trade, use emotions, etc. Making your script behave differently
depending on whether the characters must actually have the skill to do all these
things might in some cases be required.

---------------------------------------

## checkoption
*checkoption(<option number>{,<char_id>})
## checkoption1
*checkoption1(<option number>{,<char_id>})
## checkoption2
*checkoption2(<option number>{,<char_id>})
## setoption
*setoption <option number>{,<flag>{,<char_id>}};

The 'setoption' series of functions check for a so-called option that is set on
the invoking character. 'Options' are used to store status conditions and a lot
of other non-permanent character data of the yes-no kind. For most common cases,
it is better to use 'checkcart','checkfalcon','checkriding' and other similar
functions, but there are some options which you cannot get at this way. They
return 1 if the option is set and 0 if the option is not set.

Option numbers valid for the first (option) version of this command are:

0x1       - Sight in effect.
0x2       - Hide in effect.
0x4       - Cloaking in effect.
0x8       - Cart number 1 present.
0x10      - Falcon present.
0x20      - Peco Peco present.
0x40      - GM Perfect Hide in effect.
0x80      - Cart number 2 present.
0x100     - Cart number 3 present.
0x200     - Cart number 4 present.
0x400     - Cart number 5 present.
0x800     - Orc head present.
0x1000    - The character is wearing a wedding sprite.
0x2000    - Ruwach is in effect.
0x4000    - Chasewalk in effect.
0x8000    - Flying or Xmas suit.
0x10000   - Sighttrasher.
0x100000  - Warg present.
0x200000  - The character is riding a warg.

Option numbers valid for the second version (opt1) of this command are:

1 - Petrified.
2 - Frozen.
3 - Stunned.
4 - Sleeping.
6 - Petrifying (the state where you can still walk)

Option numbers valid for the third version (opt2) of this command are:

0x1  - Poisoned.
0x2  - Cursed.
0x4  - Silenced.
0x8  - Signum Crucis (plays a howl-like sound effect, but otherwise no visible effects are displayed)
0x10 - Blinded.
0x80 - Deadly poisoned.

Option numbers (except for opt1) are bit-masks - you can add them up to check
for several states, but the functions will return true if at least one of them
is in effect.

'setoption' will set options on the invoking character. There are no second and
third versions of this command, so you can only change the values in the first
list (cloak, cart, ruwach, etc). if flag is 1 (default when omitted),
the option will be added to what the character currently has; if 0, the option is removed.

This is definitely not a complete list of available option flag numbers. Ask a
core developer (or read the source: src/map/status.hpp) for the full list.

---------------------------------------

## setcart
*setcart {<type>{,<char_id>}};
## checkcart
*checkcart({<char_id>});

If <type> is 0 this command will remove the cart from the character.
Otherwise it gives the invoking character a cart. The cart given will be
cart number <type> and will work regardless of whether the character is a
merchant class or not.
Note: the character needs to have the skill MC_PUSHCART to gain a cart

The accompanying function will return 1 if the invoking character has a cart
(any kind of cart) and 0 if they don't.

    if (checkcart()) mes "But you already have a cart!";

---------------------------------------

## setfalcon
*setfalcon {<flag>{,<char_id>}};
## checkfalcon
*checkfalcon({<char_id>});

If <flag> is 0 this command will remove the falcon from the character.
Otherwise it gives the invoking character a falcon. The falcon will be there
regardless of whether the character is a hunter or not. It will (probably) not
have any useful effects for non-hunters though.
Note: the character needs to have the skill HT_FALCON to gain a falcon

The accompanying function will return 1 if the invoking character has a falcon
and 0 if they don't.

    if (checkfalcon()) mes "But you already have a falcon!";

---------------------------------------

## setriding
*setriding {<flag>{,<char_id>}};
## checkriding
*checkriding({<char_id>});

If <flag> is 0 this command will remove the mount from the character.
Otherwise it gives the invoking character a PecoPeco (if they are a Knight
series class), a GrandPeco (if they are a Crusader series class), or
a Gryphon (if they are a Royal Guard). Unlike 'setfalcon' and 'setcart'
this will not work at all if they aren't of a class which can ride.
Note: the character needs to have the skill KN_RIDING to gain a mount

The accompanying function will return 1 if the invoking character is riding a
bird and 0 if they aren't.

    if (checkriding()) mes "PLEASE leave your bird outside! No riding birds on the floor here!";

---------------------------------------

## setdragon
*setdragon {<color>{,<char_id>}};
## checkdragon
*checkdragon({<char_id>});

The 'setdragon' function toggles mounting a dragon for the invoking character.
It will return 1 if successful, 0 otherwise.

The available colors are:
 1 - Green Dragon (default)
 2 - Brown Dragon
 3 - Gray Dragon
 4 - Blue Dragon
 5 - Red Dragon

Note: the character must be a Rune Knight and have the skill RK_DRAGONTRAINING to gain a mount

The accompanying function will return 1 if the invoking character is riding a
dragon and 0 if they aren't.

---------------------------------------

## setmadogear
*setmadogear {<flag>{,<char_id>}};
## checkmadogear
*checkmadogear({<char_id>});

If <flag> is 0 this command will remove the mount from the character.
Otherwise it gives the invoking character a Mado (if they are a Mechanic).

The accompanying function will return 1 if the invoking character has a
Mado and 0 if they don't.

---------------------------------------

## setmounting
*setmounting {<char_id>};
## ismounting
*ismounting({<char_id>});

The 'setmounting' function toggles cash mount for the invoking character.
It will return 1 if successful, 0 otherwise.

Note: Character must not be mounting a non-cash mount (eg. dragon, peco, wug, etc.)

The accompanying function will return 1 if the invoking character has a
cash mount and 0 if they don't.

---------------------------------------

## checkwug
*checkwug({<char_id>});

This function will return 1 if the invoking character has a
warg and 0 if they don't.

---------------------------------------

## checkvending
*checkvending({"<Player Name>"})

Checks if the player is vending or has has a buyingstore. Additionally
it gives you the information whether the player uses autotrade or not.
Name is optional, and defaults to the attached player if omitted.

The returned value is bitmask of.
  0 = doesn't have a vending or buyingstore (which also means he can't use autotrade)
  1 = normal vending
  2 = using @autotrade
  4 = has a buyingstore

Examples:
	//This will check Aaron's state
	.@state = checkvending("Aaron");
	if (.@state&1)
		mes "Aaron is currently vending!";
	if (.@state&4)
		mes "Aaron has a buying store!";
	if (.@state&2)
		mes "Aaron is autotrading!";

---------------------------------------

## checkchatting
*checkchatting({"<Player Name>"})

Checks if the player is in a chatroom.
Name is optional, and defaults to the attached player if omitted.
Returns 1 if they are in a chat room, 0 if they are not.

Examples:
	//This will check if the attached player in a chat room or not.
	if (checkchatting())
		mes "You are currently in a chat room!";

---------------------------------------

## checkidle
*checkidle({"<Player Name>"})

Returns the time, in seconds, that the specified player has been idle.
Name is optional, and defaults to the attached player if omitted.

---------------------------------------

## agitcheck
*agitcheck()
## agitcheck2
*agitcheck2()
## agitcheck3
*agitcheck3()

These function will let you check whether the server is currently in WoE:FE mode
(agitcheck()), WoE:SE mode (agitcheck2()), or WoE:TE mode (agitcheck3()) and will
return true if War of Emperium is on and false if it isn't.

---------------------------------------

## isnight
*isnight()
## isday
*isday()

These functions will return 1 or 0 depending on whether the server is in night
mode or day mode. 'isnight' returns 1 if it's night and 0 if it isn't, 'isday'
the other way around. They can be used interchangeably, pick the one you like
more:

    // These two are equivalent:
    if (isday()) mes "I only prowl in the night.";
    if (isnight() != 1) mes "I only prowl in the night.";

---------------------------------------

## checkre
*checkre(<type>)

Checks if a renewal feature is enabled or not in renewal.hpp, and returns 1 if
enabled and 0 for disabled.

The renewal feature to check is determined by type.
 0 - RENEWAL (game renewal server mode)
 1 - RENEWAL_CAST (renewal cast time)
 2 - RENEWAL_DROP (renewal drop rate algorithms)
 3 - RENEWAL_EXP (renewal exp rate algorithms)
 4 - RENEWAL_LVDMG (renewal level modifier on damage)
 5 - RENEWAL_ASPD (renewal ASPD)

---------------------------------------
\\
3,1.- Item-related commands
\\
---------------------------------------

## isequipped
*isequipped(<id>{,<id>{,<id>{,<id>}}})

This function will return 1 if the invoking character has all of the item
IDs given equipped (if card IDs are passed, then it checks if the cards are
inserted into slots in the equipment they are currently wearing). Theoretically
there is no limit to the number of items that may be tested for at the same time.
If even one of the items given is not equipped, 0 will be returned.

    // (Poring,Santa Poring,Poporing,Marin)
    if (isequipped(4001,4005,4033,4196)) mes "Wow! You're wearing a full complement of possible poring cards!";
    // (Poring)
    if (isequipped(4001)) mes "A poring card is useful, don't you think?";

The function was meant for item scripts to support the cards released by Gravity
in February 2005, but it will work just fine in normal NPC scripts.

---------------------------------------

## isequippedcnt
*isequippedcnt(<card id>{,<card id>{,<card id>{,<card id>}}})

This function is similar to 'isequipped', but instead of 1 or 0, it will return
the number of cards in the list given that were found on the invoking character.

If a given parameter is not a card, the function returns the amount of that
item equipped on the invoking character.

    if (isequippedcnt(4001,4005,4033,4196) == 4) mes "Finally got all four poring cards?";

---------------------------------------

## checkequipedcard
*checkequipedcard(<card id>)

This function will return 1 if the card specified by its item ID number is
inserted into any equipment they have in their inventory, currently equipped or
not.

---------------------------------------
//
3,1.- End of item-related commands
//
---------------------------------------

# 4. Player-related commands

## attachrid
*attachrid(<account ID>{,force})
## detachrid
*detachrid;

These commands allow the manipulation of the script's currently attached player.
While 'attachrid' allows attaching of a different player by using its account id
for the parameter RID, 'detachrid' makes the following commands run as if the
script was never invoked by a player.

The command returns false if the player cannot be attached (if the account is offline
or does not exist), and true upon success.

By default the command is executed with force, which causes to attach the player
even if he is currently attached to another script. Since this is not always the
desired behavior you can also specify false to the command and it will only return 
true if the player is online and was not attached to another script.

---------------------------------------

## addrid
*addrid(<type>{,<flag>{,<parameters>}});

This command will attach other RIDs to the current script without detaching the
invoking RID. It returns 1 if successful and 0 upon failure.

<type> determines what RIDs are attached:
 0: All players in the server.
 1: All players in the map of the invoking player, or the invoking NPC if no player is attached.
 2: Party members of a specified party ID.
    [ Parameters: <party id> ]
 3: Guild members of a specified guild ID.
    [ Parameters: <guild id> ]
 4: All players in a specified area of the map of the invoking player (or NPC).
    [ Parameters: <x0>,<y0>,<x1>,<y1> ]
 5: All players in the map.
    [ Parameters: "<map name>" ]
 Account ID: If type is Account ID, attach the specified account ID.

<flag> can prevent certain players from being attached:
 0: Players are always attached. (default)
 1: Players currently running another script will not be attached.

---------------------------------------

## rid2name
*rid2name(<rid>)

Converts rid to name. Note: The player/monster/NPC must be online/enabled.
Good for PCKillEvent where you can convert 'killedrid' to the name of the player.

Note: rid2name may not produce correct character names since rid = account id.
      It will return the current online character of the account only.

---------------------------------------

## message
*message "<character name>","<message>";

That command will send a message to the chat window of the character specified
by name. The text will also appear above the head of that character. It will not
be seen by anyone else.

---------------------------------------

## dispbottom
*dispbottom "<message>"{,<color>{,<char_id>}};

This command will send the given message with color into the invoking character's chat
window. The color format is in RGB (0xRRGGBB). The color is
by default green

---------------------------------------

## showscript
*showscript "<message>"{,<GID>, <flag>};

Makes attached player or GID says a message like shouting a skill name, the message
will be seen to everyone around but not in chat window.
flag: Specify target
   AREA - Message is sent to players in the vicinity of the source (default).
   SELF - Message is sent only to player attached.

---------------------------------------

## warp
*warp "<map name>",<x>,<y>{,<char id>};

This command will take the invoking character or <char id>, if specified, to the specified map, and if
wanted, specified coordinates too, but these can be random.

	warp "place",50,55;

This would take them to X 50 Y 55 on the map called "place". If your X and Y
coordinates land on an unwalkable map square, it will send the warped character
to a random place. Same will happen if they are both zero:

	warp "place",0,0;

Notice that while warping people to coordinates 0,0 will normally get them into
a random place, it's not certain to always be so. Darned if I know where this is
actually coded, it might be that this happens because square 0,0 is unwalkable
on all official maps. If you're using custom maps, beware.

There are also three special 'map names' you can use.

"Random" will warp the player randomly on the current map.
"Save" and "SavePoint" will warp the player back to their save point.

---------------------------------------

## areawarp
*areawarp "<from map name>",<x1>,<y1>,<x2>,<y2>,"<to map name>",<x3>,<y3>{,<x4>,<y4>};

This command is similar to 'warp', however, it will not refer to the invoking
character, but instead, all characters within a specified area, defined by the
x1/y1-x2/y2 square, will be warped. Nobody outside the area will be affected,
including the activating character, if they are outside the area.

	areawarp "place",10,10,120,120,"place2",150,150;

Everyone that is in the area between X 10 Y 10 and X 120 Y 120, in a square
shape, on the map called "place", will be affected, and warped to "place2" X 150
Y 150

	areawarp "place",10,10,120,120,"place2",0,0;

By using ,0,0; as the destination coordinates it will take all the characters in
the affected area to a random set of co-ordinates on "place2".

	areawarp "place",10,10,120,120,"place2",150,150,200,200;

By using the optional x4 and y4 parameters, the destination coordinates will be a
random place within the defined x3/y3-x4/y4 square.

Like 'warp', areawarp will also explicitly warp characters randomly into the
current map if you give the 'to map name' as "Random".

See also 'warp'.

---------------------------------------

## warpparty
*warpparty "<to_mapname>",<x>,<y>,<party_id>,{"<from_mapname>",<range x>,<range y>};

Warps a party to specified map and coordinate given the party ID, which you can get with
getcharid(1). You can also request another party id given a member's name with getcharid(1,<player_name>).

You can use the following "map names" for special warping behavior:
Random:       All party members are randomly warped in their current map (as if they
              all used a fly wing)
SavePointAll: All party members are warped to their respective save point.
SavePoint:    All party members are warped to the save point of the currently
              attached player (will fail if there's no player attached).
Leader:       All party members are warped to the leader's position. The leader must
              be online and in the current map-server for this to work.

If you specify a from_mapname, 'warpparty' will only affect those on that map.

The <range x> and <range y> optional values allow for a randomization with the
player's warp point. The values will randomly add or subtract from the given <x>
and <y> coordinates.

Example:
	mes "[Party Warper]";
	mes "Here you go!";
	close2;
	.@party_id = getcharid(1);
	warpparty "prontera",150,100,.@party_id;
	close;

---------------------------------------

## warpguild
*warpguild "<map name>",<x>,<y>,<guild_id>;

Warps a guild to specified map and coordinate given the guild id, which you can get with
getcharid(2). You can also request another guild id given the member's name with getcharid(2,<player_name>).

You can use the following "map names" for special warping behavior:
Random:       All guild members are randomly warped in their current map (as if they
              all used a fly wing)
SavePointAll: All guild members are warped to their respective save point.
SavePoint:    All guild members are warped to the save point of the currently
              attached player (will fail if there's no player attached).

Example:

warpguild "prontera",x,y,Guild_ID;

---------------------------------------

## warppartner
*warppartner("<map name>",<x>,<y>);

This function will find the invoking character's marriage partner, if any, and
warp them to the map and coordinates given. It will return 1 upon success and
0 if the partner is not online, the character is not married, or if there's no
invoking character (no RID). 0,0 will, as usual, normally translate to random coordinates.

---------------------------------------

## savepoint
*savepoint "<map name>",<x>,<y>{,{<range x>,<range y>,}<char_id>};
## save
*save "<map name>",<x>,<y>{,{<range x>,<range y>,}<char_id>};

These commands save where the invoking character will return to upon clicking
"Return to Save Point", after death and in some other cases. The two versions are
equivalent. They ignore any and all mapflags, and can make a character respawn where
no teleportation is otherwise possible.

The <range x> and <range y> optional values allow for a randomization with the
player's save point. The values will randomly add or subtract from the given <x>
and <y> coordinates.

	savepoint "place",350,75;
	savepoint "place",350,75,2,2; // Randomly save the character between 348,73 and 352,77

---------------------------------------

## heal
*heal <hp>,<sp>{,<char_id>};

This command will heal a set amount of HP and/or SP on the invoking character.

	heal 30000,0; // This will heal 30,000 HP
	heal 0,30000; // This will heal 30,000 SP
	heal 300,300; // This will heal 300 HP and 300 SP

This command just alters the hit points and spell points of the invoking
character and produces no other output whatsoever.

---------------------------------------

## itemheal
*itemheal <hp>,<sp>{,<char_id>};

This command heals relative amounts of HP and/or SP on the invoking character.
Unlike heal, this command is intended for use in item scripts. It applies
potion-related bonuses, such as alchemist ranking, cards, and status changes.
When used inside an NPC script, certain bonuses are omitted.

The command also applies a SP/VIT-related bonus:
	heal = heal * [(100 + STATUS*2) / 100]

Example:
	// If the player has 50 vit and no bonuses, this will heal
	// anything from 200 to 300 HP and 5 SP
	itemheal rand(100,150),5;

---------------------------------------

## percentheal
*percentheal <hp>,<sp>{,<char_id>};

This command will heal the invoking character. It heals the character, but not
by a set value - it adds percent of their maximum HP/SP.

	percentheal 100,0; // This will heal 100% HP
	percentheal 0,100; // This will heal 100% SP
	percentheal 50,50; // This will heal 50% HP and 50% SP

So the amount that this will heal will depend on the total amount of HP or SP
you have maximum. Like 'heal', this will not call up any animations or effects.

---------------------------------------

## recovery
*recovery <type>{,<option>,<revive_flag>{,<map name>}};

This command will revive and fully restore the HP/SP of the selected characters.
It returns 1 upon successful use.

<type> is the target, and determines the <option> parameter:
 0: Player  -> Character ID number
 1: Party   -> Party ID number
 2: Guild   -> Guild ID number
 3: Map     -> Map name (a string)
 4: All     -> None (takes <revive_flag> as option)

If no option is specified, the invoking player's character ID, party ID, guild ID,
or map will be used.

<revive_flag> determines the action:
 1: Revive and heal all players (default)
 2: Heal living players only
 4: Revive dead players only

<map name> can optionally be used to define a single map to execute the command on
for types 1 (party) and 2 (guild).

Examples:
	// Only revive characters in invoking party on map "morocc"
	recovery 1,getcharid(1),4,"morocc";

	// Fully heal (don't revive) all members of invoking character's guild
	recovery 2,getcharid(2),2;

	// Revive and fully heal everyone in map "prontera"
	recovery 3,"prontera";

	// Only revive all dead characters on server
	recovery 4,4;

---------------------------------------

## jobchange
*jobchange <job number>{,<upper flag>,<char_id>};

This command will change the job class of the invoking character.

	jobchange 1; // This would change your player into a Swordman
	jobchange 4002; // This would change your player into a Swordman High

This command does work with numbers, but you can also use job names. The full
list of job names and the numbers they correspond to can be found in
'src/map/script_constants.hpp'.

	// This would change your player into a Swordman
	jobchange Job_Swordman;
	// This would change your player into a Swordman High
	jobchange Job_Swordman_High;

'upper flag' can alternatively be used to specify the type of job one changes
to. For example, jobchange Job_Swordman,1; will change the character to a high
swordsman. The upper values are:
-1 (or when omitted): preserves the current job type.
0: Normal/standard classes
1: High/Advanced classes
2: Baby classes

This command will also set a permanent character-based variable
'jobchange_level' which will contain the job level at the time right before
changing jobs, which can be checked for later in scripts.

---------------------------------------

## jobname
*jobname(<job number>)

This command retrieves the name of the given job using the map_msg entries 550->655.

	mes "[Kid]";
	mes "I never thought I'd met a " + jobname(Class) + " here of all places.";
	close;

---------------------------------------

## eaclass
*eaclass({<job number>,<char_id>})

This commands returns the "eA job-number" corresponding to the given class, and
uses the invoking player's class if none is given. The eA job-number is also a
class number system, but it's one that comes with constants which make it easy
to convert among classes. The command will return -1 if you pass it a job number
which doesn't have an eA job-number equivalent.

	.@eac = eaclass();
	if ((.@eac&EAJ_BASEMASK) == EAJ_SWORDMAN)
		mes "Your base job is Swordman.";
	if (.@eac&EAJL_UPPER)
		mes "You are a rebirth job.";
	if ((.@eac&EAJ_UPPERMASK) == EAJ_SWORDMAN)
		mes "You must be a Swordman, Baby Swordman or High Swordman.";

For more information on the eA Job System, see the docs/ea_job_system.txt file.

---------------------------------------

## roclass
*roclass(<job number>{,<gender>})

Does the opposite of eaclass. That is, given an eA job-number, it returns the
corresponding RO class number. A gender is required because both Bard and Dancers
share the same eA job-number (EAJ_BARDDANCER), and uses the invoking player's
gender if none is given (if no player is attached, male will be used by default).
The command will return -1 if there is no valid class to represent the specified
job (for example, if you try to get the baby version of a Taekwon class).

	.@eac = eaclass();
	//Check if class is already rebirth
	if (.@eac&EAJL_UPPER) {
		mes "You look strong.";
		close;
	}
	.@eac = roclass(.@eac|EAJL_UPPER);
	//Check if class has a rebirth version
	if (.@eac != -1) {
		mes "Bet you can't wait to become a " + jobname(.@eac) + "!";
		close;
	}

---------------------------------------

## changebase
*changebase <job ID number>{,<account ID>};

This command will change a character's appearance to that of the specified job
class. Nothing but appearance will change.

The command will run for the invoking character unless an account ID is given.

	changebase Job_Novice; // Changes player to Novice sprite.
	changebase Class; // Changes player back to default sprite.

---------------------------------------

## classchange
*classchange(<view id>{,"<NPC name>","<flag>"});

This command is very ancient, its origins are clouded in mystery.
It will send a 'display id change' packet to everyone in the immediate area of
the NPC object, which will supposedly make the NPC look like a different sprite,
an NPC sprite ID, or a monster ID. This effect is not stored anywhere and will
not persist (Which is odd, cause it would be relatively easy to make it do so)
and most importantly, will not work at all since this command was broken with
the introduction of advanced classes. The code is written with the assumption
that the lowest sprite IDs are the job sprites and the anything beyond them is
monster and NPC sprites, but since the advanced classes rolled in, they got the
ID numbers on the other end of the number pool where monster sprites float.

As a result it is currently impossible to call this command with a valid view
id. It will do nothing whatsoever if the view ID is below 4047. Getting it to
run will actually just crash the client.

It could be a real gem if it can be gotten to actually do what it's supposed to
do, but this will only happen in a later SVN revision.

Empty <NPC name> means attached NPC.

Target for <flag>:
- bc_area : Sprite is sent to players in the vicinity of the source (default value).
- bc_self : Sprite is sent only to player attached.

---------------------------------------

## changesex
*changesex({<char_id>});

This command will change the gender for the attached character's account. If it
was male, it will become female, if it was female, it will become male. The
change will be written to the character server, the player will receive the
message: "Need disconnection to perform change-sex request..." and the player
will be immediately kicked to the login screen. When they log back in, they will
be the opposite sex.

If there are any Dancer/Gypsy or Bard/Clown characters on the account,
they will also have their skills reset upon 'changesex'.

---------------------------------------

## changecharsex
*changecharsex({<char_id>});

This command will change the gender of the attached character. If it
was male, it will become female, if it was female, it will become male. The
change will be written to the character server, the player will receive the
message: "Need disconnection to perform change-sex request..." and the player
will be immediately kicked to the login screen. When they log back in, they will
be the opposite sex.

If the character being changed is a Dancer/Gypsy or Bard/Clown class type,
the character will also have their skills reset upon 'changecharsex'.

---------------------------------------

## getexp
*getexp <base_exp>,<job_exp>{,<char_id>};

This command will give the invoking character a specified number of base and job
experience points. Used for a quest reward. Negative values won't work.

The EXP values are adjustted by 'quest_exp_rate' config value, VIP bonus, Guild
Tax and EXP boost items such Battle Manual, Bubble Gum, or items that have
SC_EXPBOOST or SC_ITEMBOOST.

	getexp 10000,5000;

---------------------------------------

## getexp2
*getexp2 <base_exp>,<job_exp>{,<char_id>};

This command is safety version of 'set' command for BaseExp and JobExp. If using
'set' while the BaseExp or JobExp value is more than 2,147,483,647 (INT_MAX) will
causing overflow error.

Unlike 'getexp', this command ignores the adjustment factors!

---------------------------------------

## setlook
*setlook <look type>,<look value>{,<char_id>};
## changelook
*changelook <look type>,<look value>{,<char_id>};

'setlook' will alter the look data for the invoking character. It is used
mainly for changing the palette used on hair and clothes: you specify which look
type you want to change, then the palette you want to use. Make sure you specify
a palette number that exists/is usable by the client you use.
'changelook' works the same, but is only client side (it doesn't save the look value).

	// This will change your hair color, so that it uses palette 8, what ever your
	// palette 8 is, your hair will use that color

	setlook LOOK_HAIR_COLOR,8;

	// This will change your clothes color, so they are using palette 1, whatever
	// your palette 1 is, your clothes will then use that set of colors.

	setlook LOOK_CLOTHES_COLOR,1;

Here are the possible look types:

 LOOK_BASE - Base sprite
 LOOK_HAIR - Hairstyle
 LOOK_WEAPON - Weapon
 LOOK_HEAD_BOTTOM - Head bottom
 LOOK_HEAD_TOP - Head top
 LOOK_HEAD_MID - Head mid
 LOOK_HAIR_COLOR - Hair color
 LOOK_CLOTHES_COLOR - Clothes color
 LOOK_SHIELD - Shield
 LOOK_SHOES - Shoes
 LOOK_BODY2 - Body style

Whatever 'shoes' means is anyone's guess, ask Gravity - the client does nothing
with this value. It still wants it from the server though, so it is kept, but
normally doesn't do a thing.

Only the look data for hairstyle, hair color and clothes color are saved to the
char server's database and will persist. Body style will also persist if 'save_body_style'
configuration is enabled in '/conf/battle/client.conf'. The rest freely change as the character
puts on and removes equipment, changes maps, logs in and out and otherwise you
should not expect to set them. In fact, messing with them is generally
hazardous, do it at your own risk, it is not tested what will this actually do -
it won't cause database corruption and probably won't cause a server crash, but
it's easy to crash the client with just about anything unusual.

However, it might be an easy way to quickly check for empty view IDs for
sprites, which is essential for making custom headgear.

Since a lot of people have different palettes for hair and clothes, it's
impossible to tell you what all the color numbers are. If you want a serious
example, there is a Stylist script inside the default rAthena installation that
you can look at: 'npc/custom/stylist.txt'

---------------------------------------

## pushpc
*pushpc <direction>,<cells>;

This command will push the currently attached player to given direction by given
amount of square cells. Direction is the same as used when declaring NPCs, and
can be specified by using one of the DIR_* constants (src/map/script_constants.hpp).

The knock-back is not restricted by items or map flags, only obstacles are taken
into account. If there is not enough space to perform the push (e.g. due to a
wall), the character is pushed only up to the obstacle.

	// pushes the character 5 cells in 3 o'clock direction from its
	// current position.
	pushpc DIR_EAST, 5;

---------------------------------------

## recalculatestat
*recalculatestat;

This command will force a stat recalculation for the attached player.

---------------------------------------

## needed_status_point
*needed_status_point(<type>,<val>{,<char id>});

Returns the number of stat points needed to change the specified stat <type> by <val>.
If <val> is negative, returns the number of stat points that would be needed to
raise the specified stat from (current value - <val>) to current value.

---------------------------------------

## jobcanentermap
*jobcanentermap("<mapname>"{,<JobID>});

Return true if player (decided by job) can enter the map, false otherwise.

For optional 'JobID', see constant of Job_*, or use player's Class, BaseJob,
and BaseClass. If no player is attached, this param must have a value.

See also db/[pre-]re/job_noenter_map.txt

---------------------------------------

## get_revision
*get_revision()

This command will return the SVN revision number that the server is currently
running on.

	if (get_revision() >= 15000)
		mes "Welcome to rAthena!";

---------------------------------------

## get_githash
*get_githash()

This command will return the Git Hash that the server is currently running on.

	mes "Welcome to rAthena! Git Hash: " + get_githash();

---------------------------------------
\\
4,1.- Item-related commands
\\
---------------------------------------

## getitem
*getitem <item id>,<amount>{,<account ID>};
## getitem
*getitem "<item name>",<amount>{,<account ID>};

This command will give an amount of specified items to the invoking character.
If an optional account ID is specified, and the target character is currently
online, items will be created in their inventory instead. If they are not
online, nothing will happen.

In the first and most commonly used version of this command, items are
referred to by their database ID number found inside 'db/(pre-)re/item_db.txt'.

	getitem 502,10 // The person will receive 10 apples
	getitem 617,1  // The person will receive 1 Old Violet Box

This transaction is logged if the log script generated transactions option is
enabled.

You may also create an item by its name in the 'english name' field in the
item database:

	getitem "RED_POTION",10;

Which will do what you'd expect. If it can't find that name in the database,
apples will be created anyway. It is often a VERY GOOD IDEA to use it like this.

This is used in pretty much all NPC scripts that have to do with items and
quite a few item scripts. For more examples check just about any official script.

---------------------------------------

## getitem2
*getitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## getitem2
*getitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## getitem3
*getitem3 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
## getitem3
*getitem3 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};

This command will give an amount of specified items to the invoking character.
If an optional account ID is specified, and the target character is currently
online, items will be created in their inventory instead. If they are not
online, nothing will happen. It works essentially the same as 'getitem' but is
a lot more flexible.

Those parameters that are different from 'getitem' are:

identify    - Whether you want the item to be identified (1) or not (0).
refine      - For how many pluses will it be refined.
              It will not let you refine an item higher than the max refine.
attribute   - Whether the item is broken (1) or not (0).
card1,2,3,4 - If you want a card compound to it, place the card ID number into
              the specific card slot.

Card1-card4 values are also used to store name information for named items, as
well as the elemental property of weapons and armor. You can create a named item
in this manner, however, if you just need a named piece of standard equipment,
it is much easier to the 'getnameditem' function instead.

You will need to keep these values if you want to destroy and then perfectly
recreate a named item, for this see 'getinventorylist'.

If you still want to try creating a named item with this command because
'getnameditem' won't do it for you cause it's too limited, you can do it like
this. Careful, minor magic ahead.

	// First, let's get an ID of a character who's name will be on the item.
	// Only an existing character's name may be there.
	// Let's assume our character is 'Adam' and find his ID.
	.@charid = getcharid(0,"Adam");

	// Now we split the character ID number into two portions with a binary
	// shift operation. If you don't understand what this does, just copy it.
	.@card3 = .@charid & 65535;
	.@card4 = .@charid >> 16;

	// If you're inscribing non-equipment, .@card1 must be 254.
	// Arrows are also not equipment.
	.@card1 = 254;

	// For named equipment, card2 means the Star Crumbs and elemental
	// crystals used to make this equipment. For everything else, it's 0.
	.@card2 = 0;

	// Now, let's give the character who invoked the script some
	// Adam's Apples:
	getitem2 512,1,1,0,0,.@card1,.@card2,.@card3,.@card4;

This wasn't tested with all possible items, so I can't give any promises,
experiment first before relying on it.

To create equipment, continue this example it like this:

	// We've already have card3 and card4 loaded with correct
	// values so we'll just set up card1 and card2 with data
	// for an Ice Stiletto.

	// If you're inscribing equipment, .@card1 must be 255.
	.@card1 = 255;

	// That's the number of star crumbs in a weapon.
	.@sc = 2;

	// That's the number of elemental property of the weapon.
	.@ele = 1;

	// And that's the wacky formula that makes them into
	// a single number.
	.@card2 = .@ele+((.@sc*5)<<8);

	// That will make us an Adam's +2 VVS Ice Stiletto:
	getitem2 1216,1,1,2,0,.@card1,.@card2,.@card3,.@card4;

Experiment with the number of star crumbs - I'm not certain just how much will
work most and what it depends on. The valid element numbers are:

 1 - Ice, 2 - Earth 3 - Fire 4 - Wind.

You can, apparently, even create duplicates of the same pet egg with this
command, creating a pet which is the same, but simultaneously exists in two
eggs, and may hatch from either, although, I'm not sure what kind of a mess will
this really cause.

'getitem3' is advance version of 'getitem2' that also use Item Random Option as additional values.
<RandomIDArray>    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
<RandomValueArray> : Array variable of item random option's value.
<RandomParamArray> : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
	// +9 Crimson Dagger [2]
	setarray .@OptID[0],RDMOPT_WEAPON_ATTR_TELEKINESIS;
	setarray .@OptVal[0],0;
	setarray .@OptParam[0],0;
	getitem3 28705,1,1,9,0,0,0,0,0,.@OptID,.@OptVal,.@OptParam;

---------------------------------------

## getitembound
*getitembound <item id>,<amount>,<bound type>{,<account ID>};
## getitembound
*getitembound "<item name>",<amount>,<bound type>{,<account ID>};

This command behaves identically to 'getitem', but the items created will be
bound to the target character as specified by the bound type. All items created
in this manner cannot be dropped, sold, vended, auctioned, or mailed, and in
some cases cannot be traded or stored.

Valid bound types are:
 Bound_Account : Account Bound item
 Bound_Guild   : Guild Bound item
 Bound_Party   : Party Bound item
 Bound_Char    : Character Bound item

---------------------------------------

## getitembound2
*getitembound2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>{,<account ID>};
## getitembound2
*getitembound2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>{,<account ID>};
## getitembound3
*getitembound3 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
## getitembound3
*getitembound3 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<bound type>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};

This command behaves identically to 'getitem2', but the items created will be
bound to the target character as specified by the bound type. All items created
in this manner cannot be dropped, sold, vended, auctioned, or mailed, and in
some cases cannot be traded or stored.

For a list of bound types see 'getitembound'.

'getitembound3' is advance version of 'getitembound2' that also use Item Random Option as additional values.
<RandomIDArray>    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
<RandomValueArray> : Array variable of item random option's value.
<RandomParamArray> : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
	// +9 Crimson Dagger [2]
	setarray .@OptID[0],RDMOPT_WEAPON_ATTR_TELEKINESIS;
	setarray .@OptVal[0],0;
	setarray .@OptParam[0],0;
	getitembound3 28705,1,1,9,0,0,0,0,0,BOUND_CHAR,.@OptID,.@OptVal,.@OptParam;

---------------------------------------

## getnameditem
*getnameditem <item id>,<character name|character ID>;
## getnameditem
*getnameditem "<item name>",<character name|character ID>;

Create an item signed with the given character's name.

The command returns 1 when the item is created successfully, or 0 if it fails.
Failure occurs when:
- There is no player attached.
- Item name or ID is not valid.
- The given character ID/name is offline.

Example:

//This will give the currently attached player a Aaron's Apple (if Aaron is online).
	getnameditem "Apple","Aaron";

//Self-explanatory (I hope).
	if (getnameitem("Apple","Aaron")) {
		mes "You now have a Aaron's Apple!";
	}

---------------------------------------

## rentitem
*rentitem <item id>,<time>{,<account_id>};
## rentitem
*rentitem "<item name>",<time>{,<account_id>};

Creates a rental item in the attached character's inventory. The item will expire
in <time> seconds and be automatically deleted. When receiving a rental item,
the character will receive a message in their chat window. The character will
also receive warning messages in their chat window before the item disappears.

When rentals expire it will call the OnUnequip Script of the item. This can be used
for special cases such as removing a status change or resetting a variable or state
of the player.

This command can not be used to rent stackable items. Rental items cannot be
dropped, traded, sold to NPCs, or placed in guild storage. (i.e. trade mask 75)
Note: 'delitem' in an NPC script can still remove rental items.

---------------------------------------

## rentitem2
*rentitem2 <item id>,<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account_id>};
## rentitem2
*rentitem2 "<item name>",<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account_id>};
## rentitem3
*rentitem3 <item id>,<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account_id>};
## rentitem3
*rentitem3 "<item name>",<time>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account_id>};

Creates a rental item in the attached character's inventory. The item will expire
in <time> seconds and be automatically deleted. See 'rentitem' for further details.

See 'getitem2' for an explanation of the expanded parameters.

'rentitem3' is advance version of 'rentitem2' that also use Item Random Option as additional values.
<RandomIDArray>    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
<RandomValueArray> : Array variable of item random option's value.
<RandomParamArray> : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
	// +9 Crimson Dagger [2]
	setarray .@OptID[0],RDMOPT_WEAPON_ATTR_TELEKINESIS;
	setarray .@OptVal[0],0;
	setarray .@OptParam[0],0;
	rentitem3 28705,(24*60*60),1,9,0,0,0,0,0,.@OptID,.@OptVal,.@OptParam;

---------------------------------------

## makeitem
*makeitem <item id>,<amount>,"<map name>",<X>,<Y>;
## makeitem
*makeitem "<item name>",<amount>,"<map name>",<X>,<Y>;

This command will create an item on the specified cell of a map.

As with any dropped items, the items created with this command will disappear after
a period of time. Using an amount greater than 1 will create a single stack of the
given amount, not multiple stacks of 1.

Like 'getitem', it also accepts an 'english name' field from the database and creates
Apples if the name isn't found.
If the map name is given as "this", the map the invoking character is on will be used.

---------------------------------------

## makeitem2
*makeitem2 <item id>,<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>;
## makeitem2
*makeitem2 "<item name>",<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>;
## makeitem3
*makeitem3 <item id>,<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>;
## makeitem3
*makeitem3 "<item name>",<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>;

This command will create an item on the specified cell of a map. See 'makeitem' for
further details.

See 'getitem2' for an explanation of the expanded parameters.

'makeitem3' is advance version of 'makeitem2' that also use Item Random Option as additional values.
<RandomIDArray>    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
<RandomValueArray> : Array variable of item random option's value.
<RandomParamArray> : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
	// 0.5% chance to get +0 Valkyrie Shield [1]
	// with Neutral Resistance +10% and 5% damage reduction from Demi-Human or Player
	// when Valkyrie Randgris killed
	OnNPCKillEvent:
		if (killedrid == 1751 && rand(0,1000) > 950) { // Valkyrie Randgris
			getmapxy(.@map$,.@x,.@y,BL_PC);
			setarray .@OptID[0],RDMOPT_ATTR_TOLERACE_NOTHING,RDMOPT_RACE_TOLERACE_HUMAN;
			setarray .@OptVal[0],10,5;
			setarray .@OptParam[0],0;
			makeitem3 2115,1,.@map$,.@x,.@y,0,0,0,0,0,0,0,.@OptID,.@OptVal,.@OptParam;
		}
		end;

---------------------------------------

## cleanarea
*cleanarea "<map name>",<x1>,<y1>,<x2>,<y2>;
## cleanmap
*cleanmap "<map name>";

These commands will clear all items lying on the ground on the specified map, either
within the x1/y1-x2/y2 rectangle or across the entire map.

---------------------------------------

## searchitem
*searchitem <array name>,"<item name>";

This command will fill the given array with the ID of items whose name matches
the given one. It returns the number of items found. For performance reasons,
the results array is limited to 10 items.

	mes "What item are you looking for?";
	input .@name$;
	.@qty = searchitem(.@matches[0],.@name$);
	mes "I found " + .@qty + " items:";
	for (.@i = 0; .@i < .@qty; .@i++)
		// Display name (eg: "Apple[0]")
		mes getitemname(.@matches[.@i]) + "[" + getitemslots(.@matches[.@i]) + "]";

---------------------------------------

## delitem
*delitem <item id>,<amount>{,<account ID>};
## delitem
*delitem "<item name>",<amount>{,<account ID>};

This command will remove a specified amount of items from the invoking/target character.
Like all the item commands, it uses the item ID found inside 'db/(pre-)re/item_db.txt'.

    delitem 502,10; // The person will lose 10 apples
    delitem 617,1;  // The person will lose 1 Old Violet Box

It is always a good idea to check if the player actually has the items before you delete them.
If you try to delete more items that the player has, the player will lose the ones he/she has
and the script will terminate with an error.

Like 'getitem', this command will also accept an 'english name' field from the
database. If the name is not found, nothing will be deleted.

---------------------------------------

## cartdelitem
*cartdelitem <item id>,<amount>{,<account ID>};
## cartdelitem
*cartdelitem "<item name>",<amount>{,<account ID>};
## storagedelitem
*storagedelitem <item id>,<amount>{,<account ID>};
## storagedelitem
*storagedelitem "<item name>",<amount>{,<account ID>};
## guildstoragedelitem
*guildstoragedelitem <item id>,<amount>{,<account ID>};
## guildstoragedelitem
*guildstoragedelitem "<item name>",<amount>{,<account ID>};

This command behaves identically to 'delitem', but deletes items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartdelitem' will return -1.
If player is not in a guild or storage is open, 'guildstoragedelitem' will return -1.

---------------------------------------

## delitem2
*delitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## delitem2
*delitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## delitem3
*delitem3 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
## delitem3
*delitem3 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};

This command will remove a specified amount of items from the invoking/target character.
See 'getitem2' for an explanation of the expanded parameters.

'delitem3' is advance version of 'delitem2' that also use Item Random Option as criteria.
<RandomIDArray>    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
<RandomValueArray> : Array variable of item random option's value.
<RandomParamArray> : Array variable of item random option's param.

---------------------------------------

## cartdelitem2
*cartdelitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## cartdelitem2
*cartdelitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## storagedelitem2
*storagedelitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## storagedelitem2
*storagedelitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## guildstoragedelitem2
*guildstoragedelitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
## guildstoragedelitem2
*guildstoragedelitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};

This command behaves identically to 'delitem2', but deletes items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartdelitem2' will return -1.
If player is not in a guild or storage is open, 'guildstoragedelitem2' will return -1.

---------------------------------------

## countitem
*countitem(<item id>{,<accountID>})
## countitem
*countitem("<item name>"{,<accountID>})

This function will return the number of items for the specified item ID that the
invoking character has in the inventory.

	mes "[Item Checker]";
	mes "Hmmm, it seems you have " + countitem(502) + " apples";
	close;

Like 'getitem', this function will also accept an 'english name' from the
database as an argument.

If you want to state the number at the end of a sentence, you can do it by
adding up strings:

	mes "[Item Checker]";
	mes "Hmmm, the total number of apples you are holding is " + countitem("APPLE");
	close;

---------------------------------------

## cartcountitem
*cartcountitem(<item id>{,<accountID>})
## cartcountitem
*cartcountitem("<item name>"{,<accountID>})
## storagecountitem
*storagecountitem(<item id>{,<accountID>})
## storagecountitem
*storagecountitem("<item name>"{,<accountID>})
## guildstoragecountitem
*guildstoragecountitem(<nameID>{,<accountID>})
## guildstoragecountitem
*guildstoragecountitem("<item name>"{,<accountID>})

This command behaves identically to 'countitem', but counts items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartcountitem' will return -1.
If player is not in a guild or storage is open, 'guildstoragecountitem' will return -1.

---------------------------------------

## countitem2
*countitem2(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
## countitem2
*countitem2("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
## countitem3
*countitem3(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<accountID>})
## countitem3
*countitem3("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<accountID>})

Expanded version of 'countitem' function, used for created/carded/forged items.

This function will return the number of items for the specified item ID and
other parameters that the invoking character has in the inventory.
See 'getitem2' for an explanation of the expanded parameters.

'countitem3' is advance version of 'countitem2' that also use Item Random Option as criteria.
<RandomIDArray>    : Array variable of ID for item random option, see db/[pre-]re/item_randomopt_db.txt
<RandomValueArray> : Array variable of item random option's value.
<RandomParamArray> : Array variable of item random option's param.

---------------------------------------

## cartcountitem2
*cartcountitem2(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
## cartcountitem2
*cartcountitem2("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
## storagecountitem2
*storagecountitem2(<item id>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
## storagecountitem2
*storagecountitem2("<item name>",<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<accountID>})
## guildstoragecountitem2
*guildstoragecountitem2(<nameID>,<Identified>,<Refine>,<Attribute>,<Card0>,<Card1>,<Card2>,<Card3>{,<accountID>})
## guildstoragecountitem2
*guildstoragecountitem2("<item name>",<Identified>,<Refine>,<Attribute>,<Card0>,<Card1>,<Card2>,<Card3>{,<accountID>})

This command behaves identically to 'countitem2', but counts items from the player's
cart, storage, or guild storage.

If no cart is mounted, 'cartcountitem2' will return -1.
If player is not in a guild or storage is open, 'guildstoragecountitem2' will return -1.

---------------------------------------

## countbound
*countbound({<bound type>{,<char_id>}})

This function will return the number of bounded items in the character's
inventory, and sets an array @bound_items[] containing all item IDs of the
counted items. If a bound type is specified, only those items will be counted.

For a list of bound types see 'getitembound'.

Example:
	mes "You currently have " + countbound() + " bounded items.";
	next;
	mes "The list of bounded items include:";
	for(.@i = 0; .@i < getarraysize(@bound_items); .@i++)
		mes getitemname(@bound_items[.@i]);
	close;

---------------------------------------

## groupranditem
*groupranditem <group id>{,<sub_group>};

Returns the item_id of a random item picked from the group specified. The
different groups and their group number are specified in 'db/(pre-)re/item_group_db.txt'.

When used in conjunction with other functions, you can get a random item. For
example, for a random pet lure:

getitem groupranditem(IG_Taming),1;

'sub_group' is used to get the available random items of item group from specified random
group. 0 for 'must' item group, and random item group is 1 until 5 (MAX_ITEMGROUP_RANDGROUP+1).

More info, see doc/item_group.txt.

---------------------------------------

## getrandgroupitem
*getrandgroupitem <group_id>{,<quantity>{,<sub_group>{,<identify>{,<char_id>}}}};

Similar to the above example, this command allows players to obtain the specified
quantity of a random item from the group "<group id>". The different groups and
their group number are specified in db/(pre-)re/item_group_db.txt

If 'quantity' is not defined or 0, it will uses defined amount from Item Group list.

If 'sub_group' is not defined the value will be 1 (since random group is 1 ~ 5, and 0 is
'must' item group).

For item with type IT_WEAPON, IT_ARMOR, IT_PETARMOR, and IT_SHADOWGEAR will be given
as unidentified item (as defined by itemdb_isidentified in src/map/itemdb.cpp) except
if 'identify' is defined with value 1.

More info, see doc/item_group.txt.

---------------------------------------

## getgroupitem
*getgroupitem <group_id>{,<identify>{,<char_id>}};

Gives item(s) to the attached player based on item group contents.
This is not working like 'getrandgroupitem' which only give 1 item for specified
item group & sub_group.

For item with type IT_WEAPON, IT_ARMOR, IT_PETARMOR, and IT_SHADOWGEAR will be given
as unidentified item (as defined by itemdb_isidentified in src/map/itemdb.cpp) except
if 'identify' is defined with value 1.

More info, see doc/item_group.txt.

---------------------------------------

## enable_items
*enable_items;
## disable_items
*disable_items;

These commands toggle the ability to change equipment while interacting with
an NPC. To avoid possible exploits, the commands affect the particular script
instance only. Note that if a different script also calls enable_items, it
will override the last call (so you may want to call this command at the start
of your script without assuming it is still in effect).

The default setting, 'item_enabled_npc', is defined in 'conf/battle/items.conf'.

---------------------------------------

## itemskill
*itemskill <skill id>,<skill level>{,<keep requirement>};
## itemskill
*itemskill "<skill name>",<skill level>{,<keep requirement>};

This command is meant for item scripts to replicate single-use skills in usable
items. It will not work properly if there is a visible dialog window or menu.
If the skill is self or auto-targeting, it will be used immediately; otherwise a
target cursor is shown.

If <keep requirement> parameter is set to true, the skill's requirements will be checked.
By default, the requirements for item skills are not checked, and therefore the default value is false.

// When Anodyne is used, it will cast Endure (8), Level 1, as if the actual
// skill has been used from skill tree.
605,Anodyne,Anodyne,11,2000,0,100,,,,,10477567,2,,,,,{ itemskill 8,1; },{}

// When Sienna_Execrate_Scroll_1_5 is used, it will cast Sienna Execrate Level 5 and consume 2 Red_Gemstones.
23194,Sienna_Execrate_Scroll_1_5,Level 5 Sienna Execrate,11,10,,10,,,,,0xFFFFFFFF,63,2,,,,,,{ itemskill "WL_SIENNAEXECRATE",5,true; },{},{}

---------------------------------------

## consumeitem
*consumeitem <item id>{,<char_id>};
## consumeitem
*consumeitem "<item name>"{,<char_id>};

This command will run the item script of the specified item on the invoking
character. The character does not need to possess the item, and the item will
not be deleted. While this command is intended for usable items, it will run
for any item type.

This command does not currently work with the 'itemskill' script command.

---------------------------------------

## produce
*produce <item level>;

This command will open a crafting window on the client connected to the invoking
character. The 'item level' is a number which determines what kind of a crafting
window will pop-up.

You can see the full list of such item levels in 'db/produce_db.txt' which determines
what can actually be produced. The window will not be empty only if the invoking
character can actually produce the items of that type and has the appropriate raw
materials in their inventory.

The success rate to produce the item is the same as the success rate of the skill
associated with the item level. If there is no skill id, the success rate will be 50%.

Valid item levels are:

 1   - Level 1 Weapons
 2   - Level 2 Weapons
 3   - Level 3 Weapons
 21  - Blacksmith's Stones and Metals
 22  - Alchemist's Potions, Holy Water, Assassin Cross's Deadly Poison
 23  - Elemental Converters

---------------------------------------

## cooking
*cooking <dish level>;

This command will open a produce window on the client connected to the invoking
character. The 'dish level' is the number which determines what kind of dish
level you can produce. You can see the full list of dishes that can be produced in
'db/produce_db.txt'.

The window will be shown empty if the invoking character does not have enough of
the required incredients to cook a dish.

Valid dish levels are:

11 - Level 1 Dish
12 - Level 2 Dish
13 - Level 3 Dish
14 - Level 4 Dish
15 - Level 5 Dish
16 - Level 6 Dish
17 - Level 7 Dish
18 - Level 8 Dish
19 - Level 9 Dish
20 - Level 10 Dish

Although it's required to set a dish level, it doesn't matter if you set it to 1
and you want to cook a level 10 dish, as long as you got the required incredients
to cook the dish the command works.

---------------------------------------

## makerune
*makerune <% success bonus>{,<char_id>};

This command will open a rune crafting window on the client connected to the
invoking character. Since this command is officially used in rune ores, a bonus
success rate must be specified (which adds to the base formula).

You can see the full list of runes that can be produced in 'db/produce_db.txt'.
The window will not be empty only if the invoking character can actually produce
a rune and has the appropriate raw materials in their inventory.

---------------------------------------

## successremovecards
*successremovecards <equipment slot>;

This command will remove all cards of the cards slots defined by item_db.txt
from the item found in the specified equipment slot of the invoking character,
create new card items and give them to the character.
If any cards were removed in this manner, it will also show a success effect.

---------------------------------------

## failedremovecards
*failedremovecards <equipment slot>,<type>;

This command will remove all cards from the item found in the specified
equipment slot of the invoking character. 'type' determines what happens to the
item and the cards:

 0 - will destroy both the item and the cards.
 1 - will keep the item, but destroy the cards.
 2 - will keep the cards, but destroy the item.

Whatever the type is, it will also show a failure effect on screen.

---------------------------------------

## repair
*repair <broken item number>{,<char_id>};

This command repairs a broken piece of equipment, using the same list of broken
items as available through 'getbrokenid'.

---------------------------------------

## repairall
*repairall {<char_id>};

This command repairs all broken equipment in the attached player's inventory.
A repair effect will be shown if any items are repaired, else the command will
end silently.

---------------------------------------

## successrefitem
*successrefitem <equipment slot>{,<count>{,<char_id>}};

This command will refine an item in the specified equipment slot of the invoking
character by +1, or a count if given. For a list of equipment slots see 'getequipid'.
This command will also display a 'refine success' effect on the character and put
appropriate messages into their chat window. It will also give the character fame
points if a weapon reached +10 this way, even though these will only take effect for
blacksmith who will later forge a weapon.

---------------------------------------

## failedrefitem
*failedrefitem <equipment slot>{,<char_id>};

This command will fail to refine an item in the specified equipment slot of the
invoking character. The item will be destroyed. This will also display a 'refine
failure' effect on the character and put appropriate messages into their chat
window.

---------------------------------------

## downrefitem
*downrefitem <equipment slot>{,<count>{,<char_id>}};

This command will downgrade an item in the specified equipment slot of the invoking
character by -1, or a count if given. For a list of equipment slots see 'getequipid'.
This command will also display a 'refine failure' effect on the character and put
appropriate messages into their chat window.

---------------------------------------

## unequip
*unequip <equipment slot>{,<char_id>};

This command will unequip whatever is currently equipped in the invoking
character's specified equipment slot. For a full list of possible equipment
slots see 'getequipid'.

If an item occupies several equipment slots, it will get unequipped from all of
them.

---------------------------------------

## delequip
*delequip <equipment slot>{,<char_id>};

This command will destroy whatever is currently equipped in the invoking
character's specified equipment slot. For a full list of possible equipment
slots see 'getequipid'.

This command will return 1 if an item was deleted and 0 otherwise.

---------------------------------------

## breakequip
*breakequip <equipment slot>{,<char_id>};

This command will break and unequip whatever is currently equipped in the
invoking character's specified equipment slot. For a full list of possible
equipment slots see 'getequipid'.

This command will return 1 if an item was broken and 0 otherwise.

---------------------------------------

## clearitem
*clearitem {<char_id>};

This command will destroy all items the invoking character has in their
inventory (including equipped items). It will not affect anything else, like
storage or cart.

---------------------------------------

## equip
*equip <item id>{,<char_id>};
## autoequip
*autoequip <item id>,<option>;

These commands are to equip a equipment on the attached character.
The equip function will equip the item ID given when the player has
this item in his/her inventory, while the autoequip function will
equip the given item ID when this is looted. The option parameter of
the autoequip is 1 or 0, 1 to turn it on, and 0 to turn it off.

Examples:

//This will equip a 1104 (falchion) on the character if this is in the inventory.
	equip 1104;

//The invoked character will now automatically equip a falchion when it's looted.
	autoequip 1104,1;

//The invoked character will no longer automatically equip a falchion.
	autoequip 1104,0;

---------------------------------------

## buyingstore
*buyingstore <slots>;

Invokes buying store preparation window like the skill 'Open Buying Store',
without the item requirement. Amount of slots is limited by the server to
a maximum of 5 slots by default.

Example:
	// Gives the player opportunity to buy 4 different kinds of items.
	buyingstore 4;

---------------------------------------

## searchstores
*searchstores <uses>,<effect>;

Invokes the store search window, which allows to search for both vending
and buying stores. Parameter uses indicates, how many searches can be
started, before the window has to be reopened. Effect value affects,
what happens, when a result item is double-clicked and can be one of the
following:

	0 = Shows the store's position on the mini-map and highlights the
	    shop sign with yellow color, when the store is on same map
	    as the invoking player.
	1 = Directly opens the shop, regardless of distance.

Example:
	// Item Universal_Catalog_Gold (10 uses, effect: open shop)
	searchstores 10,1;

---------------------------------------

## enable_command
*enable_command;
## disable_command
*disable_command;

These commands toggle the ability to use atcommand while interacting with an NPC.

The default setting, 'atcommand_disable_npc', is defined in 'conf/battle/gm.conf'.

---------------------------------------
//
4,1.- End of item-related commands
//
---------------------------------------

## openstorage
*openstorage;

This will open character's Kafra storage window on the client connected to the
invoking character. It can be used from any kind of NPC or item script, not just
limited to Kafra Staff.

The storage window opens regardless of whether there are open NPC dialogs or
not, but it is preferred to close the dialog before displaying the storage
window, to avoid any disruption when both windows overlap.

    mes "Close this window to open your storage.";
    close2;
    openstorage;
    end;

---------------------------------------

## openstorage2
*openstorage2 <storage_id>,<mode>{,<account_id>};

Just like the 'openstorage' command, except this command can open additional storages
by the specified <storage_id>. For <storage_id>, please read the conf/inter_server.yml
for storage groups.

Values for <mode> are:
	STOR_MODE_NONE : Player only can read the storage entries.
	STOR_MODE_GET  : Player can get items from the storage.
	STOR_MODE_PUT  : Player can put items in the storage.

Example:
	if (vip_status(VIP_STATUS_ACTIVE)) {
		mes "I will open your Premium storage.";
		mes "Thank you for using our service.";
		close2;
		openstorage2 1,STOR_MODE_GET|STOR_MODE_PUT;
	} else {
		mes "Sorry, your Premium status is expired.";
		mes "Storage will be opened but you can't put any item into it.";
		close2;
		openstorage2 1,STOR_MODE_GET;
	}
	end;

---------------------------------------

## openmail
*openmail({<char_id>});

This will open a character's Mail window on the client connected to the
invoking character.

	mes "Close this window to open your mail inbox.";
	close2;
	openmail;
	end;

---------------------------------------

## mail
*mail <destination id>,"<sender name>","<title>","<body>"{,<zeny>{,<item id array>,<item amount array>{,<item card0 array>{,<item card1 array>{,<item card2 array>{,<item card3 array>
		{,<random option id0 array>, <random option value0 array>, <random option paramter0 array>{,<random option id1 array>, <random option value1 array>, <random option paramter1 array>
		{,<random option id2 array>, <random option value2 array>, <random option paramter2 array>{,<random option id3 array>, <random option value3 array>, <random option paramter3 array>
		{,<random option id4 array>, <random option value4 array>, <random option paramter4 array>}}}}}}};

This command will send mail to the <destination id> which is a character ID.
A <sender name> can be specified but does not have to be from the direct creator
of the mail and is limited to NAME_LENGTH (24) characters. Mail <title> is limited
to MAIL_TITLE_LENGTH (40) characters. Mail <body> is limited to MAIL_BODY_LENGTH
(200) characters for PACKETVER < 20150513 or 500 characters for later clients.

Optional <zeny> and item data can be added to the mail as well. PACKETVER < 20150513
is limited to 1 item while later clients are limited to MAIL_MAX_ITEM (5).

The <item id array>, <item amount array>, <item card0 array>, <item card1 array>,
<item card2 array>, and <item card3 array> should all be integer arrays.

For random options there can be 5 arrays in pairs of 3 (ids, values, parameters) right after the cards.
All of these arrays shall be integer arrays as well.

Example of sending mail with zeny:
	.@charid = getcharid(0);
	.@sender$ = "Poring";
	.@title$ = "Welcome";
	.@body$ = "Hi! I'm a simple Poring from the Prontera fields! Welcome to Ragnarok!";
	.@zeny = 5000;
	mail .@charid, .@sender$, .@title$, .@body$, .@zeny;

Example of sending mail with items:
	.@charid = getcharid(0);
	.@sender$ = "Angeling";
	.@title$ = "Welcome";
	.@body$ = "Hi! I'm a simple Angeling from the Prontera fields! Welcome to Ragnarok!";
	.@zeny = 0;
	setarray .@mailitem[0], 504, 505, 2220, 1214; // White Potion, Blue Potion, Hat, Dagger
	setarray .@mailamount[0], 10, 5, 1, 1; // 10 White Potions, 5 Blue Potions, 1 Hat, 1 Dagger
	setarray .@mailcard0[0], 0, 0, 4198, 4092; // Attach Maya Purple Card to the Hat, Attach Skeleton Worker Card to Dagger
	setarray .@mailcard1[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
	setarray .@mailcard2[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
	mail .@charid, .@sender$, .@title$, .@body$, .@zeny, .@mailitem, .@mailamount, .@mailcard0, .@mailcard1, .@mailcard2;
	
Example of sending mail with items and random options:
	.@charid = getcharid(0);
	.@sender$ = "Angeling";
	.@title$ = "Welcome";
	.@body$ = "Hi! I'm a simple Angeling from the Prontera fields! Welcome to Ragnarok!";
	.@zeny = 0;
	setarray .@mailitem[0], 504, 505, 2220, 1214; // White Potion, Blue Potion, Hat, Dagger
	setarray .@mailamount[0], 10, 5, 1, 1; // 10 White Potions, 5 Blue Potions, 1 Hat, 1 Dagger
	setarray .@mailcard0[0], 0, 0, 4198, 4092; // Attach Maya Purple Card to the Hat, Attach Skeleton Worker Card to Dagger
	setarray .@mailcard1[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
	setarray .@mailcard2[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
	setarray .@mailcard3[0], 0, 0, 0, 0; // Empty last slot
	setarray .@mailrndopt_id0[0], 0, 0, 0, 0, RDMOPT_VAR_MAXHPAMOUNT; // Enchant the Dagger with increased HP option
	setarray .@mailrndopt_val0[0], 0, 0, 0, 0, 1000; // Enchant the Dagger with increased HP option by 1000 points
	setarray .@mailrndopt_prm0[0], 0, 0, 0, 0, 0; // Enchant the Dagger with increased HP option - does not need any parameter
	mail .@charid, .@sender$, .@title$, .@body$, .@zeny, .@mailitem, .@mailamount, .@mailcard0, .@mailcard1, .@mailcard2, .@mailcard3, .@mailrndopt_id0, .@mailrndopt_val0, .@mailrndopt_prm0;

---------------------------------------

## openauction
*openauction({<char_id>});

This will open the Auction window on the client connected to the invoking character.

	mes "Close this window to open the Auction window.";
	close2;
	openauction;
	end;

---------------------------------------
\\
4,2.- Guild-related commands
\\
---------------------------------------

## guildopenstorage
*guildopenstorage()

This function works the same as 'openstorage' but will open a guild storage
window instead for the guild storage of the guild the invoking character belongs
to.

Return values:
 GSTORAGE_OPEN - Successfully opened.
 GSTORAGE_STORAGE_ALREADY_OPEN - Player storage is already open.
 GSTORAGE_ALREADY_OPEN - Guild storage is already open.
 GSTORAGE_NO_GUILD - Player is not in a guild.
 GSTORAGE_NO_STORAGE - Guild hasn't invested in the Guild Storage Expansion skill (only if OFFICIAL_GUILD_STORAGE is enabled).
 GSTORAGE_NO_PERMISSION - Player doesn't have permission to use the guild storage.

---------------------------------------

## guildopenstorage_log
*guildopenstorage_log({<char id>})

Opens the guild storage log window for the attached character or the given character id.

Possible return values:
GUILDSTORAGE_LOG_FINAL_SUCCESS	Window was opened successfully.
GUILDSTORAGE_LOG_EMPTY			Window was not opened, because no entries exist.
GUILDSTORAGE_LOG_FAILED			Some database error occurred.

---------------------------------------

## guild_has_permission
*guild_has_permission(<permission>{,<char id>})

Checks if the attached player or the player with the given character id has the given permission(s).
Permission can be a bitmask and allows to use multiple values at the same time.
Returns true if the player has all of the given permissions or false if the player does at least
miss one of the given permissions or is not in a guild at all.

Available permissions are:
GUILD_PERM_INVITE	If a player is allowed to invite other players.
GUILD_PERM_EXPEL	If a player is allowed to expel other guild members.
GUILD_PERM_STORAGE	If a player is allowed to access the guild storage.
GUILD_PERM_ALL		A combination of all permissions above.

---------------------------------------

## guildchangegm
*guildchangegm(<guild id>,<new master's name>)

This function will change the Guild Master of a guild. The ID is the guild's
id, and the new guild master's name must be passed.

Returns 1 on success, 0 otherwise.

---------------------------------------

## guildgetexp
*guildgetexp <amount>;

This will give the specified amount of guild experience points to the guild the
invoking character belongs to. It will silently fail if they do not belong to
any guild.

---------------------------------------

## guildskill
*guildskill <skill id>,<level>
## guildskill
*guildskill "<skill name>",<level>

This command will bump up the specified guild skill by the specified number of
levels. This refers to the invoking character and will only work if the invoking
character is a member of a guild AND its guild master, otherwise no failure
message will be given and no error will occur, but nothing will happen - same
about the guild skill trying to exceed the possible maximum. The full list of
guild skills is available in 'db/(pre-)re/skill_db.txt', these are all the GD_ skills at
the end.

// This would give your character's guild one level of Approval (GD_APPROVAL ID
// 10000). Notice that if you try to add two levels of Approval, or add
// Approval when the guild already has it, it will only have one level of
// Approval afterwards.
	guildskill 10000,1,0;

You might want to make a quest for getting a certain guild skill, make it hard
enough that all the guild needs to help or something. Doing this for the Glory
of the Guild skill, which allows your guild to use an emblem, is a good idea for
a fun quest.

---------------------------------------
//
4,2 End of guild-related commands.
//
---------------------------------------

## resetlvl
*resetlvl <action type>{,<char_id>};

This is a character reset command, meant mostly for rebirth script supporting
Advanced jobs, which will reset the invoking character's stats and level
depending on the action type given. Valid action types are:

 1 - Base level 1, Job level 1, 0 skill points, 0 base exp, 0 job exp, wipes the
     status effects (only the ones settable by 'setoption'), sets all stats to 1.
     If the new job is 'Novice High', give 100 status points, give First Aid and
     Play Dead skills.
 2 - Base level 1, Job level 1, 0 skill points, 0 base exp, 0 job exp.
     Skills and attribute values are not altered.
 3 - Base level 1, base exp 0. Nothing else is changed.
 4 - Job level 1, job exp 0. Nothing else is changed.

In all cases everything the character has on will be unequipped.

Even though it doesn't return a value, it is used as a function in the official
rebirth scripts. Ask AppleGirl why.

---------------------------------------

## resetstatus
*resetstatus({<char_id>});

This is a character reset command, which will reset the stats on the invoking
character and give back all the stat points used to raise them previously.
Nothing will happen to any other numbers about the character.

Used in reset NPC's (duh!)

---------------------------------------

## resetskill
*resetskill({<char_id>});

This command takes off all the skill points on the invoking character, so they
only have Basic Skill blanked out (lvl 0) left, and returns the points for them
to spend again. Nothing else will change but the skills. Quest skills will also
reset if 'quest_skill_reset' option is set to Yes in 'battle_athena.conf'. If
the 'quest_skill_learn' option is set in there, the points in the quest skills
will also count towards the total.

Used in reset NPC's (duh!)

---------------------------------------

## sc_start
*sc_start <effect type>,<ticks>,<value 1>{,<rate>,<flag>{,<GID>}};
## sc_start2
*sc_start2 <effect type>,<ticks>,<value 1>,<value 2>{,<rate>,<flag>{,<GID>}};
## sc_start4
*sc_start4 <effect type>,<ticks>,<value 1>,<value 2>,<value 3>,<value 4>{,<rate>,<flag>{,<GID>}};
## sc_end
*sc_end <effect type>{,<GID>};
## sc_end_class
*sc_end_class {<char_id>{,<job_id>}};

These commands will bestow a status effect on a character.

The <effect type> determines which status is invoked. This can be either a number
or constant, with the common statuses (mostly negative) found in 'src/map/script_constants.hpp'
with the 'SC_' prefix. A full list is located in 'src/map/status.hpp', though
they are not currently documented.

The duration of the status is given in <ticks>, or milleseconds.
Use INFINITE_TICK for infinite duration.

Certain status changes take an additional parameter <value 1>, which typically
modifies player stats by the given number or percentage. This differs for each
status, and is sometimes zero.

Optional value <rate> is the chance that the status will be invoked (100 = 1%).
This is used primarily in item scripts. When used in an NPC script, a flag MUST
be defined for the rate to work.

Optional value <flag> is how the status change start will be handled (a bitmask).
 SCSTART_NOAVOID   : Status change cannot be avoided.
 SCSTART_NOTICKDEF : Tick cannot be reduced by stats (default).
 SCSTART_LOADED    : sc_data loaded, so no value will be altered.
 SCSTART_NORATEDEF : Rate cannot be reduced.
 SCSTART_NOICON    : Status icon won't be sent to client

If a <GID> is given, the status change will be invoked on the specified character
instead of the one attached to the script. This can only be defined after setting
a rate and flag.

'sc_start2' and 'sc_start4' allow extra parameters to be passed, and are used only
for effects that require them. The meaning of the extra values vary depending on the
effect type. For more infos, read status_change.txt containing a list of all Status Changes
and theirs val1, val2, val3, and val4 usage in source.

'sc_end' will remove a specified status effect. If SC_ALL (-1) is given, it will
perform a complete removal of all statuses (although permanent ones will re-apply).

'sc_end_class' works like 'sc_end' but will remove all status effects from any learned
skill on the invoking character. If <job_id> is provided it will end the effect for that job.

Examples:
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

Note: to use SC_NOCHAT you should alter Manner
	set Manner, -5;	// Will mute a user for 5 minutes
	set Manner, 0;	// Will unmute a user
	set Manner, 5;	// Will unmute a user and prevent the next use of 'Manner'

---------------------------------------

## getstatus
*getstatus(<effect type>{,<type>{,<char_id>}})

Retrieve information about a specific status effect when called. Depending on <type>
specified the function will return different information.

Possible <type> values:
	- 0 or undefined: whether the status is active
	- 1: the val1 of the status
	- 2: the val2 of the status
	- 3: the val3 of the status
	- 4: the val4 of the status
	- 5: the amount of time in milliseconds that the status has remaining

If <type> is not defined or is set to 0, then the script function will either
return 1 if the status is active, or 0 if the status is not active. If the status
is not active when any of the <type> fields are provided, this script function
will always return 0.

---------------------------------------

## skilleffect
*skilleffect <skill id>,<number>;
## skilleffect
*skilleffect "<skill name>",<number>;

This command displays visual and aural effects of given skill on currently
attached character. The number parameter is for skill whose visual effect
involves displaying of a number (healing or damaging). Note, that this command
will not actually use the skill, it is intended for scripts, which simulate
skill usage by the NPC, such as buffs, by setting appropriate status and
displaying the skill's effect.

	mes "Be blessed!";
	// Heal of 2000 HP
	heal 2000,0;
	skilleffect 28,2000;
	// Blessing Level 10
	sc_start SC_BLESSING,240000,10;
	skilleffect 34,0;
	// Increase AGI Level 5
	sc_start SC_INCREASEAGI,140000,5;
	skilleffect 29,0;

This will heal the character with 2000 HP, buff it with Blessing Lv 10 and
Increase AGI Lv 5, and display appropriate effects.

---------------------------------------

## npcskilleffect
*npcskilleffect <skill id>,<number>,<x>,<y>;
## npcskilleffect
*npcskilleffect "<skill name>",<number>,<x>,<y>;

This command behaves identically to 'skilleffect', however, the effect will not
be centered on the invoking character's sprite, nor on the NPC sprite, if any,
but will be centered at map coordinates given on the same map as the invoking
character.

---------------------------------------

## specialeffect
*specialeffect <effect number>{,<send_target>{,"<NPC Name>"}};

This command will display special effect with the given number, centered on the
specified NPCs coordinates, if any. For a full list of special effect numbers
known see 'doc/effect_list.txt'. Some effect numbers are known not to work in
some client releases. (Notably, rain is absent from any client executables
released after April 2005.)

<NPC name> parameter will display <effect number> on another NPC. If the NPC
specified does not exist, the command will do nothing. When specifying an NPC,
<send_target> must be specified when specifying an <NPC Name>, specifying AREA
will retain the default behavior of the command.

	// this will make the NPC "John Doe#1"
	// show the effect "EF_HIT1" specified by
	// Jane Doe. I wonder what John did...
	mes "[Jane Doe]";
	mes "Well, I never!";
	specialeffect EF_HIT1,AREA,"John Doe#1";
	close;

---------------------------------------

## specialeffect2
*specialeffect2 <effect number>{,<send_target>{,"<Player Name>"}};

This command behaves identically to 'specialeffect', but the effect will be
centered on the invoking character's sprite.

<Player name> parameter will display <effect number> on another Player than the
one currently attached to the script. Like with specialeffect, when specifying
a player, <send_target> must be supplied, specifying AREA will retain the default
behavior of the command.

---------------------------------------

## statusup
*statusup <stat>{,<char_id>};

This command will change a specified stat of the invoking character up by one
permanently. Stats are to be given as number, but you can use these constants to
replace them:

bStr -  Strength
bVit -  Vitality
bInt -  Intelligence
bAgi -  Agility
bDex -  Dexterity
bLuk -  Luck

---------------------------------------

## statusup2
*statusup2 <stat>,<amount>{,<char_id>};

This command will change a specified stat of the invoking character by the
specified amount permanently. The amount can be negative. See 'statusup'.

	// This will decrease a character's Vit forever.
	statusup2 bVit,-1;

---------------------------------------

## bonus
*bonus <bonus type>,<val1>;
## bonus2
*bonus2 <bonus type>,<val1>,<val2>;
## bonus3
*bonus3 <bonus type>,<val1>,<val2>,<val3>;
## bonus4
*bonus4 <bonus type>,<val1>,<val2>,<val3>,<val4>;
## bonus5
*bonus5 <bonus type>,<val1>,<val2>,<val3>,<val4>,<val5>;

These commands are meant to be used in item scripts. They will probably work
outside item scripts, but the bonus will not persist for long. They, as
expected, refer only to an invoking character.

You can find the full list of possible bonuses and which command to use for each
kind in 'doc/item_bonus.txt'.

---------------------------------------

## autobonus
*autobonus <bonus script>,<rate>,<duration>{,<flag>,{<other script>}};
## autobonus2
*autobonus2 <bonus script>,<rate>,<duration>{,<flag>,{<other script>}};
## autobonus3
*autobonus3 <bonus script>,<rate>,<duration>,<skill id>,{<other script>};
## autobonus3
*autobonus3 <bonus script>,<rate>,<duration>,"<skill name>",{<other script>};

These commands are meant to be used in item scripts. They will probably work
outside item scripts, but the bonus will not persist for long. They, as
expected, refer only to an invoking character.

What these commands do is 'attach' a script to the player which will get
executed on attack (or when attacked in the case of autobonus2).

Rate is the trigger rate of the script (1000 = 100%).

Duration is the time in milliseconds that the bonus will last for since the script has triggered.

Skill ID/skill name the skill which will be used as trigger to start the bonus. (autobonus3)

The optional argument 'flag' is used to classify the type of attack where the script
can trigger (it shares the same flags as the bAutoSpell bonus script):

Range criteria:
	BF_SHORT:  Trigger on melee attack
	BF_LONG:   Trigger on ranged attack
	Default:   BF_SHORT+BF_LONG
Attack type criteria:
	BF_WEAPON: Trigger on weapon skills
	BF_MAGIC:  Trigger on magic skills
	BF_MISC:   Trigger on misc skills
	Default:   BF_WEAPON
Skill criteria:
	BF_NORMAL: Trigger on normal attacks
	BF_SKILL:  Trigger on skills
	default:   If the attack type is BF_WEAPON (only) BF_NORMAL is used,
		   otherwise BF_SKILL+BF_NORMAL is used.

The difference between the optional argument 'other script' and the 'bonus script' is that,
the former one triggers only when attacking(or attacked) and the latter one runs on
status calculation as well, which makes sure, within the duration, the "bonus" that get
lost on status calculation is restored. So, 'bonus script' is technically supposed to accept
"bonus" command only. And we usually use 'other script' to show visual effects.

In all cases, when the script triggers, the attached player will be the one
who holds the bonus. There is currently no way of knowing within this script
who was the other character (the attacker in autobonus2, or the target in
autobonus and autobonus3).

//Grants a 1% chance of starting the state "all stats +10" for 10 seconds when
//using weapon or misc attacks (both melee and ranged skills) and shows a special
//effect when the bonus is active.
	autobonus "{ bonus bAllStats,10; }",10,10000,BF_WEAPON|BF_MISC,"{ specialeffect2 EF_FIRESPLASHHIT; }";

---------------------------------------

## bonus_script
*bonus_script "<script code>",<duration>{,<flag>{,<type>{,<status_icon>{,<char_id>}}}};

This command will attach a script to a player for a given duration, in seconds.
After that time, the script will automatically expire. The same bonus cannot be
stacked. By default, this bonus will be stored on `bonus_script` table when player
logs out.

Flags (bitmask):
	1   : Remove when dead.
	2   : Removable by Dispell.
	4   : Removable by Clearance.
	8   : Remove when player logs out.
	16  : Removeable by Banishing Buster.
	32  : Removable by Refresh.
	64  : Removable by Lux Anima.
	128 : Remove when Madogear is activated or deactivated.
	256 : Remove when receive damage.
	512 : Script is permanent, cannot be cleared by bonus_script_clear.
	1024: Force to replace duplicated script by expanding the duration.
	2048: Force to add duplicated script. This flag cannot be stacked with 1024,
	      if both are defined, 1024 will be checked first and ignore this flag.

Types:
	This will be used to decide negative or positive buff for 'debuff_on_logout'.
	0: Ignore the buff type and won't be removed if the flag is not &8 (Default)
	1: Buff
	2: Debuff

Status_icon: See "Status Icon" section in 'src/map/script_constants.hpp'. Default is SI_BLANK (-1).

Example:
	// Apple gives you +5 Str bonus for 1 minute when it's consumed.
	512,Apple,Apple,0,15,,20,,,,,0xFFFFFFFF,63,2,,,,,,{ bonus_script "{ bonus bStr,5; }",60; },{},{}

---------------------------------------

## bonus_script_clear
*bonus_script_clear {<flag>,{<char_id>}};

Removes attached bonus_script from player. If no 'char_id' given, it will removes
from the invoker.

If 'flag' is 1, means will clears all scripts even it's Permanent effect. By default,
it just removes non-permanent script.

---------------------------------------

## skill
*skill <skill id>,<level>{,<flag>};
## skill
*skill "<skill name>",<level>{,<flag>};
## addtoskill
*addtoskill <skill id>,<level>{,<flag>};
## addtoskill
*addtoskill "<skill name>",<level>{,<flag>};

These commands will give the invoking character a specified skill. This is also
used for item scripts.

Level is obvious. Skill id is the ID number of the skill in question as per
'db/(pre-)re/skill_db.txt'. It is not known for certain whether this can be used to give
a character a monster's skill, but you're welcome to try with the numbers given
in 'db/(pre-)re/mob_skill_db.txt'.

Flag is 0 if the skill is given permanently (will get written with the character
data) or 1 if it is temporary (will be lost eventually, this is meant for card
item scripts usage.).  The flag parameter is optional, and defaults to 1 in
'skill' and to 2 in 'addtoskill'.

Flag 2 means that the level parameter is to be interpreted as a stackable
additional bonus to the skill level. If the character did not have that skill
previously, they will now at 0+the level given.

Flag 3 is the same as flag 1 in that it saves to the database.  However, these skills
are ignored when any action is taken that adjusts the skill tree (reset/job change).

Flag constants:
	0 - SKILL_PERM
	1 - SKILL_TEMP
	2 - SKILL_TEMPLEVEL
	3 - SKILL_PERM_GRANT

// This will permanently give the character Stone Throw (TF_THROWSTONE,152), at
// level 1.
    skill 152,1,0;

---------------------------------------

## nude
*nude {<char_id>};

This command will unequip anything equipped on the invoking character.

It is not required to do this when changing jobs since 'jobchange' will unequip
everything not equippable by the new job class anyway.

---------------------------------------

## sit
*sit {"<character name>"};
## stand
*stand {"<character name>"};

These commands will make a character sit or stand.
If no character is specified, the command will run for the invoking character.

Additionnally Sitting constant is true when the character is sitting, false otherwise.

---------------------------------------

## disguise
*disguise <Monster ID>{,<char_id>};
## undisguise
*undisguise {<char_id>};

This command disguises the current player with a monster sprite.
The disguise lasts until 'undisguise' is issued or the player logs out.

Example:

disguise 1002; // Disguise character as a Poring.
next;
undisguise; // Return to normal character sprite.

---------------------------------------

## transform
*transform <monster ID>,<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
## transform
*transform "<monster name>",<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
## active_transform
*active_transform <monster ID>,<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
## active_transform
*active_transform "<monster name>",<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};

This command will turn a player into a monster for a given duration and can grant
a SC attribute effect while transformed. Note that players cannot be transformed
during War of Emperium or if already disguised.
Can only be removed when you die or the duration ends.

'transform' and 'active_transform' can stack on each other but using 'transform' or
'active_transform' twice will not stack (it will cancel the previous bonus for the new).
'active_transform' will take priority over transform for its duration.

---------------------------------------
\\
4,3 Marriage-related commands
\\
---------------------------------------

## marriage
*marriage("<spouse name>");

This function will marry two characters, the invoking character and the one
referred to by name given, together, setting them up as each other's marriage
partner. No second function call has to be issued (in current SVN at least) to
make sure the marriage works both ways. The function returns 1 upon success, or
0 if the marriage could not be completed, either because the other character
wasn't found or because one of the two characters is already married.

This will do nothing else for the marriage except setting up the spouse ID for
both of these characters. No rings will be given and no effects will be shown.

---------------------------------------

## wedding
*wedding;

This command will call up wedding effects - the music and confetti - centered on
the invoking character. Example can be found in the wedding script.

---------------------------------------

## divorce
*divorce({<char_id>})

This function will "un-marry" the invoking character from whoever they were
married to. Both will no longer be each other's marriage partner, (at least in
current SVN, which prevents the cases of multi-spouse problems). It will return
1 upon success or 0 if the character was not married at all.

This function will also destroy both wedding rings and send a message to both
players, telling them they are now divorced.

---------------------------------------

## adopt
*adopt("<parent_name>","<baby_name>");
## adopt
*adopt(<parent_id>,<baby_id>);

This function will send the client adoption request to the specified baby
character. The parent value can be either parent. Both parents and the baby
need to be online in order for adoption to work.

Return values:
 ADOPT_ALLOWED - Sent message to Baby to accept or deny.
 ADOPT_ALREADY_ADOPTED - Character is already adopted.
 ADOPT_MARRIED_AND_PARTY - Parents need to be married and in a party with the baby.
 ADOPT_EQUIP_RINGS - Parents need wedding rings equipped.
 ADOPT_NOT_NOVICE - Baby is not a Novice.
 ADOPT_CHARACTER_NOT_FOUND - A parent or Baby was not found.
 ADOPT_MORE_CHILDREN - You cannot adopt more than 1 child. (client message)
 ADOPT_LEVEL_70 - Parents need to be at least level 70 in order to adopt someone. (client message)
 ADOPT_MARRIED - You cannot adopt a married person. (client message)

---------------------------------------
//
4,3.- End of marriage-related commands
//
---------------------------------------

## pcfollow
*pcfollow <id>,<target id>;
## pcstopfollow
*pcstopfollow <id>;

Makes a character follow or stop following someone. This command does the same
as the @follow command. The main difference is that @follow can use character
names, and this commands needs the account ID for the target.

Examples:
	// This will make Aaron follow Bullah, when both of these characters are online.
	pcfollow getCharID(3,"Aaron"),getCharID(3,"Bullah");

	// Makes Aaron stop following whoever he is following.
	pcstopfollow getCharID(3,"Aaron");

---------------------------------------

## pcblockmove
*pcblockmove <id>,<option>;
## unitblockmove
*unitblockmove <id>,<option>;

Prevents the given GID from moving when the option is 1, and enables the ID to
move again when the option is 0. This command will run for the attached unit
if the given GID is zero.

Examples:
	// Prevents the current char from moving away.
	pcblockmove getcharid(3),1;

	// Enables the current char to move again.
	pcblockmove getcharid(3),0;

---------------------------------------

## pcblockskill
*pcblockskill <id>,<option>;
## unitblockskill
*unitblockskill <id>,<option>;

Prevents the given GID from casting skills when the option is 1, and enables
the ID to cast skills again when the option is 0. This command will run for
the attached unit if the given GID is zero.

Examples:
	// Prevents the current char from casting skills.
	pcblockskill getcharid(3),1;

	// Enables the current char to cast skills again.
	pcblockskill getcharid(3),0;

---------------------------------------

## setpcblock
*setpcblock <type>,<state>{,<account ID>};
## getpcblock
*getpcblock {<account ID>};

'setpcblock' command prevents/allows the player from doing the given <type> of action according
to the <state> during the player session (note: @reloadscript removes all <type> except PCBLOCK_IMMUNE).
The <type> values are bit-masks, multiples of <type> can be added to change the player action.

The action is blocked when the <state> is true, while false allows the action again.

'getpcblock' command return the bit-mask value of the currently
enabled block flags.

Available <type>:
	PCBLOCK_MOVE				Prevent the player from moving.
	PCBLOCK_ATTACK				Prevent the player from attacking.
	PCBLOCK_SKILL				Prevent the player from using skills/itemskills.
	PCBLOCK_USEITEM				Prevent the player from using usable items.
	PCBLOCK_CHAT				Prevent the player from sending global/guild/party/whisper messages.
	PCBLOCK_IMMUNE				Prevent the player from being hit by monsters.
	PCBLOCK_SITSTAND			Prevent the player from sitting/standing.
	PCBLOCK_COMMANDS			Prevent the player from using atcommands/charcommands.
	PCBLOCK_NPCCLICK			Prevent the player from clicking/touching any NPC/shop/warp.
	PCBLOCK_EMOTION				Prevent the player from using emotions.
	PCBLOCK_NPC				Simulate NPC interaction. Useful for NPC with no mes window. Sum of PCBLOCK_MOVE|PCBLOCK_SKILL|PCBLOCK_USEITEM|PCBLOCK_COMMANDS|PCBLOCK_NPCCLICK.
	PCBLOCK_ALL				Sum of all the flags.

Examples:

// Make the attached player invulnerable to monster (same as @monsterignore)
	setpcblock PCBLOCK_IMMUNE, true;

// Prevents the attached player from attacking and using skills
	setpcblock PCBLOCK_ATTACK|PCBLOCK_SKILL, true;

// Re-enables attack, skills and item use
	setpcblock PCBLOCK_ATTACK|PCBLOCK_SKILL|PCBLOCK_USEITEM, false;

// getpcblock related checks
	if (getpcblock() & PCBLOCK_IMMUNE)
		mes "You are invulnerable!";

	if (getpcblock() & (PCBLOCK_MOVE|PCBLOCK_SITSTAND))
		mes "You can't walk or sit.";

	if ((getpcblock() & (PCBLOCK_ATTACK|PCBLOCK_SKILL)) == 0)
		mes "You can attack and use skills.";

	if (getpcblock() & PCBLOCK_CHAT)
		mes "You can't chat.";

---------------------------------------

# 5. Mob / NPC -related commands

## monster
*monster     "<map name>",<x>,<y>,"<name to show>",<mob id>,<amount>{,"<event label>",<size>,<ai>};
## areamonster
*areamonster "<map name>",<x1>,<y1>,<x2>,<y2>,"<name to show>",<mob id>,<amount>{,"<event label>",<size>,<ai>};

This command will spawn a monster on the specified coordinates on the specified
map. If the script is invoked by a character, a special map name, "this", will
be recognized to mean the name of the map the invoking character is located at.
This command works fine in the item scripts.

The same command arguments mean the same things as described above in the
beginning of this document when talking about permanent monster spawns. Monsters
spawned in this manner will not respawn upon being killed.

Unlike the permanent monster spawns, if the mob id is -1, a random monster will
be picked from the entire database according to the rules configured in the
server for dead branches. This will work for all other kinds of non-permanent
monster spawns.

The only very special thing about this command is an event label, which is an
optional parameter. This label is written like '<NPC object name>::<label name>'
and upon the monster being killed, it will execute the script inside of the
specified NPC object starting from the label given. The RID of the player
attached at this execution will be the RID of the killing character.
The variable 'killedrid' is set to the Class (mob ID) of the monster killed.
The variable 'killedgid' is set to the ID (unique mob game ID) of the monster killed.

<size> can be:
	Size_Small	(0)		(default)
	Size_Medium	(1)
	Size_Large	(2)

<ai> can be:
	AI_NONE		(0)		(default)
	AI_ATTACK	(1)		(attack/friendly)
	AI_SPHERE	(2)		(Alchemist skill)
	AI_FLORA	(3)		(Alchemist skill)
	AI_ZANZOU	(4)		(Kagerou/Oboro skill)
	AI_LEGION	(5)		(Sera skill)
	AI_FAW		(6)		(Mechanic skill)

    monster "place",60,100,"Poring",1002,1,"NPCNAME::OnLabel";

The coordinates of 0,0 will spawn the monster on a random place on the map.

The 'areamonster' command works much like the 'monster' command and is not
significantly different, but spawns the monsters within a square defined by
x1/y1-x2/y2.

Returned value is an array with the game ID of the spawned monster(s) depending
on the amount spawned. Array is stored in $@mobid[].

Simple monster killing script:

		<Normal NPC object definition. Let's assume you called him NPCNAME.>
		mes "[Summon Man]";
		mes "Want to start the Poring hunt?";
		next;
		if (select("Yes.:No.") == 2) {
			mes "[Summon Man]";
			mes "Come back later.";
			close;
		}

		// Summon 10 Porings.
		// Using coordinates 0,0 will spawn them in a random location.
		monster "prontera",0,0,"Quest Poring",1002,10,"NPCNAME::OnPoringKilled";

		mes "[Summon Man]";
		mes "Now go and kill all the Porings I summoned.";
		close;

	OnPoringKilled:
		$PoringKilled++;
		if ($PoringKilled >= 10) {
			announce "Summon Man: Well done. All the Porings are dead!",3;
			$PoringKilled = 0;
		}
		end;

For more good examples see just about any official 2-1 or 2-2 job quest script.

---------------------------------------

## areamobuseskill
*areamobuseskill "<map name>",<x>,<y>,<range>,<mob id>,<skill id>,<skill level>,<cast time>,<cancelable>,<emotion>,<target type>;
## areamobuseskill
*areamobuseskill "<map name>",<x>,<y>,<range>,<mob id>,"<skill name>",<skill level>,<cast time>,<cancelable>,<emotion>,<target type>;

This command will make all monsters of the specified mob ID in the specified
area use the specified skill. Map name, x, and y define the center of the area,
which extending <range> cells in each direction (ex: a range of 3 would create
a 7x7 square). The skill can be specified by skill ID or name. <cast time> is in
milliseconds (1000 = 1 second), and the rest should be self-explanatory.

<target type> can be:
	0 = self
	1 = the mob's current target
	2 = the mob's master
	3 = random target

Example:

	// spawn 1 Shining Plant in the 5x5 area centered on (155,188)
	areamonster "prontera",153,186,157,190,"Shining Plant",1083,1;
	// make the plant cast level 10 Cold Bolt on a random target
	areamobuseskill "prontera",155,188,2,1083,"MG_COLDBOLT",10,3000,1,ET_KEK,3;

---------------------------------------

## killmonster
*killmonster "<map name>","<event label>"{,<type>};

This command will kill all monsters that were spawned with 'monster' or
'addmonster' and have a specified event label attached to them. Commonly used to
get rid of remaining quest monsters once the quest is complete.

If the label is given as "All", all monsters which have their respawn times set
to -1 (like all the monsters summoned with 'monster' or 'areamonster' script
command, and all monsters summoned with GM commands, but no other ones - that
is, all non-permanent monsters) on the specified map will be killed regardless
of the event label value.

As of r12876 killmonster now supports an optional argument type. Using 1 for type
will make the command fire "OnMyMobDead" events from any monsters that do die
as a result of this command.

---------------------------------------

## killmonsterall
*killmonsterall "<map name>"{,<type>};

This command will kill all monsters on a specified map name, regardless of how
they were spawned or what they are. As of r12873, The behavior has changed slightly.
In light of a label behavior fix for mob spawning commands that will now allow the label to
trigger when there is no player, killmonsterall has also been modified to support this.

Using this the normal/old way means labels don't trigger when a player didn't
attack/kill a monster. This is because it breaks compatibility with older scripts if
forced to use the new method. However, if you wish to use the new label type with this
command, simply use 1 for type. Any other number won't be recognized.

---------------------------------------

## strmobinfo
*strmobinfo(<type>,<monster id>);

This function will return information about a monster record in the database, as
per 'db/(pre-)re/mob_db.txt'. Type is the kind of information returned. Valid types are:
It will return 0 if there is no such monster (or the type value is invalid),
or an empty string if you requested the monster's name.

 1 - 'english name' field in the database, a string.
 2 - 'japanese name' field in the database, a string.
     All other returned values are numbers:
 3 - Level.
 4 - Maximum HP.
 5 - Maximum SP.
 6 - Experience reward.
 7 - Job experience reward.

---------------------------------------

## mobcount
*mobcount("<map name>","<event label>")

This function will count all the monsters on the specified map that have a given
event label and return the number or 0 if it can't find any. Naturally, only
monsters spawned with 'monster' and 'areamonster' script commands can have non-empty
event label.
If you pass this function an empty string for the event label, it will return
the total count of monster without event label, including permanently spawning monsters.
With the dynamic mobs system enabled, where mobs are not kept
in memory for maps with no actual people playing on them, this will return a 0
for any such map.
If the event label is given as "all", all monsters will be counted, regardless of
having any event label attached.

If the map name is given as "this", the map the invoking character is on will
be used. If the map is not found, or the invoker is not a character while the map
is "this", it will return -1.

---------------------------------------

## clone
*clone "<map name>",<x>,<y>,"<event>",<char id>{,<master_id>{,<mode>{,<flag>,<duration>}}}

This command creates a monster which is a copy of another player. The first
four arguments serve the same purpose as in the monster script command, The
<char id> is the character id of the player to clone (player must be online).
If <master id> is given, the clone will be a 'slave/minion' of it. Master_id
must be a character id of another online player.

The mode can be specified to determine the behavior of the clone. Its
values are the same as the ones used for the mode field in the mob_db. The
default mode is aggressive, assists, can move, can attack.

Flag can be either zero or one currently. If zero, the clone is a normal
monster that'll target players, if one, it is considered a summoned monster,
and as such, it'll target other monsters. Defaults to zero.

The duration specifies how long the clone will live before it is auto-removed.
Specified in seconds, defaults to no limit (zero).

Returned value is the monster ID of the spawned clone. If command fails,
returned value is zero.

---------------------------------------

## summon
*summon "monster name",<monster id>{,<Time Out>{,"event label"}};

This command will summon a monster. (see also 'monster') Unlike monsters spawned
with other commands, this one will set up the monster to fight to protect the
invoking character. Monster name and mob id obey the same rules as the one given
at the beginning of this document for permanent monster spawns with the
exceptions mentioned when describing 'monster' command.

The effect for the skill 'Call Homunculus' will be displayed centered on the
invoking character.

Timeout is the time in milliseconds the summon lives, and is set default
to 60000 (1 minute). Note that also the value 0 will set the timer to default,
and it is not possible to create a spawn that lasts forever.
If an event label is given, upon the monster being killed, the event label will
run as if by 'donpcevent'.

Returned value is the game ID of the spawned monster.

// Will summon a dead branch-style monster to fight for the character.
summon "--ja--",-1;

---------------------------------------

## addmonsterdrop
*addmonsterdrop <monster id>,<item id>,<rate>;
## addmonsterdrop
*addmonsterdrop "<monster name>",<item id>,<rate>;
## delmonsterdrop
*delmonsterdrop <monster id>,<item id>;
## delmonsterdrop
*delmonsterdrop "<monster name>",<item id>;

These commands will temporarily add or delete a monster drop, which will be reset
when the mob database reloads or the server shuts down. They return 1 upon success.

If the monster already drops the specified item, its drop rate will be updated with
the given rate (100 = 1%).

Examples:
	// Makes Owl Baron drop Honey at an 80% rate.
	addmonsterdrop 1295,518,8000;

	// Deletes Executioner's Mitten from Rybio.
	delmonsterdrop 1201,7017;

---------------------------------------

## disablenpc
*disablenpc "<NPC object name>";
## enablenpc
*enablenpc "<NPC object name>";

These two commands will disable and enable, respectively, an NPC object
specified by name. The disabled NPC will disappear from sight and will no longer
be triggerable in the normal way. It is not clear whether it will still be
accessible through 'donpcevent' and other triggering commands, but it probably
will be. You can disable even warp NPCs if you know their object names, which is
an easy way to make a map only accessible through walking half the time. Then
you 'enablenpc' them back.

You can also use these commands to create the illusion of an NPC switching
between several locations, which is often better than actually moving the NPC -
create one NPC object with a visible and a hidden part to their name, make a few
copies, and then disable all except one.

---------------------------------------

## hideonnpc
*hideonnpc "<NPC object name>";
## hideoffnpc
*hideoffnpc "<NPC object name>";

These commands will make the NPC object specified display as hidden/visible,
even though not actually disabled per se. Hidden as in thief Hide skill, but
unfortunately, not detectable by Ruwach or Sight.

As they are now, these commands are pointless, it is suggested to use
'disablenpc'/'enablenpc', because these two commands actually unload the NPC
sprite location and other accompanying data from memory when it is not used.
However, you can use these for some quest ideas (such as cloaking NPCs talking
while hidden then revealing.... you can wonder around =P

---------------------------------------

## unloadnpc
*unloadnpc "<NPC object name>";

This command will fully unload a NPC object and all of it's duplicates.

---------------------------------------

## doevent
*doevent "<NPC object name>::<event label>";

This command will start a new execution thread in a specified NPC object at the
specified label. The execution of the script running this command will not stop,
and the event called by the 'doevent' command will not run until the invoking
script has terminated. No parameters may be passed with a doevent call.

The script of the NPC object invoked in this manner will run as if it's been
invoked by the RID that was active in the script that issued a 'doevent'. As
such, the command will not work if an RID is not attached.

	place,100,100,1%TAB%script%TAB%NPC%TAB%53,{
		mes "This is what you will see when you click me";
		close;
	OnLabel:
		mes "This is what you will see if the doevent is activated";
		close;
	}

	....

	doevent "NPC::OnLabel";

---------------------------------------

## donpcevent
*donpcevent "<NPC object name>::<event label>";

This command invokes the event label code within an another NPC or NPCs. It
starts a separate instance of execution, and the invoking NPC will resume
execution its immediately.

If the supplied event label has the form "NpcName::OnLabel", then only given
NPC's event label will be invoked (much like 'goto' into another NPC). If the
form is "::OnLabel" (NPC name omitted), the event code of all NPCs with given
label will be invoked, one after another. In both cases the invoked script
will run without an attached RID, whether or not the invoking script was
attached to a player. The event label name is required to start with "On".

This command can be used to make other NPCs act, as if they were responding to
the invoking NPC's actions, such as using an emotion or talking.

	place,100,100,1%TAB%script%TAB%NPC1%TAB%53,{
		mes "NPC2 copies my actions!";
		close2;
		donpcevent "NPC2::OnEmote";
		end;
	OnEmote:
		emotion rand(1,30);
		end;
	}

	place,102,100,1%TAB%script%TAB%NPC2%TAB%53,{
		mes "NPC1 copies my actions!";
		close2;
		donpcevent "NPC1::OnEmote";
		end;
	OnEmote:
		emotion rand(1,30);
		end;
	}

Whichever of the both NPCs is talked to, both will show a random emotion at the
same time.

As of r16564, command now returns 1 or 0 on success and failure.
A debug message also shows on the console when no events are triggered.

---------------------------------------

## cmdothernpc
*cmdothernpc "<npc name>","<command>";

This is simply "donpcevent <npc name>::OnCommand<command>".
It is an approximation of official server script language's 'cmdothernpc'.

Returns true if the command was executed on the other NPC successfully, false if not.

---------------------------------------

## npctalk
*npctalk "<message>"{,"<NPC name>","<flag>"};

This command will display a message as if the NPC object running it was a player
talking - that is, above their head and in the chat window.
The display name of the NPC won't get appended in front of the message.
If the <NPC name> option is given and not empty, then that NPC will display the message,
else the attached NPC will display the message.

Target for <flag>:
- bc_all  : Broadcast message is sent server-wide (only in the chat window).
- bc_map  : Message is sent to everyone in the same map as the source of the npc.
- bc_area : Message is sent to players in the vicinity of the source (default value).
- bc_self : Message is sent only to player attached.

	// This will make everyone in the area see the NPC greet the character
	// who just invoked it.
	npctalk "Hello " + strcharinfo(0) + ", how are you?";

---------------------------------------

## chatmes
*chatmes "<message>"{,"<NPC name>"};

This command will display a message in the waitingroom (chat) of the NPC.
If the <NPC name> option is given, then that NPC will display the message, else
the attached NPC will display the message.
If the NPC is not in a waitingroom, nothing happens.

	// Everyone in the waitingroom will see this message:
	chatmes "Waiting 5 minutes until the next match will start";

---------------------------------------

## setnpcdisplay
*setnpcdisplay("<npc name>", "<display name>", <class id>, <size>)
## setnpcdisplay
*setnpcdisplay("<npc name>", "<display name>", <class id>)
## setnpcdisplay
*setnpcdisplay("<npc name>", "<display name>")
## setnpcdisplay
*setnpcdisplay("<npc name>", <class id>)

Changes the display name and/or display class of the target NPC.
Returns 0 is successful, 1 if the NPC does not exist.
Size is 0 = normal 1 = small 2 = big.

---------------------------------------
\\
5,1.- Time-related commands
\\
---------------------------------------

## addtimer
*addtimer <ticks>,"NPC::OnLabel";
## deltimer
*deltimer "NPC::OnLabel";
## addtimercount
*addtimercount <ticks>,"NPC::OnLabel";

These commands will create, destroy, and delay a countdown timer - 'addtimer' to
create, 'deltimer' to destroy and 'addtimercount' to delay it by the specified
number of ticks. For all three cases, the event label given is the identifier of
that timer. The timer runs on the character object that is attached to the script,
and can have multiple instances. When the label is run, it is run as if the player that
the timer runs on has clicked the NPC.

When this timer runs out, a new execution thread will start in the specified NPC
object at the specified label.

The ticks are given in 1/1000ths of a second.

One more thing. These timers are stored as part of player data. If the player
logs out, all of these get immediately deleted, without executing the script.
If this behavior is undesirable, use some other timer mechanism (like 'sleep').

Example:
<NPC Header> {
	dispbottom "Starting a 5 second timer...";
	addtimer 5000, strnpcinfo(3) + "::On5secs";
	end;
On5secs:
	dispbottom "5 seconds have passed!";
	end;
}

---------------------------------------

## initnpctimer
*initnpctimer{ "<NPC name>" {, <Attach Flag>} } |
             { "<NPC name>" | <Attach Flag> };
## stopnpctimer
*stopnpctimer{ "<NPC name>" {, <Detach Flag>}  } |
             { "<NPC name>" | <Detach Flag> };
## startnpctimer
*startnpctimer{ "<NPC name>" {, <Attach Flag>} } |
              { "<NPC name>" | <Attach Flag> };
## setnpctimer
*setnpctimer <tick>{,"<NPC name>"};
## getnpctimer
*getnpctimer(<type of information>{,"<NPC name>"})
## attachnpctimer
*attachnpctimer {"<character name>"};
## detachnpctimer
*detachnpctimer {"<NPC name>"};

This set of commands and functions will create and manage an NPC-based timer.
The NPC name may be omitted, in which case the calling NPC is used as target.

Contrary to addtimer/deltimer commands which let you have many different timers
referencing different labels in the same NPC, each with their own countdown,
'initnpctimer' can only have one per NPC object. But it can trigger many labels
and let you know how many were triggered already and how many still remain.

This timer is counting up from 0 in ticks of 1/1000ths of a second each. Upon
creating this timer, the execution will not stop, but will happily continue
onward. The timer will then invoke new execution threads at labels
"OnTimer<time>:" in the NPC object it is attached to.

To create the timer, use the 'initnpctimer', which will start it running.
'stopnpctimer' will pause the timer, without clearing the current tick, while
'startnpctimer' will let the paused timer continue.

By default timers do not have a RID attached, which lets them continue even
if the player that started them logs off. To attach a RID to a timer, you can
either use the optional "attach flag" when using 'initnpctimer/startnpctimer',
or do it manually by using 'attachnpctimer'. Likewise, the optional flag of
stopnpctimer lets you detach any RID after stopping the timer, and by using
'detachnpctimer' you can detach a RID at any time.

Normally there is only a single timer per NPC, but as an exception, as long as
you attach a player to the timer, you can have multiple timers running at once,
because these will get stored on the players instead of the NPC.
NOTE: You need to attach the RID before the timer _before_ you start it to
get a player-attached timer. Otherwise it'll stay a NPC timer (no effect).

If the player that is attached to the npctimer logs out, the "OnTimerQuit:"
event label of that NPC will be triggered, so you can do the appropriate
cleanup (the player is still attached when this event is triggered).

The 'setnpctimer' command will explicitly set the timer to a given tick.
'getnpctimer' provides timer information. Its parameter defines what type:

 0 - Will return the current tick count of the timer.
 1 - Will return 1 if there are remaining "OnTimer<ticks>:" labels in the
     specified NPC waiting for execution.
 2 - Will return the number of times the timer has triggered and will trigger
     an "OnTimer<tick>:"  label in the specified NPC.

Example 1:

	<NPC Header> {
	// We need to use attachnpctimer because the mes command below needs RID attach
		attachnpctimer;
		initnpctimer;
		npctalk "I cant talk right now, give me 10 seconds";
		end;
	OnTimer5000:
		npctalk "Ok 5 seconds more";
		end;
	OnTimer6000:
		npctalk "4";
		end;
	OnTimer7000:
		npctalk "3";
		end;
	OnTimer8000:
		npctalk "2";
		end;
	OnTimer9000:
		npctalk "1";
		end;
	OnTimer10000:
		stopnpctimer;
		mes "[Man]";
		mes "Ok we can talk now";
		detachnpctimer;
		// and remember attachnpctimer and detachnpctimer can only use while the NPC timer is not running !
	}

Example 2:

	OnTimer15000:
		npctalk "Another 15 seconds have passed.";

		// You have to use 'initnpctimer' instead of 'setnpctimer 0'.
		// This is equal to 'setnpctimer 0' + 'startnpctimer'.
		// Alternatively, you can also insert another 'OnTimer15001' label so that the timer won't stop. */
		initnpctimer;
		end;

	// This OnInit label will run when the script is loaded, so that the timer
	// is initialized immediately as the server starts. It is dropped back to 0
	// every time the NPC says something, so it will cycle continuously.
	OnInit:
		initnpctimer;
		end;

Example 3:

	mes "[Man]";
	mes "I have been waiting " + (getnpctimer(0)/1000) + " seconds for you.";
	// We divide the timer returned by 1000 to convert milliseconds to seconds.
	close;

Example 4:

	mes "[Man]";
	mes "Ok, I will let you have 30 more seconds...";
	close2;
	setnpctimer (getnpctimer(0)-30000);
	// Notice the 'close2'. If there were a 'next' there the timer would be
	// changed only after the player pressed the 'next' button.
	end;

---------------------------------------

## sleep
*sleep {<milliseconds>};
## sleep2
*sleep2 {<milliseconds>};
## awake
*awake "<NPC name>";

These commands are used to control the pause of a NPC.
sleep and sleep2 will pause the script for the given amount of milliseconds.
Awake is used to cancel a sleep. When awake is called on a NPC it will run as
if the sleep timer ran out, and thus making the script continue. Sleep and sleep2
basically do the same, but the main difference is that sleep will not keep the rid,
while sleep2 does. Also sleep2 will stop the script if there is no unit attached.

Examples:
	sleep 10000; //pause the script for 10 seconds and ditch the RID (so no player is attached anymore)
	sleep2 5000; //pause the script for 5 seconds, and continue with the RID attached.
	awake "NPC"; //Cancels any running sleep timers on the NPC 'NPC'.

---------------------------------------

## progressbar
*progressbar "<color>",<seconds>;

This command works almost like sleep2, but displays a progress bar
above the head of the currently attached character (like cast bar).
Once the given amount of seconds passes, the script resumes. If the
character moves while the progress bar progresses, it is aborted and
the script ends. The color format is in RGB (RRGGBB). The color is
currently ignored by the client and appears always green.

---------------------------------------

## progressbar_npc
*progressbar_npc "<color>",<seconds>{,<"NPC Name">};

This command works like progressbar, but displays a progress bar
above the head of the currently attached (or given) NPC. Once the
given amount of seconds passes, the script resumes. The color format
is in RGB (RRGGBB). The color is currently ignored by the client and
appears always green.

Note: If a player is attached to the NPC, they are detached from the NPC
as soon as the progress bar activates.

---------------------------------------
//
5,1.- End of time-related commands
//
---------------------------------------

## announce
*announce "<text>",<flag>{,<fontColor>{,<fontType>{,<fontSize>{,<fontAlign>{,<fontY>}}}}};

This command will broadcast a message to all or most players, similar to
@kami/@kamib GM commands.

	announce "This will be shown to everyone at all in yellow.",0;

The region the broadcast is heard in (target), source of the broadcast
and the color the message will come up as is determined by the flags.

The flag values are coded as constants in 'src/map/script_constants.hpp' to make them easier to use.

Target flags:
- bc_all: Broadcast message is sent server-wide (default).
- bc_map: Message is sent to everyone in the same map as the source of the broadcast (see below).
- bc_area: Message is sent to players in the vicinity of the source.
- bc_self: Message is sent only to current player.
You cannot use more than one target flag.

Source flags:
- bc_pc: Broadcast source is the attached player (default).
- bc_npc: Broadcast source is the NPC, not the player attached to the script
  (useful when a player is not attached or the message should be sent to those
  nearby the NPC).
You cannot use more than one source flag.

Special flags:
- bc_yellow: Broadcast will be displayed in yellow color (default).
- bc_blue: Broadcast will be displayed in blue color.
- bc_woe: Indicates that this broadcast is 'WoE Information' that can be disabled client-side.
Due to the way client handles broadcasts, it is impossible to set both bc_blue and bc_woe.

The optional parameters allow usage of broadcasts in custom colors, font-weights, sizes etc.
If any of the optional parameters is used, special flag is ignored.
Optional parameters may not work well (or at all) depending on a game client used.

The color parameter is a single number which can be in hexadecimal notation.
For example:
    announce "This will be shown to everyone at all in green.",bc_all,0x00FF00;
Will display a global announce in green. The color format is in RGB (0xRRGGBB).

In official scripts only two font-weights (types) are used:
 - normal (FW_NORMAL = 400, default),
 - bold (FW_BOLD = 700).

Default font size is 12.

Using this for private messages to players is probably not that good an idea,
but it can be used instead in NPCs to "preview" an announce.

	// This will be a private message to the player using the NPC that made the
	// announcement
	announce "This is my message just for you",bc_blue|bc_self;

	// This will be shown on everyones screen that is in sight of the NPC.
	announce "This is my message just for you people here",bc_npc|bc_area;

---------------------------------------

## mapannounce
*mapannounce "<map name>","<text>",<flag>{,<fontColor>{,<fontType>{,<fontSize>{,<fontAlign>{,<fontY>}}}}}};

This command will work like 'announce' but will only broadcast to characters
currently residing on the specified map. The flag and optional parameters
parameters are the same as in 'announce', but target and source flags are ignored.

---------------------------------------

## areaannounce
*areaannounce "<map name>",<x1>,<y1>,<x2>,<y2>,"<text>",<flag>{,<fontColor>{,<fontType>{,<fontSize>{,<fontAlign>{,<fontY>}}}}}};

This command works like 'announce' but will only broadcast to characters
residing in the specified x1/y1-x2/y2 rectangle on the map given. The flags and
optional parameters are the same as in 'announce', but target and source flags are ignored.

	areaannounce "prt_church",0,0,350,350,"God's in his heaven, all right with the world",0;

---------------------------------------

## callshop
*callshop "<name>",<option>;

These are a series of commands used to create dynamic shops.
The 'callshop' function calls an invisible shop (view -1) as if the player clicked on it.

The options are:
	0 = The normal window (buy, sell and cancel)
	1 = The buy window
	2 = The sell window

Note: The <option> parameter only works on the 'shop' type NPC.

A shop called with this command will trigger the labels "OnBuyItem" and "OnSellItem"
(as long as an npcshop* command is executed from that NPC, see note below). These
labels, if used, will replace how the shop handles the buying and selling of items,
allowing for the creation of dynamic shops.

The label "OnBuyItem" sets the following arrays:
	@bought_nameid   - item ID bought
	@bought_quantity - amount bought

The label "OnSellItem" sets the following arrays:
	@sold_nameid        - item ID sold
	@sold_quantity      - amount sold
	@sold_refine        - refine count
	@sold_attribute     - if the item is broken (1) or not (0)
	@sold_identify      - if the item is identified (1) or not (0)
	@sold_card1         - card slot 1
	@sold_card2         - card slot 2
	@sold_card3         - card slot 3
	@sold_card4         - card slot 4
	@sold_option_id1    - random option ID 1
	@sold_option_val1   - random option value 1
	@sold_option_param1 - random option param 1
	@sold_option_id2    - random option ID 2
	@sold_option_val2   - random option value 2
	@sold_option_param2 - random option param 2
	@sold_option_id3    - random option ID 3
	@sold_option_val3   - random option value 3
	@sold_option_param3 - random option param 3
	@sold_option_id4    - random option ID 4
	@sold_option_val4   - random option value 4
	@sold_option_param4 - random option param 4
	@sold_option_id5    - random option ID 5
	@sold_option_val5   - random option value 5
	@sold_option_param5 - random option param 5

Note: These labels will only be triggered if an npcshop* command is executed because these
commands set a special data on the shop NPC, named master_nd in the source. The above labels
are triggered in the NPC whose master_nd is given in the shop.

A full example of a dynamic shop can be found in doc/sample/npc_dynamic_shop.txt.

---------------------------------------

## npcshopitem
*npcshopitem "<name>",<item id>,<price>{,<item id>,<price>{,<item id>,<price>{,...}}};

This command lets you override the contents of an existing NPC shop or cashshop. The
current sell list will be wiped, and only the items specified with the price
specified will be for sale.

The function returns 1 if shop was updated successfully, or 0 if not found.

NOTES:
 - That you cannot use -1 to specify default selling price!
 - If attached shop type is market shop, need an extra param after price, it's <qty>
   and make sure don't add duplication item!

---------------------------------------

## npcshopadditem
*npcshopadditem "<name>",<item id>,<price>{,<item id>,<price>{,<item id>,<price>{,...}}};

This command will add more items at the end of the selling list for the
specified NPC shop or cashshop. If you specify an item already for sell, that item will
appear twice on the sell list.

The function returns 1 if shop was updated successfully, or 0 if not found.

NOTES:
 - That you cannot use -1 to specify default selling price!
 - If attached shop type is market shop, need an extra param after price, it's <qty>
   and make sure don't add duplication item!

---------------------------------------

## npcshopdelitem
*npcshopdelitem "<name>",<item id>{,<item id>{,<item id>{,...}}};

This command will remove items from the specified NPC shop or cashshop.
If the item to remove exists more than once on the shop, all instances will be
removed.

Note that the function returns 1 even if no items were removed. The return
value is only to confirm that the shop was indeed found.

---------------------------------------

## npcshopattach
*npcshopattach "<name>"{,<flag>};

This command will attach the current script to the given NPC shop.
When a script is attached to a shop, the events "OnBuyItem" and "OnSellItem"
of your script will be executed whenever a player buys/sells from the shop.
Additionally, the arrays @bought_nameid[], @bought_quantity[] or @sold_nameid[]
and @sold_quantity[] will be filled up with the items and quantities
bought/sold.

The optional parameter specifies whether to attach ("1") or detach ("0") from
the shop (the default is to attach). Note that detaching will detach any NPC
attached to the shop, even if it's from another script, while attaching will
override any other script that may be already attached.

The function returns 0 if the shop was not found, 1 otherwise.

NOTES:
 - If attached shop type is market shop, will be default to call the 'buy' window.

---------------------------------------

## npcshopupdate
*npcshopupdate "<name>",<item_id>,<price>{,<stock>}

Update an entry from shop. If price is 0 means don't change the price, maybe used for
marketshop to update the stock quantity. Except marketshop type, 'stock' value means
nothing.

---------------------------------------

## waitingroom
*waitingroom "<chatroom name>",<limit>{,"<event label>"{,<trigger>{,<required zeny>{,<min lvl>{,<max lvl>}}}}};

This command will create a chat room, owned by the NPC object running this
script and displayed above the NPC sprite.
The maximum length of a chat room name is 60 letters.

The limit is the maximum number of people allowed to enter the chat room.
The attached NPC is included in this count. If the optional event and trigger
parameters are given, the event label ("<NPC object name>::<label name>")
will be invoked as if with a 'doevent' upon the number of people in the chat
room reaching the given triggering amount.

// The NPC will just show a box above its head that says "Hello World", clicking
// it will do nothing, since the limit is zero.
    waitingroom "Hello World",0;

// The NPC will have a box above its head, it will say "Disco - Waiting Room"
// and will have 8 waiting slots. Clicking this will enter the chat room, where
// the player will be able to wait until 7 players accumulate. Once this happens,
// it will cause the NPC "Bouncer" run the label "OnStart".

    waitingroom "Disco - Waiting Room",8,"Bouncer::OnStart",7;

// The NPC will have a box above its head, it will say "Party - Waiting Room"
// and will have 8 waiting slots. Clicking this will allow a player who has
// 5000 zeny and lvl 50~99 to enter the chat room, where the player will be
// able to wait until 7 players accumulate. Once this happens, it will cause
// the NPC "Bouncer" run the label "OnStart".

	waitingroom "Party - Waiting Room",8,"Bouncer::OnStart",7,5000,50,99;

Creating a waiting room does not stop the execution of the script and it will
continue to the next line.

For more examples see the 2-1 and 2-2 job quest scripts which make extensive use
of waiting rooms.

---------------------------------------

## delwaitingroom
*delwaitingroom {"<NPC object name"};

This command will delete a waiting room. If no parameter is given, it will
delete a waiting room attached to the NPC object running this command, if it is,
it will delete a waiting room owned by another NPC object. This is the only way
to get rid of a waiting room, nothing else will cause it to disappear.

It's not clear what happens to a waiting room if the NPC is disabled with
'disablenpc', by the way.

---------------------------------------

## enablewaitingroomevent
*enablewaitingroomevent {"<NPC object name>"};
## disablewaitingroomevent
*disablewaitingroomevent {"<NPC object name>"};
## enablearena
*enablearena;
## disablearena
*disablearena;

This will enable and disable triggering the waiting room event (see
'waitingroom') respectively. Optionally giving an NPC object name will do that
for a specified NPC object. The chat room will not disappear when triggering is
disabled and enabled in this manner and players will not be kicked out of it.
Enabling a chat room event will also cause it to immediately check whether the
number of users in it exceeded the trigger amount and trigger the event
accordingly.

Normally, whenever a waiting room was created to make sure that only one
character is, for example, trying to pass a job quest trial, and no other
characters are present in the room to mess up the script.

The 'enablearena'/'disablearena' commands are just aliases with no parameter.
These are supposedly left here for compatibility with official server scripts,
but no rAthena script uses these at the moment.

---------------------------------------

## getwaitingroomstate
*getwaitingroomstate(<information type>{,"<NPC object name>"})

This function will return information about the waiting room state for the
attached waiting room or for a waiting room attached to the specified NPC if
any.

The valid information types are:

 0  - Number of users currently chatting.
 1  - Maximum number of users allowed.
 2  - Will return 1 if the waiting room has a trigger set.
      0 otherwise.
 3  - Will return 1 if the waiting room is currently disabled.
      0 otherwise.
 4  - The Title of the waiting room (string)
 5  - Password of the waiting room, if any. Pointless, since there is no way to
      set a password on a waiting room right now.
 16 - Event name of the waiting room (string)
 32 - Whether or not the waiting room is full.
 33 - Whether the amount of users in the waiting room is higher than the trigger
      number.

---------------------------------------

## warpwaitingpc
*warpwaitingpc "<map name>",<x>,<y>{,<number of people>};

This command will warp the amount of characters equal to the trigger number of
the waiting room chat attached to the NPC object running this command to the
specified map and coordinates, kicking them out of the chat. Those waiting the
longest will get warped first. It can also do a random warp on the same map
("Random" instead of map name) and warp to the save point ("SavePoint").

The list of characters to warp is taken from the list of the chat room members.
Those not in the chat room will not be considered even if they are talking to
the NPC in question. If the number of people is given, exactly this much people
will be warped.

This command can also keep track of who just got warped. It does this by setting
special variables:

$@warpwaitingpc[] is an array containing the account_id numbers of the
                  characters who were just warped.
$@warpwaitingpcnum contains the number of the character it just warped.

See also 'getpartymember' for advice on what to do with those variables.

The obvious way of using this effectively would be to set up a waiting room for
two characters to be warped onto a random PVP map for a one-on-one duel, for
example.

---------------------------------------

## waitingroomkick
*waitingroomkick "<NPC object name>" , "<character name>";

This command kicks the given character from the waiting room attached to the given NPC.

---------------------------------------

## getwaitingroomusers
*getwaitingroomusers "<NPC object name>";

This command get all the characters in the waiting room of the given NPC and stores
their gids in the array .@waitingroom_users[]. Also, stores the number of characters
in the variable .@waitingroom_usercount.

---------------------------------------

## kickwaitingroomall
*kickwaitingroomall {"<NPC object name>"};

This command kicks everybody out of a specified waiting room chat.

---------------------------------------

## setmapflagnosave
*setmapflagnosave "<map name>","<alternate map name>",<x>,<y>;

This command sets the 'nosave' flag for the specified map and also gives an
alternate respawn-upon-relogin point.

It does not make a map impossible to make a save point on as you would normally
think, 'savepoint' will still work. It will, however, make the specified map
kick the reconnecting players off to the alternate map given to the coordinates
specified.

---------------------------------------

## setmapflag
*setmapflag "<map name>",<flag>{,<zone>{,<type>}};

This command marks a specified map with the given map flag, which will alter the
behavior of the map. A full list of mapflags is located in 'src/map/script_constants.hpp' with
the 'mf_' prefix, and documentation can be found in 'doc/mapflags.txt'.

The map flags alter the behavior of the map regarding teleporting (mf_nomemo,
mf_noteleport, mf_nowarp, mf_nogo), storing location when disconnected
(mf_nosave), dead branch usage (mf_nobranch), penalties upon death
(mf_nopenalty, mf_nozenypenalty), PVP behavior (mf_pvp, mf_pvp_noparty,
mf_pvp_noguild), WoE behavior (mf_gvg,mf_gvg_noparty), ability to use
skills or open up trade deals (mf_notrade, mf_novending, mf_noskill, mf_noicewall),
current weather effects (mf_snow, mf_fog, mf_sakura, mf_leaves, mf_rain, mf_clouds,
mf_fireworks) and whether night will be in effect on this map (mf_nightenabled).

The optional parameter <zone> is used to set the zone for 'restricted' mapflags,
GM level bypass for 'nocommand', base/job experience for 'bexp'/'jexp', and
flag for 'battleground'.

For 'skill_damage' mapflag:
	- Setting the flag here will adjust the global (all skills) damage on the map.
	- <zone> is the -100 to 100000 damage adjustment value of the skills.
	- See 'getmapflag' for the different <type> values.
For 'skill_duration' mapflag:
	- <zone> is the skill ID to adjust.
	- <type> is the percentage of adjustment from 0 to 100000.

---------------------------------------

## removemapflag
*removemapflag "<map name>",<flag>{,<zone>};

This command removes a mapflag from a specified map.
See 'setmapflag' for a list of mapflags.

The optional parameter 'zone' is used to remove the zone from restricted mapflags.

---------------------------------------

## getmapflag
*getmapflag("<map name>",<flag>{,<type>})

This command checks the status of a given mapflag and returns the mapflag's state.
0 means OFF, and 1 means ON. See 'setmapflag' for a list of mapflags.

For MF_RESTRICTED, the zone value of the map is returned.

The optional parameter 'type' is used in the 'skill_damage' mapflag:
 SKILLDMG_MAX: if mapflag is set (default)
 SKILLDMG_PC: damage against players
 SKILLDMG_MOB: damage against mobs
 SKILLDMG_BOSS: damage against bosses
 SKILLDMG_OTHER: damage against other
 SKILLDMG_CASTER: caster type

---------------------------------------

## setbattleflag
*setbattleflag "<battle flag>",<value>{,<reload>};
## getbattleflag
*getbattleflag("<battle flag>")

Sets or gets the value of the given battle flag.
Battle flags are the flags found in the battle / *.conf files and is also used in Lupus' variable rates script.
If the reload value is given then the server will attempt to reload monster data
to properly apply the new rates. This applies to EXP/Drop type configs. The server
will only attempt to reload specific configs.

Examples:

// Will set the base experience rate to 20x (2000%) - Monster data will continue to use previous rates at server start
	setBattleFlag "base_exp_rate",2000;

// Will set the base experience rate to 20x (2000%) - Monster data will be reloaded to new value
	setBattleFlag "base_exp_rate",2000,true;

// Will return the value of the base experience rate (when used after the above example, it would print 2000).
	mes getBattleFlag("base_exp_rate");

---------------------------------------

## warpportal
*warpportal <source x>,<source y>,"<map name>",<target x>,<target y>;

Creates a warp portal identical to the Acolyte "Warp Portal" skill.
The source coordinates specify the portal's location on the map of the invoking NPC.
The target map and coordinates determine the destination of the portal.

Examples:

// Will create a warp portal on the NPC's map at 150,150 leading to prontera, coords 150,180.
	warpportal 150,150,"prontera",150,180;

---------------------------------------

## mapwarp
*mapwarp "<from map>","<to map>",<x>,<y>{,<type>,<ID>};

This command will collect all characters located on the From map and warp them
wholesale to the same point on the To map, or randomly distribute them there if
the coordinates are zero. "Random" is understood as a special To map name and
will mean randomly shuffling everyone on the same map.

Optionally, a type and ID can be specified. Available types are:

 0 - Everyone
 1 - Guild
 2 - Party

Example:

// Will warp all members of guild with ID 63 on map prontera to map alberta.
	mapwarp "prontera","alberta",150,150,1,63;

---------------------------------------
\\
5,2.- Guild-related commands
\\
---------------------------------------

## maprespawnguildid
*maprespawnguildid "<map name>",<guild id>,<flag>;

This command goes through the specified map and for each player and monster
found there does stuff.

Flag is a bit-mask (add up numbers to get effects you want)
 1 - warp all guild members to their save points.
 2 - warp all non-guild members (including guildless players) to their save points.
 4 - remove all monsters which are not guardian or Emperium.

Flag 7 will, therefore, mean 'wipe all mobs but guardians and the Emperium and
kick all characters out', which is what the official scripts do upon castle
surrender. Upon start of WoE, the scripts do 2 (warp all intruders out).

For examples, check the WoE scripts in the distribution.

---------------------------------------

## agitstart
*agitstart;
## agitend
*agitend;
## agitstart2
*agitstart2;
## agitend2
*agitend2;
## agitstart3
*agitstart3;
## agitend3
*agitend3;

These commands will start and end War of Emperium FE, War of Emperium SE,
or War of Emperium TE.

This is a bit more complex than it sounds, since the commands themselves won't
actually do anything interesting, except causing all 'OnAgitStart:' and
'OnAgitEnd:', 'OnAgitStart2:' and 'OnAgitEnd2:', or 'OnAgitStart3:' and
'OnAgitEnd3:' in the case of latter two commands, events to run everywhere,
respectively. They are used as  simple triggers to run a lot of complex scripts
all across the server, and they, in turn, are triggered by clock with an
'OnClock<time>:' time-triggering label.

---------------------------------------

## gvgon
*gvgon "<map name>";
## gvgoff
*gvgoff "<map name>";

These commands will turn GVG mode for the specified maps on and off, setting up
appropriate map flags. In GVG mode, maps behave as if during the time of WoE,
even though WoE itself may or may not actually be in effect.

---------------------------------------

## gvgon3
*gvgon3 "<map name>";
## gvgoff3
*gvgoff3 "<map name>";

Theses commands behave identically to gvgon/gvgoff, but apply GVG_TE mapflag.

---------------------------------------

## flagemblem
*flagemblem <guild id>;

This command only works when run by the NPC objects which have sprite id 722,
which is a 3D guild flag sprite. If it isn't, the data will change, but nothing
will be seen by anyone. If it is invoked in that manner, the emblem of the
specified guild will appear on the flag, though, if any players are watching it
at this moment, they will not see the emblem change until they move out of sight
of the flag and return.

This is commonly used in official guildwar scripts with a function call which
returns a guild id:

// This will change the emblem on the flag to that of the guild that owns
// "guildcastle"

    flagemblem GetCastleData("guildcastle",1);

---------------------------------------

## guardian
*guardian "<map name>",<x>,<y>,"<name to show>",<mob id>{,"<event label>"{,<guardian index>}};

This command is roughly equivalent to 'monster', but is meant to be used with
castle guardian monsters and will only work with them. It will set the guardian
characteristics up according to the castle's investment values and otherwise
set the things up that only castle guardians need.

Since trunk r12524:
Returns the id of the mob or 0 if an error occurred.
When 'guardian index' isn't supplied it produces a temporary guardian.
Temporary guardians are not saved with the castle and can't be accessed by guardianinfo.

---------------------------------------

## guardianinfo
*guardianinfo("<map name>", <guardian number>, <type>);

This function will return various info about the specified guardian, or -1
if it fails for some reason. It is primarily used in the castle manager NPC.

Map name and guardian number (value between 0 and 7) define the target.
Type indicates what information to return:
 0 - visibility (whether the guardian is installed or not)
 1 - max. hp
 2 - current hp

---------------------------------------

## getguildalliance
*getguildalliance(<guild id1>, <guild id2>);

This command will return the relation between 2 guilds.

NOTE: This should be used in collaboration with 'requestguildinfo' as the
map-server needs to request for information from the char-server.

Return values:
	-2 - Guild ID1 does not exist
	-1 - Guild ID2 does not exist
	 0 - Both guilds have no relation OR guild ID aren't given
	 1 - Both guilds are allies
	 2 - Both guilds are antagonists

---------------------------------------
//
5,2.- End of guild-related commands
//
---------------------------------------

## npcspeed
*npcspeed <speed value>;
## npcwalkto
*npcwalkto <x>,<y>;
## npcstop
*npcstop;

These commands will make the NPC object in question move around the map. As they
currently are, they are a bit buggy and are not useful for much more than making
an NPC move randomly around the map.

'npcspeed' will set the NPCs walking speed to a specified value. As in the
@speed GM command, 200 is the slowest possible speed while 0 is the fastest
possible (instant motion). 100 is the default character walking speed.

'npcwalkto' will start the NPC sprite moving towards the specified coordinates
on the same map it is currently on. The script proceeds immediately after the
NPC begins moving.

'npcstop' will stop the motion.

While in transit, the NPC will be clickable, but invoking it will cause it to
stop moving, which will make its coordinates different from what the client
computed based on the speed and motion coordinates. The effect is rather
unnerving.

Only a few NPC sprites have walking animations, and those that do, do not get
the animation invoked when moving the NPC, due to the problem in the NPC walking
code, which looks a bit silly. You might have better success by defining a job-
sprite based sprite id in 'db/mob_avail.txt' with this.

---------------------------------------

## movenpc
*movenpc "<NPC name>",<x>,<y>{,<dir>};

This command looks like the NPCWalkToxy function,but is a little different.

While NPCWalkToXY just makes the NPC 'walk' to the coordinates given (which
sometimes gives problems if the path isn't a straight line without objects),
this command just moves the NPC. It basically warps out and in on the current
and given spot. Direction can be used to change the NPC's facing direction.

Example:

// This will move Bugga from it's old coordinates to the new coordinates at 100,20 (if those coordinates are legit).
	moveNPC "Bugga",100,20;

---------------------------------------

# 6. Other commands.

## debugmes
*debugmes "<message>";

This command will send the message to the server console (map-server window). It
will not be displayed anywhere else.

    // Displays "NAME has clicked me!" in the map-server window.
    debugmes strcharinfo(0) + " has clicked me!";

---------------------------------------

## logmes
*logmes "<message>";

This command will write the message given to the map server NPC log file, as
specified in 'conf/log_athena.conf'. In the TXT version of the server, the log
file is 'log/npclog.log' by default. In the SQL version, if SQL logging is
enabled, the message will go to the 'npclog' table, otherwise, it will go to the
same log file.

If logs are not enabled, nothing will happen.

---------------------------------------

## globalmes
*globalmes "<message>"{,"<NPC name>"};

This command will send a message to the chat window of all currently connected
characters.

If NPC name is specified, the message will be sent as if the sender would be
the NPC with the said name.
The display name of the NPC won't get appended in front of the message.

---------------------------------------

## rand
*rand(<number>{,<number>});

This function returns a number ...
(if you specify one) ... randomly positioned between 0 and the number you specify -1.
(if you specify two) ... randomly positioned between the two numbers you specify.

rand(10)  would result in 0,1,2,3,4,5,6,7,8 or 9
rand(0,9) would result in 0,1,2,3,4,5,6,7,8 or 9
rand(2,5) would result in 2,3,4 or 5

---------------------------------------

## viewpoint
*viewpoint <action>,<x>,<y>,<point number>,<color>{,<Char ID>};

This command will mark places on the mini map in the client connected to the
invoking character. It uses the normal X and Y coordinates from the main map.
The colors of the marks are defined using a hexadecimal number, same as the ones
used to color text in 'mes' output, but are written as hexadecimal numbers in C.
(They look like 0x<six numbers>.)

Action is what you want to do with a point, 1 will set it, while 2 will clear
it. 0 will also set it, but automatically removes the point after 15 seconds.
Point number is the number of the point - you can have several. If more than
one point is drawn at the same coordinates, they will cycle, which can be used
to create flashing marks.

	// This command will show a mark at coordinates X 30 Y 40, is mark number 1,
	// and will be red.

	viewpoint 1,30,40,1,0xFF0000;

This will create three points:

	viewpoint 1,30,40,1,0xFF0000;
	viewpoint 1,35,45,2,0xFF0000;
	viewpoint 1,40,50,3,0xFF0000;

And this is how you remove them:

	viewpoint 2,30,40,1,0xFF0000;
	viewpoint 2,35,45,2,0xFF0000;
	viewpoint 2,40,50,3,0xFF0000;

The client determines what it does with the points entirely, the server keeps no
memory of where the points are set whatsoever.

---------------------------------------

## cutin
*cutin "<filename>",<position>;

This command will display a picture, usually an NPC illustration, also called
cutin, for the currently attached client. The position parameter determines the
placement of the illustration and takes following values:

	0	bottom left corner
	1	bottom middle
	2	bottom right corner
	3	middle of screen in a movable window with an empty title bar
	4	middle of screen without the window header, but still movable
	255	clear all displayed cutins

The picture is read from data\texture\유저인터페이스\illust, from both the GRF archive
and data folder, and is required to be a bitmap. The file extension .bmp can be
omitted. Magenta color (#ff00ff) is considered transparent. There is no limit
placed on the size of the illustrations by the client, although loading of large
pictures (about 700x700 and larger) causes the client to freeze shortly (lag).
Typically the size is about 320x480. New illustrations can be added by just
putting the new file into the location above.

The client is able to display only one cutin at the same time and each new one
will cause the old one to disappear. To delete the currently displayed
illustration without displaying a new one, an empty file name and position 255
must be used.

	// Displays the Comodo Kafra illustration in lower right corner.
	cutin "kafra_07",2;

	// Typical way to end a script, which displayed an illustration during a
	// dialog with a player.
	mes "See you.";
	close2;
	cutin "",255;
	end;

---------------------------------------

## emotion
*emotion <emotion number>{,<target>};

This command makes an object display an emotion sprite above their own as
if they were doing that emotion. For a full list of emotion numbers,
see 'src/map/script_constants.hpp' under 'ET_'. The not so obvious ones are 'ET_QUESTION'
(a question mark) and 'ET_SURPRISE' (the exclamation mark).

The optional target parameter specifies who will get the emotion on top of
their head. Use the target Game ID (GID).

---------------------------------------

## misceffect
*misceffect <effect number>;

This command, if run from an NPC object that has a sprite, will call up a
specified effect number, centered on the NPC sprite. If the running code does
not have an object ID (a 'floating' NPC) or is not running from an NPC object at
all (an item script) the effect will be centered on the character who's RID got
attached to the script, if any. For usable item scripts, this command will
create an effect centered on the player using the item.

A full list of known effects is found in 'doc/effect_list.txt'. The list of
those that actually work may differ greatly between client versions.

---------------------------------------

## soundeffect
*soundeffect "<effect filename>",<type>;
## soundeffectall
*soundeffectall "<effect filename>",<type>{,"<map name>"}{,<x0>,<y0>,<x1>,<y1>};

These two commands will play a sound effect to either the invoking character
only ('soundeffect') or multiple characters ('soundeffectall'). If the running
code does not have an object ID (a 'floating' NPC) or is not running from an NPC
object at all (an item script) the sound will be centered on the character who's
RID got attached to the script, if any. If it does, it will be centered on that
object. (an NPC sprite)

Effect filename is the filename in a GRF. It must have the .wav extension.

It's not quite certain what the 'type' actually does, it is sent to the client
directly. It probably determines which directory to play the effect from.
It's certain that giving 0 for the number will play sound files from '\data\wav\',
but where the other numbers will read from is unclear.

The sound files themselves must be in the PCM format, and file names should also
have a maximum length of 23 characters including the .wav extension:

soundeffect "1234567890123456789.wav", 0; // this will play the soundeffect
soundeffect "12345678901234567890.wav", 0; // throw gravity error

You can add your own effects this way, naturally.

---------------------------------------

## playBGM
*playBGM "<BGM filename>";
## playBGMall
*playBGMall "<BGM filename>"{,"<map name>"{,<x0>,<y0>,<x1>,<y1>}};

These two commands will play a Background Music to either the invoking character
only ('playBGM') or multiple characters ('playBGMall').

BGM filename is the filename in /BGM/ folder. It has to be in .mp3 extension.

It's not required to specify the extension inside the script.
If coordinates are omitted, BGM will be broadcasted on the entire map. If the map name
is omitted as well the BGM will be played for the entire server.

You can add your own BGMs this way, naturally.

---------------------------------------

## pvpon
*pvpon "<map name>";
## pvpoff
*pvpoff "<map name>";

These commands will turn PVP mode for the specified maps on and off. Beside
setting the flags referred to in 'setmapflag', 'pvpon' will also create a PVP
timer and ranking as will @pvpon GM command do.

---------------------------------------

## atcommand
*atcommand "<command>";

This command will run the given command line exactly as if it was typed in from
the keyboard by the player connected to the invoking character, and that
character belonged to an account which had GM level 99.

	// This will ask the invoker for a character name and then use the '@nuke'
	// GM command on them, killing them mercilessly.
	input .@player$;
	atcommand "@nuke " + .@player$;

Note that for atcommands bound using 'bindatcmd', this command will execute the
original atcommand, not the script-bound atcommand.

---------------------------------------

## charcommand
*charcommand "<command>";

This command will run the given command line exactly as if it was typed in from
the keyboard from a character that belonged to an account which had GM level 99.

The commands can also run without an attached rid.

	// This would do the same as above, but now
	// it doesn't need a player attached by default.
	charcommand "#option 0 0 0 Roy";

---------------------------------------

## bindatcmd
*bindatcmd "<command>","<NPC object name>::<event label>"{,<atcommand level>,<charcommand level>};

This command will bind a NPC event label to an atcommand. Upon execution of the
atcommand, the user will invoke the NPC event label. Each atcommand is only allowed
one binding. If you rebind, it will override the original binding.
Note: The default level for atcommand is 0 while the default level for charcommand is 100.

The following variables are set upon execution:
	.@atcmd_command$       =  The name of the @command used.
	.@atcmd_parameters$[]  =  Array containing the given parameters, starting from an index of 0.
	.@atcmd_numparameters  =  The number of parameters defined.

Example:

	When a user types the command "@test", an angel effect will be shown.

	-	script	atcmd_example	-1,{
	OnInit:
		bindatcmd "test",strnpcinfo(3) + "::OnAtcommand";
		end;
	OnAtcommand:
		specialeffect2 EF_ANGEL2;
		end;
	}

---------------------------------------

## unbindatcmd
*unbindatcmd "<command>";

This command will unbind a NPC event label from an atcommand.

---------------------------------------

## useatcmd
*useatcmd "<command>";

This command will execute a script-bound atcommand for the attached RID. If the
supplied command is not bound to any script, this command will act like 'atcommand'
and attempt to execute a source-defined command.

The three .@atcmd_***** variables will NOT be set when invoking script-bound atcommands
in this way.

---------------------------------------

## camerainfo
*camerainfo <range>,<rotation>,<latitude>{,<char id>};

This command will update the client's camera information with the given values where
the client can be the attached character or the player given by the char id parameter.
Note: This requires 2016-05-25aRagexeRE or newer.

The values given will be divided by 100 and transmitted as floating-point number.

	range		The zoomfactor of the camera.
				Default: 230000 (230.0) when fully zoomed in
				Maximum: 400000 (400.0) when fully zoomed out

	rotation	The rotation of the camera.
				Default: 0 (0.0) when no rotation is applied
				Maximum: 360000 (360.0°) when fully rotated

	latitude	The angle of the camera.
				Default: -50000 (-50.0)
				Maximum: -75000 (-75.0)

---------------------------------------
\\
6,1.- Unit-related commands
\\
---------------------------------------

## unitwalk
*unitwalk <GID>,<x>,<y>{,"<event label>"};
## unitwalkto
*unitwalkto <GID>,<Target GID>{,"<event label>"};

This command will tell a <GID> to walk to a position, defined either as a set of
coordinates or another object. The command returns a 1 for success and 0 upon failure.

If coordinates are passed, the <GID> will walk to the given x,y coordinates on the
unit's current map. While there is no way to move across an entire map with 1 command
use, this could be used in a loop to move long distances.

If an object ID is passed, the initial <GID> will walk to the <Target GID> (similar to
walking to attack). This is based on the distance from <GID> to <Target ID>. This command
uses a hard walk check, so it will calculate a walk path with obstacles. Sending a bad
target ID will result in an error.

An optional Event Label can be passed as well which will execute when the <GID> has reached
the given coordinates or <Target GID>.

Examples:

// Makes player walk to the coordinates (150,150).
	unitwalk getcharid(3),150,150;

// Performs a conditional check with the command and reports success or failure to the player.
	if (unitwalk(getcharid(3),150,150))
		dispbottom "Walking you there...";
	else
		dispbottom "That's too far away, man.";

// Makes player walk to another character named "WalkToMe".
	unitwalkto getcharid(3),getcharid(3,"WalkToMe");

---------------------------------------

## unitattack
*unitattack <GID>,<Target ID>{,<action type>};
## unitattack
*unitattack <GID>,"<Target Name>"{,<action type>};

This command will make a <GID> attack the specified target. It returns true upon
success and false for all failures.

If <GID> is a player and a non-zero <action type> is given, the unit will perform a
continuous attack instead of a single attack.

Note:
Using unitattack with <GID> 0 means that it will use the currently attached unit.
For players any attack requests will fail, because talking to an NPC prevents attacking a monster.
Therefore you need to detach the player from the NPC before using this command.

---------------------------------------

## unitkill
*unitkill <GID>;

This command will kill a <GID>.

---------------------------------------

## unitwarp
*unitwarp <GID>,"<map name>",<x>,<y>;

This command will warp a <GID> to the specified map and coordinates.

If <GID> is zero, the command runs for the unit that invoked the script. This can be
used with "OnTouch" to warp monsters:

OnTouch:
	unitwarp 0,"this",-1,-1;

---------------------------------------

## unitstopattack
*unitstopattack <GID>;

This command will make a <GID> stop attacking.

---------------------------------------

## unitstopwalk
*unitstopwalk <GID>{,<flag>};

This command will make a <GID> stop moving.

Note: If this is called from OnTouch, then the walktimer attached to the unit is
removed from OnTouch which causes this command to not stop the unit from walking.
Suggest to use 'unitblockmove' to forcefully stop the unit with OnTouch.

The <flag> value affects how the unit is stopped. The following flags are bitwise
values (can be combined using the pipe operator):
	USW_NONE = Unit will keep walking to their original destination.
	USW_FIXPOS = Issue a fixpos packet afterwards.
	USW_MOVE_ONCE = Force the unit to move one cell if it hasn't yet.
	USW_MOVE_FULL_CELL = Enable moving to the next cell when unit was already half-way there (may cause on-touch/place side-effects, such as a scripted map change).
	USW_FORCE_STOP = Force stop moving.

---------------------------------------

## unittalk
*unittalk <GID>,"<text>"{,flag};

This command will make a <GID> say a message. The display name of the <GID> won't get appended in front of the message.
flag: Specify target
   bc_area - Message is sent to players in the vicinity of the source (default).
   bc_self - Message is sent only to player attached.

---------------------------------------

## unitskilluseid
*unitskilluseid <GID>,<skill id>,<skill lvl>{,<target id>,<casttime>};
## unitskilluseid
*unitskilluseid <GID>,"<skill name>",<skill lvl>{,<target id>,<casttime>};
## unitskillusepos
*unitskillusepos <GID>,<skill id>,<skill lvl>,<x>,<y>{,<casttime>};
## unitskillusepos
*unitskillusepos <GID>,"<skill name>",<skill lvl>,<x>,<y>{,<casttime>};

This is the replacement of the older commands, these use the same values for
GID as the other unit* commands (See 'GID').

Skill ID is the ID of the skill, skill level is the level of the skill.
Cast time is the amount of seconds to add or remove from the skill. Use a positive value to
add and negative value to subtract. Using 0 or no value will use the default skill cast time.
For the position, the x and y are given in the UnitSkillUsePos.

---------------------------------------

## unitexists
*unitexists <GID>;

Checks if the given Game ID exists. Returns false if the object doesn't exist, or true if
it does.

---------------------------------------

## getunittype
*getunittype <GID>;

Returns the type of object from the given Game ID. Returns -1 if the given GID does not
exist.

Return values:
	BL_PC   - Character object
	BL_MOB  - Monster object
	BL_PET  - Pet object
	BL_HOM  - Homunculus object
	BL_MER  - Mercenary object
	BL_NPC  - NPC object
	BL_ELEM - Elemental object

---------------------------------------

## getunitname
*getunitname <GID>;

Gets the name of the given unit. Supported types are monster, homunculus, pet, and NPC.
Mercenary and Elemental don't support custom names.

Returns "Unknown" if unit is not found.

---------------------------------------

## setunitname
*setunitname <GID>,"<new name>";

Changes the name of the given unit to the new name given. Supported types are monster,
homunculus, and pet. To change an NPC's name, see 'setnpcdisplay'. Mercenary and
Elemental don't support custom names.

Changing a homunculus or pet name will be permanent.

Returns "Unknown" if unit is not found.

---------------------------------------

## getunitdata
*getunitdata <GID>,<arrayname>;
## setunitdata
*setunitdata <GID>,<parameter>,<new value>;

This is used to get and set special data related to the unit.
With getunitdata, the array given will be filled with the current data. In setunitdata
the indexes in the array would be used to set that data on the unit.

Both getunitdata and setunitdata will return -1 if the given GID does not exist.

Note: When adjusting a unit's stat (STR, AGI, etc) the unit's respective statuses are
      recalculated (HIT, FLEE, etc) automatically. Keep in mind that some stats don't
	  affect a unit's status and will have to directly be modified.

Parameters (indexes) for monsters are:
	UMOB_SIZE
	UMOB_LEVEL
	UMOB_HP
	UMOB_MAXHP
	UMOB_MASTERAID
	UMOB_MAPID
	UMOB_X
	UMOB_Y
	UMOB_SPEED
	UMOB_MODE
	UMOB_AI
	UMOB_SCOPTION
	UMOB_SEX
	UMOB_CLASS
	UMOB_HAIRSTYLE
	UMOB_HAIRCOLOR
	UMOB_HEADBOTTOM
	UMOB_HEADMIDDLE
	UMOB_HEADTOP
	UMOB_CLOTHCOLOR
	UMOB_SHIELD
	UMOB_WEAPON
	UMOB_LOOKDIR
	UMOB_CANMOVETICK
	UMOB_STR
	UMOB_AGI
	UMOB_VIT
	UMOB_INT
	UMOB_DEX
	UMOB_LUK
	UMOB_SLAVECPYMSTRMD
	UMOB_DMGIMMUNE
	UMOB_ATKRANGE
	UMOB_ATKMIN
	UMOB_ATKMAX
	UMOB_MATKMIN
	UMOB_MATKMAX
	UMOB_DEF
	UMOB_MDEF
	UMOB_HIT
	UMOB_FLEE
	UMOB_PDODGE
	UMOB_CRIT
	UMOB_RACE
	UMOB_ELETYPE
	UMOB_ELELEVEL
	UMOB_AMOTION
	UMOB_ADELAY
	UMOB_DMOTION
	UMOB_TARGETID

-----

Parameter (indexes) for homunculi are:
	UHOM_SIZE
	UHOM_LEVEL
	UHOM_HP
	UHOM_MAXHP
	UHOM_SP
	UHOM_MAXSP
	UHOM_MASTERCID
	UHOM_MAPID
	UHOM_X
	UHOM_Y
	UHOM_HUNGER
	UHOM_INTIMACY
	UHOM_SPEED
	UHOM_LOOKDIR
	UHOM_CANMOVETICK
	UHOM_STR
	UHOM_AGI
	UHOM_VIT
	UHOM_INT
	UHOM_DEX
	UHOM_LUK
	UHOM_DMGIMMUNE
	UHOM_ATKRANGE
	UHOM_ATKMIN
	UHOM_ATKMAX
	UHOM_MATKMIN
	UHOM_MATKMAX
	UHOM_DEF
	UHOM_MDEF
	UHOM_HIT
	UHOM_FLEE
	UHOM_PDODGE
	UHOM_CRIT
	UHOM_RACE
	UHOM_ELETYPE
	UHOM_ELELEVEL
	UHOM_AMOTION
	UHOM_ADELAY
	UHOM_DMOTION
	UHOM_TARGETID

-----

Parameter (indexes) for pets are:
	UPET_SIZE
	UPET_LEVEL
	UPET_HP
	UPET_MAXHP
	UPET_MASTERAID
	UPET_MAPID
	UPET_X
	UPET_Y
	UPET_HUNGER
	UPET_INTIMACY
	UPET_SPEED
	UPET_LOOKDIR
	UPET_CANMOVETICK
	UPET_STR
	UPET_AGI
	UPET_VIT
	UPET_INT
	UPET_DEX
	UPET_LUK
	UPET_DMGIMMUNE
	UPET_ATKRANGE
	UPET_ATKMIN
	UPET_ATKMAX
	UPET_MATKMIN
	UPET_MATKMAX
	UPET_DEF
	UPET_MDEF
	UPET_HIT
	UPET_FLEE
	UPET_PDODGE
	UPET_CRIT
	UPET_RACE
	UPET_ELETYPE
	UPET_ELELEVEL
	UPET_AMOTION
	UPET_ADELAY
	UPET_DMOTION

-----

Parameter (indexes) for mercenaries are:
	UMER_SIZE
	UMER_HP
	UMER_MAXHP
	UMER_MASTERCID
	UMER_MAPID
	UMER_X
	UMER_Y
	UMER_KILLCOUNT
	UMER_LIFETIME
	UMER_SPEED
	UMER_LOOKDIR
	UMER_CANMOVETICK
	UMER_STR
	UMER_AGI
	UMER_VIT
	UMER_INT
	UMER_DEX
	UMER_LUK
	UMER_DMGIMMUNE
	UMER_ATKRANGE
	UMER_ATKMIN
	UMER_ATKMAX
	UMER_MATKMIN
	UMER_MATKMAX
	UMER_DEF
	UMER_MDEF
	UMER_HIT
	UMER_FLEE
	UMER_PDODGE
	UMER_CRIT
	UMER_RACE
	UMER_ELETYPE
	UMER_ELELEVEL
	UMER_AMOTION
	UMER_ADELAY
	UMER_DMOTION
	UMER_TARGETID

-----

Parameter (indexes) for elementals are:
	UELE_SIZE
	UELE_HP
	UELE_MAXHP
	UELE_SP
	UELE_MAXSP
	UELE_MASTERCID
	UELE_MAPID
	UELE_X
	UELE_Y
	UELE_LIFETIME
	UELE_MODE
	UELE_SPEED
	UELE_LOOKDIR
	UELE_CANMOVETICK
	UELE_STR
	UELE_AGI
	UELE_VIT
	UELE_INT
	UELE_DEX
	UELE_LUK
	UELE_DMGIMMUNE
	UELE_ATKRANGE
	UELE_ATKMIN
	UELE_ATKMAX
	UELE_MATKMIN
	UELE_MATKMAX
	UELE_DEF
	UELE_MDEF
	UELE_HIT
	UELE_FLEE
	UELE_PDODGE
	UELE_CRIT
	UELE_RACE
	UELE_ELETYPE
	UELE_ELELEVEL
	UELE_AMOTION
	UELE_ADELAY
	UELE_DMOTION
	UELE_TARGETID

-----

Parameter (indexes) for NPCs are:
	UNPC_DISPLAY
	UNPC_LEVEL
	UNPC_HP
	UNPC_MAXHP
	UNPC_MAPID
	UNPC_X
	UNPC_Y
	UNPC_LOOKDIR
	UNPC_STR
	UNPC_AGI
	UNPC_VIT
	UNPC_INT
	UNPC_DEX
	UNPC_LUK
	UNPC_PLUSALLSTAT
	UNPC_DMGIMMUNE
	UNPC_ATKRANGE
	UNPC_ATKMIN
	UNPC_ATKMAX
	UNPC_MATKMIN
	UNPC_MATKMAX
	UNPC_DEF
	UNPC_MDEF
	UNPC_HIT
	UNPC_FLEE
	UNPC_PDODGE
	UNPC_CRIT
	UNPC_RACE
	UNPC_ELETYPE
	UNPC_ELELEVEL
	UNPC_AMOTION
	UNPC_ADELAY
	UNPC_DMOTION

## Notes
*Notes:
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

Example:
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

---------------------------------------

## geteleminfo
*geteleminfo <type>{,<char_id>};

Get info of elemental of attached player or player by char_id.
Other info can be obtained by 'getunitdata' command.

Valid types are:
   0: Elemental ID
   1: Elemental Game ID

---------------------------------------
\\
6,1.- End of unit-related commands
\\
---------------------------------------

## npcskill
*npcskill <skill id>,<skill lvl>,<stat point>,<NPC level>;
## npcskill
*npcskill "<skill name>",<skill lvl>,<stat point>,<NPC level>;

This command causes the attached NPC object to cast a skill on the attached
player. The skill will have no cast time or cooldown. The player must be
within the default skill range or the command will fail silently.

The "stat point" parameter temporarily sets all NPC stats to the given value,
and "NPC level" is the temporary level of the NPC (used in some skills).
Neither value can be greater than the max level defined in config, and will
not work properly if the NPC has a mob sprite.

Before using skills, NPCs must have basic stats applied to them depending on the
skill being used: UNPC_ATKMIN, UNPC_ATKMAX, UNPC_MATKMIN, UNPC_MATKMAX, UNPC_STR,
UNPC_AGI, UNPC_VIT, UNPC_INT, UNPC_DEX, UNPC_LUK.
See 'setunitdata' for more information on usage.

    // Casts Level 10 Heal on the attached player, calculated with
    // all stats 99 and base level 60.
    npcskill "AL_HEAL",10,99,60;

---------------------------------------

## day
*day;
## night
*night;

These two commands will switch the entire server between day and night mode
respectively. If your server is set to cycle between day and night by
configuration, it will eventually return to that cycle.

Example:

-	script	DayNight	-1,{
OnClock0600:
	day;
	end;
OnInit:
	// setting correct mode upon server start-up
	if (gettime(DT_HOUR)>=6 && gettime(DT_HOUR)<18) end;
OnClock1800:
	night;
	end;
}

This script allows to emulate the day/night cycle as the server does, but also
allows triggering additional effects upon change, like announces, gifts, etc.
The day/night cycle set by configuration should be disabled when this script is used.

---------------------------------------

## defpattern
*defpattern <set number>,"<regular expression pattern>","<event label>";
## activatepset
*activatepset <set number>;
## deactivatepset
*deactivatepset <set number>;
## deletepset
*deletepset <set number>;

This set of commands is only available if the server is compiled with regular
expressions library enabled. Default compilation and most binary distributions
aren't, which is probably bad, since these, while complex to use, are quite
fascinating.

They will make the NPC object listen for text spoken publicly by players and
match it against regular expression patterns, then trigger labels associated
with these regular expression patterns.

Patterns are organized into sets, which are referred to by a set number. You can
have multiple sets patterns, and multiple patterns may be active at once.
Numbers for pattern sets start at 1.

'defpattern' will associate a given regular expression pattern with an event
label. This event will be triggered whenever something a player says is matched
by this regular expression pattern, if the pattern is currently active.

'activatepset' will make the pattern set specified active. An active pattern
will enable triggering labels defined with 'defpattern', which will not happen
by default.
'deactivatepset' will deactivate a specified pattern set. Giving -1 as a pattern
set number in this case will deactivate all pattern sets defined.

'deletepset' will delete a pattern set from memory, so you can create a new
pattern set in its place.

Using regular expressions is high wizardry. But with this high wizardry comes
unparalleled power of text manipulation. For an explanation of what a regular
expression pattern is, see a few web pages:

http://www.regular-expressions.info/
http://www.weitz.de/regex-coach/

For an example of this in use, see doc/sample/npc_test_pcre.txt

With this you could, for example, automatically punish players for asking for
Zeny in public places, or alternatively, automatically give them Zeny instead if
they want it so much.

---------------------------------------

## pow
*pow(<number>,<power>)

Returns the result of the calculation.

Example:
	.@i = pow(2,3); // .@i will be 8

---------------------------------------

## sqrt
*sqrt(<number>)

Returns the square-root of a number.

Example:
	.@i = sqrt(25); // .@i will be 5

---------------------------------------

## distance
*distance(<x0>,<y0>,<x1>,<y1>)

Returns distance between 2 points.

Example:
	.@i = distance(100,200,101,202);

---------------------------------------

## min
*min(<number or array>{,<number or array>,...})
## minimum
*minimum(<number or array>{,<number or array>,...})
## max
*max(<number or array>{,<number or array>,...})
## maximum
*maximum(<number or array>{,<number or array>,...})

Returns the smallest (or biggest) from the set of given parameters.
These parameters have to be either numbers or number arrays.

Example:
	.@minimum = min( 1, -6, -2, 8, 2 ); // .@minimum will be equal to -6
	.@maximum = max( 0, 5, 10, 4 ); // .@maximum will be equal to 10
	.@level = min( BaseLevel, 70 ); // .@level will be the character's base level, capped to 70

	setarray .@testarray, 4, 5, 12, 6, 7, 3, 8, 9, 10;

	.@minimum = min( .@testarray ); // .@minimum will be equal to 3
	.@maximum = max( .@testarray ); // .@maximum will be equal to 12

	.@minimum = min( -6, 1, 2, 3, .@testarray ); // .@minimum will be equal to -6
	.@maximum = max( -6, 1, 2, 3, .@testarray ); // .@maximum will be equal to 12

---------------------------------------

## round
*round(<number>,<precision>);
## ceil
*ceil(<number>,<precision>);
## floor
*floor(<number>,<precision>);

Returns <number> rounded to multiple of <precision>.

`round` function will round the <number> up if its division with <precision> yield a remainder
with a value equals to or more than half of <precision>. Otherwise, it rounds the <number> down.
`ceil` always round the <number> up.
`floor` always round the <number> down.

---------------------------------------

## md5
*md5("<string>")

Returns the md5 checksum of a number or string.

Example:
	mes md5(12345);
	mes md5("12345"); 	// Will both display 827ccb0eea8a706c4c34a16891f84e7b
	mes md5("qwerty"); 	// Will display d8578edf8458ce06fbc5bb76a58c5ca4

---------------------------------------

## query_sql
*query_sql("your MySQL query"{, <array variable>{, <array variable>{, ...}}});
## query_logsql
*query_logsql("your MySQL query"{, <array variable>{, <array variable>{, ...}}});

Executes an SQL query. A 'select' query can fill array variables with up to 2 billion rows of
values, and will return the number of rows (i.e. array size) or -1 on failure.

Note that 'query_sql' runs on the main database while 'query_logsql' runs on the log database.

Example:
	.@nb = query_sql("select name,fame from `char` ORDER BY fame DESC LIMIT 5", .@name$, .@fame);
	mes "Hall Of Fame: TOP5";
	mes "1." + .@name$[0] + "(" + .@fame[0] + ")"; // largest fame value.
	mes "2." + .@name$[1] + "(" + .@fame[1] + ")";
	mes "3." + .@name$[2] + "(" + .@fame[2] + ")";
	mes "4." + .@name$[3] + "(" + .@fame[3] + ")";
	mes "5." + .@name$[4] + "(" + .@fame[4] + ")";

---------------------------------------

## escape_sql
*escape_sql(<value>)

Converts the value to a string and escapes special characters so that it is safe to
use in query_sql(). Returns the escaped form of the given value.

Example:
	.@name$ = "John's Laptop";
	.@esc_str$ = escape_sql(.@name$); // Escaped string: John\'s Laptop

---------------------------------------

## setiteminfo
*setiteminfo(<item id>,<type>,<value>)

This function will set some value of an item.
Returns the new value on success, or -1 on fail (item_id not found or invalid type).

Valid types are:
	0 - Buy Price; 1 - Sell Price; 2 - Item Type;
	3 - maxchance (Max drop chance of this item e.g. 1 = 0.01% , etc..
		if = 0, then monsters don't drop it at all (rare or a quest item)
		if = 10000, then this item is sold in NPC shops only
	4 - sex; 5 - equip; 6 - weight; 7 - atk; 8 - def; 9 - range;
	10 - slot; 11 - look; 12 - elv; 13 - wlv; 14 - view id

Example:
	setiteminfo 7049,6,9990; // Stone now weighs 999.0

---------------------------------------

## setitemscript
*setitemscript(<item id>,<"{ new item script }">{,<type>});

Set a new script bonus to the Item. Very useful for game events.
You can remove an item's itemscript by leaving the itemscript argument empty.
Returns 1 on success, or 0 on fail (item_id not found or new item script is invalid).
Type can optionally be used indicates which script to set (default is 0):
 0 - Script
 1 - OnEquip_Script
 2 - OnUnequip_Script

Example:
	setitemscript 2637,"{ if (isequipped(2236) == 0)end; if (getskilllv(26)){skill 40,1;}else{skill 26,1+isequipped(2636);} }";
	setitemscript 2637,"";

---------------------------------------

## atoi
*atoi("<string>")
## axtoi
*axtoi("<string>")
## strtol
*strtol("<string>", base)

These commands are used to convert strings to numbers. 'atoi' will interpret
given string as a decimal number (base 10), while 'axtoi' interprets strings as
hexadecimal numbers (base 16). 'strtol' lets the user specify a base (valid range
is between 2 and 36 inclusive, or the special value0, which means auto-detection).

The 'atoi' and 'strtol' functions conform to the C functions with the same names,
and 'axtoi' is the same as strtol, with a base of 16. Results are clamped to signed
32 bit int range (INT_MIN ~ INT_MAX).

Examples:

	.@var = atoi("11");        // Sets .@var to 11
	.@var = axtoi("FF");       // Sets .@var to 255
	mes axtoi("11");           // Displays 17 (1 = 1, 10 = 16)
	.@var = strtol("11", 10);  // Sets .@var to 11 (11 base 10)
	.@var = strtol("11", 16);  // Sets .@var to 17 (11 base 16)
	.@var = strtol("11", 0);   // Sets .@var to 11 (11 base 10, auto-detected)
	.@var = strtol("0x11", 0); // Sets .@var to 17 (11 base 16, auto-detected because of the "0x" prefix)
	.@var = strtol("011", 0);  // Sets .@var to 9 (11 base 8, auto-detected because of the "0" prefix)
	.@var = strtol("11", 2);   // Sets .@var to 3 (binary 11)

---------------------------------------

## compare
*compare("<string>","<substring>")

This command returns 1 or 0 when the substring is in the main string (1) or not (0).
This command is not case sensitive.

Examples:
	//dothis; will be executed ('Bloody Murderer' contains 'Blood').
	if (compare("Bloody Murderer","Blood"))
		dothis;

	//dothat; will not be executed ('Blood butterfly' does not contain 'Bloody').
	if (compare("Blood Butterfly","Bloody"))
		dothat;

---------------------------------------

## strcmp
*strcmp("<string>","<string>")

This command compares two strings are returns a value:
   1: string 1 > string 2
   0: strings are equal
  -1: string 1 < string 2

---------------------------------------

## getstrlen
*getstrlen("<string>")

This function will return the length of the string given as an argument. It is
useful to check if anything input by the player exceeds name length limits and
other length limits and asking them to try to input something else.

---------------------------------------

## charisalpha
*charisalpha("<string>",<position>)

This function will return 1 if the character number Position in the given string
is a letter, 0 if it isn't a letter but a digit or a space.
The first letter is position 0.

---------------------------------------

## charat
*charat(<string>,<index>)

Returns char at specified index. If index is out of range, returns empty string.
The first letter of a string is index 0.

Example:
	charat("This is a string", 10); //returns "s"

---------------------------------------

## setchar
*setchar(<string>,<char>,<index>)

Returns the original string with the char at the specified index set to the
specified char. If index out of range, the original string will be returned.
Only the 1st char in the <char> parameter will be used.

Example:
	setchar("Cat", "B", 0); //returns "Bat"

---------------------------------------

## insertchar
*insertchar(<string>,<char>,<index>)

Returns the original string with the specified char inserted at the specified
index. If index is out of range, the char will be inserted on the end of the
string that it is closest. Only the 1st char in the <char> parameter will be used.

Example:
	insertchar("laughter", "s", 0); //returns "slaughter"

---------------------------------------

## delchar
*delchar(<string>,<index>)

Returns the original string with the char at the specified index removed.
If index is out of range, original string will be returned.

Example:
	delchar("Diet", 3); //returns "Die"

---------------------------------------

## strtoupper
*strtoupper(<string>)
## strtolower
*strtolower(<string>)

Returns the specified string in its uppercase/lowercase form.
All non-alpha characters will be preserved.

Example:
	strtoupper("The duck is blue!!"); //returns "THE DUCK IS BLUE!!"

---------------------------------------

## charisupper
*charisupper(<string>,<index>)
## charislower
*charislower(<string>,<index>)

Returns 1 if character at specified index of specified string is
uppercase/lowercase. Otherwise, 0. Characters not of the alphabet will return 0.

Example:
	charisupper("rAthena", 1); //returns 1

---------------------------------------

## substr
*substr(<string>,<start_index>,<end_index>)

Returns the sub-string of the specified string inclusively between the set
indexes. If indexes are out of range, or the start index is after the end
index, an empty string will be returned.

Example:
	substr("foobar", 3, 5); //returns "bar"

---------------------------------------

## explode
*explode(<dest_array>,<string>,<delimiter>)

Breaks a string up into substrings based on the specified delimiter. Substrings
will be stored within the specified string array. Only the 1st char of the
delimiter parameter will be used. If an empty string is passed as a delimiter,
the string will be placed in the array in its original form.

Example:
	explode(.@my_array$, "Explode:Test:1965:red:PIE", ":");
	//.@my_array$ contents will be...
	//.@my_array$[0]: "Explode"
	//.@my_array$[1]: "Test"
	//.@my_array$[2]: "1965"
	//.@my_array$[3]: "red"
	//.@my_array$[4]: "PIE"

---------------------------------------

## implode
*implode(<string_array>{,<glue>})

Combines all substrings within the specified string array into a single string.
If the glue parameter is specified, it will be inserted inbetween each substring.

Example:
	setarray .@my_array$[0], "This", "is", "a", "test";
	implode(.@my_array$, " "); //returns "This is a test"

---------------------------------------

## sprintf
*sprintf(<format>[,param[,param[,...]]])

C style sprintf. The resulting string is returned same as in PHP. All C format
specifiers are supported except %n. More info: sprintf @ www.cplusplus.com.
The number of params is only limited by rA's script engine.

Example:
	.@format$ = "The %s contains %d monkeys";
	dispbottom(sprintf(.@format$, "zoo", 5));        //prints "The zoo contains 5 monkeys"
	dispbottom(sprintf(.@format$, "barrel", 82));    //prints "The barrel contains 82 monkeys"

---------------------------------------

## sscanf
*sscanf(<string>,<format>[,param[,param[,...]]])

C style sscanf. All C format specifiers are supported.
More info: sscanf @ www.cplusplus.com. The number of params is only limited
by rA's script engine.

Example:
	sscanf("This is a test: 42 foobar", "This is a test: %d %s", .@num, .@str$);
	dispbottom(.@num + " " + .@str$); //prints "42 foobar"

---------------------------------------

## strpos
*strpos(<haystack>,<needle>{,<offset>})

PHP style strpos. Finds a substring (needle) within a string (haystack).
The offset parameter indicates the index of the string to start searching.
Returns index of substring on successful search, else -1.
Comparison is case sensitive.

Example:
	strpos("foobar", "bar", 0); //returns 3
	strpos("foobarfoo", "foo", 0); //returns 0
	strpos("foobarfoo", "foo", 1); //returns 6

---------------------------------------

## replacestr
*replacestr(<input>, <search>, <replace>{, <usecase>{, <count>}})

Replaces all instances of a search string in the input with the specified
replacement string. By default is case sensitive unless <usecase> is set
to 0. If specified it will only replace as many instances as specified
in the count parameter.

Example:
	replacestr("testing tester", "test", "dash"); //returns "dashing dasher"
	replacestr("Donkey", "don", "mon", 0); //returns "monkey"
	replacestr("test test test test test", "test", "yay", 0, 3); //returns "yay yay yay test test"

---------------------------------------

## countstr
*countstr(<input>, <search>{, <usecase>})

Counts all instances of a search string in the input. By default is case
sensitive unless <usecase> is set to 0.

Example:
	countstr("test test test Test", "test"); //returns 3
	countstr("cake Cake", "Cake", 0); //returns 2

---------------------------------------

## preg_match
*preg_match(<regular expression pattern>,<string>{,<offset>})

Searches a string for a match to the regular expression provided. The
offset parameter indicates the index of the string to start searching.
Returns offsets to captured substrings, or 0 if no match is found.

This command is only available if the server is compiled with the regular
expressions library enabled.

---------------------------------------

## setfont
*setfont <font>;

This command sets the current RO client interface font to one of the
fonts stored in data\*.eot by using an ID of the font. When the ID
of the currently used font is used, default interface font is used
again.

	0 - Default
	1 - RixLoveangel
	2 - RixSquirrel
	3 - NHCgogo
	4 - RixDiary
	5 - RixMiniHeart
	6 - RixFreshman
	7 - RixKid
	8 - RixMagic
	9 - RixJJangu

---------------------------------------

## showdigit
*showdigit <value>{,<type>};

Displays given numeric 'value' in large digital clock font on top of
the screen. The optional parameter 'type' specifies visual aspects
of the "clock" and can be one of the following values:

	0 - Displays the value for 5 seconds (default).
	1 - Incremental counter (1 tick/second).
	2 - Decremental counter (1 tick/second). Does not stop at zero,
		but overflows.
	3 - Decremental counter (2 ticks/second). Two digits only, stops
		at zero.

Except for type 3 the value is interpreted as seconds and formatted
as time in days, hours, minutes and seconds. Note, that the official
script command does not have the optional parameter.

	// displays 23:59:59 for 5 seconds
	showdigit 86399;

	// counter that starts at 60 and runs for 30 seconds
	showdigit 60,3;

---------------------------------------

## setcell
*setcell "<map name>",<x1>,<y1>,<x2>,<y2>,<type>,<flag>;

Each map cell has several 'flags' that specify the properties of that cell.
These include terrain properties (walkability, shootability, presence of water),
skills (basilica, land protector, ...) and other (NPC nearby, no vending, ...).
Each of these can be 'on' or 'off'. Together they define a cell's behavior.

This command lets you alter these flags for all map cells in the specified
(x1,y1)-(x2,y2) rectangle. The 'flag' can be 0 or 1 (0:clear flag, 1:set flag).
The 'type' defines which flag to modify. Possible options see 'src/map/script_constants.hpp'.

Example:

	setcell "arena",0,0,300,300,cell_basilica,1;
	setcell "arena",140,140,160,160,cell_basilica,0;
	setcell "arena",135,135,165,165,cell_walkable,0;
	setcell "arena",140,140,160,160,cell_walkable,1;

This will add a makeshift ring into the center of the map. The ring will be
surrounded by a 5-cell wide 'gap' to prevent interference from outside, and
the rest of the map will be marked as 'basilica', preventing observers from
casting any offensive skills or fighting among themselves. Note that the wall
will not be shown nor known client-side, which may cause movement problems.

Another example:

OnBarricadeDeploy:
	setcell "schg_cas05",114,51,125,51,cell_walkable,0;
	end;
OnBarricadeBreak:
	setcell "schg_cas05",114,51,125,51,cell_walkable,1;
	end;

This could be a part of the WoE:SE script, where attackers are not allowed
to proceed until all barricades are destroyed. This script would place and
remove a nonwalkable row of cells after the barricade mobs.

---------------------------------------

## checkcell
*checkcell ("<map name>",<x>,<y>,<type>);

This command will return 1 or 0, depending on whether the specified cell has
the 'type' flag set or not. There are various types to check, all mimicking
the server's cell_chk enumeration. The types can be found in 'src/map/script_constants.hpp'.

The meaning of the individual types can be confusing, so here's an overview:
  - cell_chkwall/water/cliff
    these check directly for the 'terrain component' of the specified cell
  - cell_chkpass/reach/nopass/noreach
    passable = not wall & not cliff, reachable = passable wrt. no-stacking mod
  - cell_chknpc/basilica/landprotector/novending/nochat
    these check for specific dynamic flags (their name indicates what they do)

Example:
	mes "Pick a destination map.";
	input .@map$;
	mes "Alright, now give me the coordinates.";
	input .@x;
	input .@y;
	if ( !checkcell(.@map$,.@x,.@y,cell_chkpass) ) {
		mes "Can't warp you there, sorry!";
		close;
	} else {
		mes "Ok, get ready...";
		close2;
		warp .@map$, .@x, .@y;
		end;
	}

---------------------------------------

## getfreecell
*getfreecell "<map name>",<rX>,<rY>{,<x>,<y>,<rangeX>,<rangeY>,<flag>};

Finds a free cell on the given map and stores the reference to the found cell
in <rX> and <rY>. Passing <x> and <y> with <rangeX> and <rangeY> allows for
searching within a specified area on the given map. The <flag> is a bitmask
and has the following possible values:
 - 1 = Random cell on the map or from <x>,<y> range. (default)
 - 2 = The target should be able to walk to the target tile.
 - 4 = There shouldn't be any players around the target tile (use the no_spawn_on_player setting).

Examples:
	getfreecell("prontera",.@x,.@y); // Find a random empty cell in Prontera and store it within .@x and .@y
	getfreecell("prontera",.@x,.@y,150,150,5,5); // Find a random empty cell on 150,150 (with a range of 5x5) in Prontera and store it within .@x and .@y

---------------------------------------

## setwall
*setwall "<map name>",<x>,<y>,<size>,<dir>,<shootable>,"<name>";
## delwall
*delwall "<name>";

Creates an invisible wall, an array of "setcell" starting from x,y and doing a
line of the given size in the given direction. The difference with setcell is
this one update client part too to avoid the glitch problem. Directions are the
same as NPC sprite facing directions: 0=north, 1=northwest, 2=west, etc.

---------------------------------------

## checkwall
*checkwall "<name>";

This command will return true if the wall with the given name exists, false otherwise.

---------------------------------------

## readbook
*readbook <book id>,<page>;

This command will open a book item at the specified page.

---------------------------------------

## open_roulette
*open_roulette( {char_id} )

Opens the roulette window for the currently attached character or the character
with the given character id.

---------------------------------------

# 7. Instance commands

## instance_create
*instance_create("<instance name>"{,<instance mode>{,<owner id>}});

Creates an instance for the <owner id> of <mode>. The instance name, along with
all other instance data, is read from 'db/(pre-)re/instance_db.txt'. Upon success,
the command generates a unique instance ID, duplicates all listed maps and NPCs,
sets the alive time, and triggers the "OnInstanceInit" label in all NPCs inside
the instance.

Instance Mode options:
 IM_NONE: Attached to no one.
 IM_CHAR: Attached to a single character.
 IM_PARTY: Attached to a party (default instance mode).
 IM_GUILD: Attached to a guild.
 IM_CLAN: Attached to a clan.

The command returns the instance ID upon success, and these values upon failure:
 -1: Invalid type.
 -2: Character/Party/Guild/Clan not found.
 -3: Instance already exists.
 -4: No free instances (MAX_INSTANCE exceeded).

---------------------------------------

## instance_destroy
*instance_destroy {<instance id>};

Destroys instance with the ID <instance id>. If no ID is specified, the instance
the script is attached to is used. If that fails, the script will come to a halt.
This will also trigger the "OnInstanceDestroy" label in all NPCs inside the instance.

---------------------------------------

## instance_enter
*instance_enter("<instance name>",{<x>,<y>,<char_id>,<instance id>});

Warps the attached player to the specified <instance id>. If no ID is specified,
the IM_PARTY instance the invoking player is attached to is used.

The map and coordinates are located in 'db/(pre-)re/instance_db.txt'.

The command returns IE_OK upon success, and these values upon failure:
 IE_NOMEMBER:	Party/Guild/Clan not found (for party/guild/clan modes).
 IE_NOINSTANCE:	Character/Party/Guild/Clan does not have an instance.
 IE_OTHER:		Other errors (invalid instance name, instance doesn't match with character/party/guild/clan).

Put -1 for x and y if want to warp player with default entrance coordinates.

---------------------------------------

## instance_npcname
*instance_npcname("<npc name>"{,<instance id>})

Returns the unique name of the instanced script. If no ID is specified,
the instance the script is attached to is used. If that fails, the script
will come to a halt.

---------------------------------------

## instance_mapname
*instance_mapname("<map name>"{,<instance id>})

Returns the unique name of the instanced map. If no instance ID is specified,
the instance the script is attached to is used. If that fails, the command
returns an empty string instead.

---------------------------------------

## instance_id
*instance_id({<instance mode>})

Returns the unique instance ID of the given mode. By default it returns the
attached script instance. If <instance mode> is provided then the instance
of the currently attached player is used. If that fails, the function will return 0.

Instance Mode options:
 IM_CHAR:	Attached to character.
 IM_PARTY:	Attached to character's party.
 IM_GUILD:	Attached to character's guild.
 IM_CLAN:	Attached to character's clan.

---------------------------------------

## instance_warpall
*instance_warpall "<map name>",<x>,<y>{,<instance id>};

Warps all players in the <instance id> to <map name> to the given coordinates.
If no ID is specified, the IM_PARTY instance the invoking player is attached
to is used. If that fails, the script will come to a halt.

---------------------------------------

## instance_announce
*instance_announce <instance id>,"<text>",<flag>{,<fontColor>{,<fontType>{,<fontSize>{,<fontAlign>{,<fontY>}}}}};

Broadcasts a message to all players in the <instance id> currently residing on
an instance map. If 0 is specified for <instance id>, the instance the script
is attached to is used.

For details on the other parameters, see 'announce'.

---------------------------------------

## instance_check_party
*instance_check_party(<party id>{,<amount>{,<min>{,<max>}}})

This function checks if a party meets certain requirements, returning 1 if all
conditions are met and 0 otherwise. It will only check online characters.

amount - number of online party members (default is 1).
min    - minimum level of all characters in the party (default is 1).
max    - maximum level of all characters in the party (default is max level in conf).

Example:

if (instance_check_party(getcharid(1),2,2,149)) {
	mes "Your party meets the Memorial Dungeon requirements.",
	mes "All online members are between levels 1-150 and at least two are online.";
	close;
} else {
	mes "Sorry, your party does not meet requirements.";
	close;
}

---------------------------------------

## instance_check_guild
*instance_check_guild(<guild id>{,<amount>{,<min>{,<max>}}})

This function checks if a guild meets certain requirements, returning 1 if all
conditions are met and 0 otherwise. It will only check online characters.

amount - number of online guild members (default is 1).
min    - minimum level of all characters in the guild (default is 1).
max    - maximum level of all characters in the guild (default is max level in conf).

Example:

if (instance_check_guild(getcharid(2),2,2,149)) {
	mes "Your guild meets the Memorial Dungeon requirements.",
	mes "All online members are between levels 1-150 and at least two are online.";
	close;
} else {
	mes "Sorry, your guild does not meet requirements.";
	close;
}

---------------------------------------

## instance_check_clan
*instance_check_clan(<clan id>{,<amount>{,<min>{,<max>}}})

This function checks if a clan meets certain requirements, returning 1 if all
conditions are met and 0 otherwise. It will only check online characters.

amount - number of online clan members (default is 1).
min    - minimum level of all characters in the clan (default is 1).
max    - maximum level of all characters in the clan (default is max level in conf).

Example:

if (instance_check_clan(getcharid(5),2,2,149)) {
	mes "Your clan meets the Memorial Dungeon requirements.",
	mes "All online members are between levels 1-150 and at least two are online.";
	close;
} else {
	mes "Sorry, your clan does not meet requirements.";
	close;
}

---------------------------------------

## instance_info
*instance_info("<instance name>",<info type>{,<instance_db map index>});

Returns the specified <info type> of the given <instance name> from the instance database.
If the <instance name> is unknown or an invalid <info type> is supplied -1 will be returned.

Valid info types:
 IIT_ID: Instance database ID as integer.
 IIT_TIME_LIMIT: Instance database total life time as integer.
 IIT_IDLE_TIMEOUT: Instance database timeout time as integer.
 IIT_ENTER_MAP: Instance database enter map as string.
 IIT_ENTER_X: Instance database enter X location as integer.
 IIT_ENTER_Y: Instance database enter Y location as integer.
 IIT_MAPCOUNT: Instance database total maps as integer.
 IIT_MAP: Instance database map name from the given <instance_db map index> as string.
          If the index is invalid an empty string will be returned.

Example:

.@name$ = "Endless Tower";
mes .@name$ + " will be destroyed if no one is in the instance for " + instance_info(.@name$,IIT_IDLETIMEOUT) + " seconds.";
// Endless Tower will be destroyed if no one is in the instance for 300 seconds.

---------------------------------------

## instance_live_info
*instance_live_info(<info type>{,<instance id>});

Returns the specified <info type> of instance attached to the npc or, if
an instance ID is specified, of that instance.

Valid <info type>:
ILI_NAME	- Instance Name
			  Return the name of the instance or "" if that fails.
ILI_MODE	- Instance Mode
			  Return IM_NONE, IM_CHAR, IM_PARTY, IM_GUILD, IM_CLAN or -1 if that fails.
ILI_OWNER	- Owner ID
			  Return an ID according to the instance mode of the instance attached/specified or -1 if that fails.
			  When the instance mode is IM_NONE, ILI_OWNER will return the npc ID that created the instance,
			  IM_CHAR	- the owner char ID
			  IM_PARTY	- the party ID
			  IM_GUILD	- the guild ID
			  IM_CLAN	- the clan ID

Examples:
	// Return the instance name of the instance attached to the npc.
	.@instance_name$ = instance_live_info(ILI_NAME);

	// Return the guild owner ID of the given instance ID.
	.@owner = instance_live_info(ILI_OWNER, instance_id(IM_GUILD));

---------------------------------------

## getvariableofinstance
*getvariableofinstance(<variable>,<instance id>);

Returns a reference to an instance variable (' prefix) of the specific instance ID.
This can only be used to get ' variables.

Examples:
	// This will set the .@s variable to the value of 'var variable of the specific instance ID.
	set .@s, getvariableofinstance('var, instance_id(IM_PARTY));

	// This will set the 'var variable of the specific instance ID to 1.
	set getvariableofinstance('var, instance_id(IM_GUILD)), 1;

---------------------------------------

# 8. Quest Log commands

## questinfo
*questinfo <Quest ID>,<Icon>{,<Map Mark Color>{,<Job Class>}};

This is esentially a combination of checkquest and showevent. Use this only
in an OnInit label. For the Quest ID, specify the quest ID that you want
checked if it has been started yet.

For Icon, use one of the following:

No Icon			: QTYPE_NONE
! Quest Icon	: QTYPE_QUEST
? Quest Icon	: QTYPE_QUEST2
! Job Icon		: QTYPE_JOB
? Job Icon		: QTYPE_JOB2
! Event Icon	: QTYPE_EVENT
? Event Icon	: QTYPE_EVENT2
Warg			: QTYPE_WARG (Only for packetver < 20170315)
Warg Face		: QTYPE_WARG2 (Only for packetver >= 20120410 and < 20170315)
Click Me		: QTYPE_CLICKME (Only for packetver >= 20170315)
Daily Quest		: QTYPE_DAILYQUEST (Only for packetver >= 20170315)
! Event Icon	: QTYPE_EVENT3 (Only for packetver >= 20170315)
Job Quest		: QTYPE_JOBQUEST (Only for packetver >= 20170315)
Jumping Poring	: QTYPE_JUMPING_PORING (Only for packetver >= 20170315)

Map Mark Color, when used, creates a mark in the user's mini map on the position of the NPC,
the available color values are:

0 - No Marker (default)
1 - Yellow Marker
2 - Green Marker
3 - Purple Marker

When a user shows up on a map, each NPC is checked for questinfo that has been set.
If questinfo is present, it will check if the quest has been started, if it has not, the bubble will appear.

Optionally, you can also specify a Job Class if the quest bubble should only appear for a certain class.

Example
izlude,100,100,4	script	Test	844,{
	mes "[Test]";
	mes "Hello World.";
	close;

	OnInit:
		questinfo 1001, QTYPE_QUEST, 0, Job_Novice;
		end;
}

---------------------------------------

## setquestinfo_level
*setquestinfo_level <quest_id>,<min_level>,<max_level>;

Add level range criteria for quest info with quest id 'quest_id'.

This command must be used after 'questinfo'.

---------------------------------------

## setquestinfo_req
*setquestinfo_req <quest_id>,<req_quest_id>,<state>{,<req_quest_id>,<state>,...};

Add 'req_quest_id' as requirement for quest info with quest id 'quest_id'.

Value os 'state' are:
    0: Player doesn't started 'req_quest_id'.
    1: Player has 'req_quest_id' (state is either "inactive" or "active").
    2: Player has 'req_quest_id' completed

This command must be used after 'questinfo'.

---------------------------------------

## setquestinfo_job
*setquestinfo_job <quest_id>,<job_id>{,<job_id>...};

Add job criteria for quest info with quest id 'quest_id'.
The job criteria may more than one job_id.

This command must be used after 'questinfo'.

---------------------------------------

## setquest
*setquest <ID>{,<char_id>};

Place quest of <ID> in the users quest log, the state of which is "active".

If *questinfo is set, and the same ID is specified here, the icon will be cleared when the quest is set.

---------------------------------------

## completequest
*completequest <ID>{,<char_id>};

Change the state for the given quest <ID> to "complete" and remove from the users quest log.

---------------------------------------

## erasequest
*erasequest <ID>{,<char_id>};

Remove the quest of the given <ID> from the user's quest log.

---------------------------------------

## changequest
*changequest <ID>,<ID2>{,<char_id>};

Remove quest of the given <ID> from the user's quest log.
Add quest of the <ID2> to the the quest log, and the state is "active".

---------------------------------------

## checkquest
*checkquest(<ID>{,PLAYTIME|HUNTING{,<char_id>}})

If no additional argument supplied, return the state of the quest:
	-1 = Quest not started (not in quest log)
	0  = Quest has been given, but the state is "inactive"
	1  = Quest has been given, and the state is "active"
	2  = Quest completed

If parameter "PLAYTIME" is supplied:
	-1 = Quest not started (not in quest log)
	0  = the time limit has not yet been reached
	1  = the time limit has not been reached but the quest is marked as complete
	2  = the time limit has been reached

If parameter "HUNTING" is supplied:
	-1 = Quest not started (not in quest log)
	0  = you haven't killed all of the target monsters and the time limit has not been reached.
	1  = you haven't killed all of the target monsters but the time limit has been reached.
	2  = you've killed all of the target monsters

---------------------------------------

## isbegin_quest
*isbegin_quest(<ID>{,<char_id>})

Return the state of the quest:
	0  = Quest not started (not in quest log)
	1  = Quest has been given (state is either "inactive" or "active")
	2  = Quest completed

---------------------------------------

## showevent
*showevent <icon>{,<mark color>{,<char_id>}}

Show an emotion on top of a NPC, and optionally,
a colored mark in the mini-map like "viewpoint".
This is used to indicate that a NPC has a quest or an event to
a certain player.

Available Icons:

Remove Icon		: QTYPE_NONE
! Quest Icon	: QTYPE_QUEST
? Quest Icon	: QTYPE_QUEST2
! Job Icon		: QTYPE_JOB
? Job Icon		: QTYPE_JOB2
! Event Icon	: QTYPE_EVENT
? Event Icon	: QTYPE_EVENT2
Warg			: QTYPE_WARG
Warg Face		: QTYPE_WARG2 (Only for packetver >= 20120410)

Mark Color:
0 - No Mark
1 - Yellow Mark
2 - Green Mark
3 - Purple Mark

---------------------------------------

# 9. Battleground commands

## waitingroom2bg_single
*waitingroom2bg_single(<battle group>,{"<map name>",<x>,<y>{,"<npc name>"}});

Adds the first waiting player from the chat room of the given NPC to an existing battleground group.
The player will also be warped to the default spawn point of the battle group or to the specified coordinates <x> and <y> on the given <map>.

---------------------------------------

## waitingroom2bg
*waitingroom2bg("<map name>",<x>,<y>,{"<On Quit Event>","<On Death Event>"{,"<NPC Name>"}});

<map name>,<x>,<y> refer to where the "respawn" base is, where the player group will respawn when they die.
<On Quit Event> refers to an NPC label that attaches to the character and is run when they relog. (Optional)
<On Death Event> refers to an NPC label that attaches to the character and is run when they die. (Optional)

If "-" is supplied for <map name> then the player will not automatically respawn after the 1 second delay.
This allows for better manipulation of <On Death Event>. The player will have to be warped to desired location
at the end of <On Death Event>.

Unlike the prior command, the latter will attach a GROUP in a waiting room to the battleground, and
sets the array $@arenamembers[0] where 0 holds the IDs of the first group, and 1 holds the IDs of the second.

If the optional NPC Name parameter is left out, the waiting room of the current NPC is used.

Example:
	// Battle Group will be referred to as $@KvM01BG_id1, and when they die, respawn at bat_c01,52,129.
	set $@KvM01BG_id1, waitingroom2bg("bat_c01",52,129,"KvM01_BG::OnGuillaumeQuit","KvM01_BG::OnGuillaumeDie");
	end;

---------------------------------------

## bg_create
*bg_create("<map name>",<x>,<y>{,"<On Quit Event>","<On Death Event>"});

Creates an instance of battleground battle group that can be used with other battleground commands.

<map name>,<x>,<y> refer to where the "respawn" base is, where the player group will respawn when they die.
<On Quit Event> refers to an NPC label that attaches to the character and is run when they relog. (Optional)
<On Death Event> refers to an NPC label that attaches to the character and is run when they die. (Optional)

If "-" is supplied for <map name> then the player will not automatically respawn after the 1 second delay.
This allows for better manipulation of <On Death Event>. The player will have to be warped to desired location
at the end of <On Death Event>.

Returns battle group ID on success. Returns 0 on failure.

---------------------------------------

## bg_join
*bg_join(<battle group>,{"<map name>",{<x>,<y>{,<char id>}});

Adds an attached player or <char id> if specified to an existing battleground group. The player will also be warped to the default spawn point of the battle group or to the specified coordinates <x> and <y> on the given <map>.

Returns true on success. Returns false on failure.

---------------------------------------

## bg_team_setxy
*bg_team_setxy <Battle Group ID>,<x>,<y>;

Updates the respawn point of the given Battle Group to x,y on the same map. <Battle Group ID> can be retrieved using getcharid(4).

Example:
	bg_team_setxy getcharid(4),56,212;
	mapannounce "bat_a01", "Group [1] has taken the work shop, and will now respawn there.",bc_map,"0xFFCE00";
	end;

---------------------------------------

## bg_warp
*bg_warp <Battle Group>,"<map name>",<x>,<y>;

Similar to the 'warp' command.
Places all members of <Battle Group> at the specified map and coordinates.

Example:
	//place the battle group one for Tierra Gorge at starting position.
	bg_warp $@TierraBG1_id1,"bat_a01",352,342;
	end;

---------------------------------------

## bg_monster
*bg_monster <Battle Group>,"<map name>",<x>,<y>,"<name to show>",<mob id>,"<event label>";
## bg_monster
*bg_monster(<Battle Group>,"<map name>",<x>,<y>,"<name to show>",<mob id>,"<event label>");

Similar to the 'monster' command.
Spawns a monster with allegiance to the given Battle Group.
Does not allow for the summoning of multiple monsters.
Monsters are similar to those in War of Emperium, in that the specified Battle Group is considered friendly.

Example:
	// It can be used in two different ways.
	bg_monster $@TierraBG1_id2,"bat_a01",167,50,"Food Depot",1910,"Feed Depot#1::OnMyMobDead";
	end;

	// Alternatively, you can set an ID for the monster using "set".
	// This becomes useful when used with the command below.
	set $@Guardian_3, bg_monster($@TierraBG1_id2,"bat_a01",268,204,"Guardian",1949,"NPCNAME::OnMyMobDead");
	end;

---------------------------------------

## bg_monster_set_team
*bg_monster_set_team <GID>,<Battle Group>;

This command will change the allegiance if a monster in a battle ground.
GID can be set when spawning the monster via the 'bg_monster' command.

Example:

	end;

OnEnable:
	mapannounce "A guardian has been summoned for Battle Group 2!",bc_map,"0xFFCE00";
	set $@Guardian, bg_monster($@BG_2,"bat_a01",268,204,"Guardian",1949,"NPCNAME::OnMyMobDead");
	initnpctimer;
	end;

OnTimer1000:
	stopnpctimer;
	mapannounce "Erm, sorry about that! This monster was meant for Battle Group 1.",bc_map,"0xFFCE00";
	bg_monster_set_team $@Guardian, $@BG_1;
	end;

---------------------------------------

## bg_leave
*bg_leave {<char_id>};

Removes attached player from their Battle Group.

---------------------------------------

## bg_destroy
*bg_destroy <Batte Group>;

Destroys the Battle Group created for that battle ground.

---------------------------------------

## areapercentheal
*areapercentheal "<map name>",<x1>,<y1>,<x2>,<y2>,<hp>,<sp>;

Restores a percentage of the maximum HP/SP of players within a defined area.
This is primarily used in battleground scripts, but is not limited to them.

Example:
	areapercentheal "bat_a01",52,208,61,217,100,100;
	end;

---------------------------------------

## bg_get_data
*bg_get_data(<Battle Group>,<type>);

Retrieves data related to given Battle Group. Type can be one of the following:

	0 - Amount of players currently belonging to the group.
	1 - Store GID of players in <Battle Group> in a temporary global array $@arenamembers
		and returns amount of players currently belonging to the group.

---------------------------------------

## bg_getareausers
*bg_getareausers(<Battle Group>,"<map name>",<x0>,<y0>,<x1>,<y1>);

Retrieves the amount of players belonging to the given Battle Group on the given
map within the specified rectangular area.

---------------------------------------

## bg_updatescore
*bg_updatescore "<map name>",<Guillaume Score>,<Croix Score>;

This command will force the update of the displayed scoreboard.
It is only usable when the map is defined as a Type 2 Battleground:
mapflag	<map name>	battleground	2

---------------------------------------

# 10. Pet commands

## bpet
*bpet;
## birthpet
*birthpet;

This command opens up a pet hatching window on the client connected to the
invoking character. It is used in item script for the pet incubators and will
let the player hatch an owned egg. If the character has no eggs, it will just
open up an empty incubator window.
This is still usable outside item scripts.

---------------------------------------

## pet
*pet <pet id>;
## catchpet
*catchpet <pet id>;

This command is used in all the item scripts for taming items. Running this
command will make the pet catching cursor appear on the client connected to the
invoking character, usable on the monsters with the specified pet ID number. It
will still work outside an item script.
If the <pet id> is PET_CATCH_UNIVERSAL the item will attempt to catch the targeted
monster as long as it is in the pet database and does not have the MD_STATUS_IMMUNE
monster mode.
If the <pet id> is PET_CATCH_UNIVERSAL_ITEM the item will attempt to catch the targeted
monster as long as it is in the pet database and the targeted monster requires the lure
item used.
See 'doc/mob_db_mode_list.txt' for more information about monster modes.

A full list of pet IDs can be found inside 'db/(pre-)re/pet_db.yml'.

---------------------------------------

## makepet
*makepet <pet id>;

This command will create a pet egg and put it in the invoking character's
inventory. The kind of pet is specified by pet ID numbers listed in
'db/(pre-)re/pet_db.yml'. The egg is created exactly as if the character just successfully
caught a pet in the normal way.

	// This will make you a poring:
	makepet 1002;

Notice that you absolutely have to create pet eggs with this command. If you try
to give a pet egg with 'getitem', pet data will not be created by the char
server and the egg will disappear when anyone tries to hatch it.

---------------------------------------

## getpetinfo
*getpetinfo(<type>{,<char_id>})

This function will return pet information for the pet the invoking character
currently has active. Valid types are:

 PETINFO_ID - Pet ID
 PETINFO_CLASS - Pet class number as per 'db/(pre-)re/pet_db.yml' - will tell you what kind of a pet it is.
 PETINFO_NAME - Pet name. Will return "null" if there's no pet.
 PETINFO_INTIMATE - Pet friendly level (intimacy score). 1000 is full loyalty.
 PETINFO_HUNGRY - Pet hungry level. 100 is full hunger.
 PETINFO_RENAMED - Pet rename flag. 0 means this pet has not been named yet.
 PETINFO_LEVEL - Pet level
 PETINFO_BLOCKID - Pet Game ID
 PETINFO_EGGID - Pet egg item ID
 PETINFO_FOODID - Pet food item ID

PETINFO_INTIMATE can be used with the following constants for checking values:
 PET_INTIMATE_NONE = 0
 PET_INTIMATE_AWKWARD = 1 ~ 99
 PET_INTIMATE_SHY = 100 ~ 249
 PET_INTIMATE_NEUTRAL = 250 ~ 749
 PET_INTIMATE_CORDIAL = 750 ~ 909
 PET_INTIMATE_LOYAL = 910 ~ 1000

PETINFO_HUNGRY can be used with the following constants for checking values:
 PET_HUNGRY_NONE = 0
 PET_HUNGRY_VERY_HUNGRY = 1 ~ 10
 PET_HUNGRY_HUNGRY = 11 ~ 25
 PET_HUNGRY_NEUTRAL = 26 ~ 75
 PET_HUNGRY_SATISFIED = 76 ~ 90
 PET_HUNGRY_STUFFED = 91 ~ 100

Example:
	mes "[Vet]";
	mes "Your pet + " getpetinfo(PETINFO_NAME);
	if (getpetinfo(PETINFO_INTIMATE) < PET_INTIMATE_LOYAL)
		mes "has some growing to do on you!";
	else
		mes "seems to love you very much!";
	close;

---------------------------------------

=============================
|10.1.- The Pet AI commands.|
=============================
---------------------------------------

These commands will only work if the invoking character has a pet, and are meant
to be executed from pet scripts. They will modify the pet AI decision-making for
the current pet of the invoking character, and will NOT have any independent
effect by themselves, which is why only one of them each may be in effect at any
time for a specific pet. A pet may have 'petloot', 'petskillbonus',
'petskillattack' OR 'petpetskillattack2' and 'petskillsupport'.

All commands with delays and durations will only make the behavior active for
the specified duration of seconds, with a delay of the specified number of
seconds between activations. Rates are a chance of the effect occurring and are
given in percent. 'bonusrate' is added to the normal rate if the pet intimacy is
at the maximum possible.

The behavior modified with the below mentioned commands will only be exhibited if
the pet is loyal and appropriate configuration options are set in
'battle_athena.conf'.

Pet scripts in the database normally run whenever a pet of that type hatches
from the egg. Other commands usable in item scripts (see 'bonus') will also
happily run from pet scripts. Apparently, the pet-specific commands will also
work in NPC scripts and modify the behavior of the current pet up until the pet
is hatched again. (Which will also occur when the character is logged in again
with the pet still out of the egg.) It is not certain for how long the effect of
such command running from an NPC script will eventually persist, but apparently,
it is possible to usefully employ them in usable item scripts to create pet
buffing items.

Nobody tried this before, so you're essentially on your own here.

---------------------------------------

## petskillbonus
*petskillbonus <bonus type>,<value>,<duration>,<delay>;

This command will make the pet give a bonus to the owner's stat in certain
duration in seconds and will be repeated for certain delay in seconds.

For a full bonus list, see 'doc/item_bonus.txt'
NOTE: Currently ONLY supported for bonuses that used by 'bonus' script.

---------------------------------------

## petrecovery
*petrecovery <status type>,<delay>;

This command will make the pet cure a specified status condition. The curing
actions will occur once every Delay seconds. For a full list of status
conditions that can be cured, see the list of 'SC_' status condition constants
in 'src/map/script_constants.hpp'.

---------------------------------------

## petloot
*petloot <max items>;

This command will turn on pet looting, with a maximum number of items to loot
specified. Pet will store items and return them when the maximum is reached or
when pet performance is activated.

---------------------------------------

## petskillsupport
*petskillsupport <skill id>,<skill level>,<delay>,<percent hp>,<percent sp>;
## petskillsupport
*petskillsupport "<skill name>",<skill level>,<delay>,<percent hp>,<percent sp>;

This will make the pet use a specified support skill on the owner whenever the
HP and SP are below the given percent values, with a specified delay time
between activations. The skill numbers are as per 'db/(pre-)re/skill_db.txt'.

It's not quite certain who's stats will be used for the skills cast, the
character's or the pets. Probably, Skotlex can answer that question.

---------------------------------------

## petskillattack
*petskillattack <skill id>,<skill level>,<rate>,<bonusrate>;
## petskillattack
*petskillattack "<skill name>",<skill level>,<rate>,<bonusrate>;
## petskillattack2
*petskillattack2 <skill id>,<damage>,<number of attacks>,<rate>,<bonusrate>;
## petskillattack2
*petskillattack2 "<skill name>",<damage>,<number of attacks>,<rate>,<bonusrate>;

These two commands will make the pet cast an attack skill on the enemy the pet's
owner is currently fighting. Skill IDs and levels are as per 'petskillsupport'.
'petskillattack2' will make the pet cast the skill with a fixed amount of damage
inflicted and the specified number of attacks.

Value of 'rate' is between 1 and 100. 100 = 100%

---------------------------------------

# 11. Homunculus commands

## homevolution
*homevolution;

This command will try to evolve the current player's homunculus.
If it doesn't work, the /swt emotion is shown.

To evolve a homunculus, the invoking player must have a homunculus,
the homunculus must not be the last evolution and
the homunculus must have above 91000 intimacy with its owner.

---------------------------------------

## morphembryo
*morphembryo;

This command will try to put the invoking player's Homunculus in an
uncallable state, required for mutation into a Homunculus S. The player
will also receive a Strange Embryo (ID 6415) in their inventory if
successful, which is deleted upon mutation.

The command will fail if the invoking player does not have an evolved
Homunculus at level 99 or above. The /swt emotion is shown upon failure.

Returns 1 upon success and 0 for all failures.

---------------------------------------

## hommutate
*hommutate {<ID>};

This command will try to mutate the invoking player's Homunculus into
a Homunculus S. The Strange Embryo (ID 6415) is deleted upon success.

The command will fail if the invoking player does not have an evolved
Homunculus at level 99 or above, if it is not in the embryo state
(from the 'morphembryo' command), or if the invoking player does not
possess a Strange Embryo. The /swt emotion is shown upon failure.

If the optional parameter <ID> is set, the invoking player's Homunculus
will change into the specified Homunculus ID. Otherwise, a random Homunculus S
will be chosen. See 'db/homunculus_db.txt' for a full list of IDs.

Returns 1 upon success and 0 for all failures.

---------------------------------------

## checkhomcall
*checkhomcall()

This function checks if the attached player's Homunculus is active,
and will return the following values:
 -1: The player has no Homunculus.
  0: The player's Homunculus is active.
  1: The player's Homunculus is vaporized.
  2: The player's Homunculus is in morph state.

---------------------------------------

## gethominfo
*gethominfo(<type>{,<char_id>})

This function will return Homunculus information for the Homunculus of the
invoking character, regardless of its vaporize state. It returns zero or
"null" if the player does not own a Homunculus.

Valid types are:
 0 - Homunculus ID
 1 - Homunculus Class
 2 - Homunculus Name
 3 - Homunculus friendly level (intimacy score). 100000 is full loyalty.
 4 - Homunculus hungry level. 100 is completely full.
 5 - Homunculus rename flag. 0 means this homunculus has not been named yet.
 6 - Homunculus level
 7 - Homunculus Game ID

---------------------------------------

## homshuffle
*homshuffle;

This will recalculate the homunculus stats according to its level, of the
current invoking character.

---------------------------------------

# 12. Mercenary commands

## mercenary_create
*mercenary_create <class>,<contract time>;

This command summons a mercenary for a given time (in milliseconds). For a
list of all available classes, see 'db/mercenary_db.txt'.

This command is typically used in item scripts of mercenary scrolls.

---------------------------------------

## mercenary_heal
*mercenary_heal <hp>,<sp>;

This command works like 'heal', but affects the mercenary of the
currently attached character.

---------------------------------------

## mercenary_sc_start
*mercenary_sc_start <type>,<tick>,<val1>;

This command works like 'sc_start', but affects the mercenary of the
currently attached character.

---------------------------------------

## mercenary_get_calls
*mercenary_get_calls(<guild>);
## mercenary_set_calls
*mercenary_set_calls <guild>,<value>;

Sets or gets the mercenary calls value for given guild for currently
attached character. Guild can be one or the following constants:

	ARCH_MERC_GUILD
	SPEAR_MERC_GUILD
	SWORD_MERC_GUILD

---------------------------------------

## mercenary_get_faith
*mercenary_get_faith(<guild>);
## mercenary_set_faith
*mercenary_set_faith <guild>,<value>;

Sets or gets the mercenary faith value for given guild for currently
attached character. Guild can be one or the following constants:

	ARCH_MERC_GUILD
	SPEAR_MERC_GUILD
	SWORD_MERC_GUILD

---------------------------------------

## getmercinfo
*getmercinfo(<type>{,<char id>});

Retrieves information about mercenary of the currently attached
character. If char id is given, the information of that character is
retrieved instead. Type specifies what information to retrieve and
can be one of the following:

	0 - Mercenary ID
	1 - Mercenary Class
	2 - Mercenary Name
	3 - Mercenary faith value for this mercenary's guild, if any
	4 - Mercenary calls value for this mercenary's guild, if any
	5 - Mercenary kill count
	6 - Mercenary remaining life time in msec
	7 - Mercenary level
	8 - Mercenary Game ID

If the character does not have a mercenary, the command returns ""
for name and 0 for all other types.

---------------------------------------

# 13. Party commands

## getpartyname
*getpartyname(<party id>)

This function will return the name of a party that has the specified ID number.
If there is no such party ID, "null" will be returned.

Lets say the ID of a party was saved as a global variable:

	// This would return the name of the party from the ID stored in a variable
	mes "You're in the '" + getpartyname($@var) + "' party, I know!";

---------------------------------------

## getpartymember
*getpartymember <party id>{,<type>{,<array_variable>}};

This command will find all members of a specified party and returns their names
(or character id or account id depending on the value of "type") into an array
of temporary global variables. There's actually quite a few commands like this
which will fill a special variable with data upon execution and not do anything
else.

Upon executing this,

$@partymembername$[] is a global temporary string array which contains all the
                     names of these party members
                     (only set when type is 0 or not specified)

$@partymembercid[]   is a global temporary number array which contains the
                     character id of these party members.
                     (only set when type is 1)

$@partymemberaid[]   is a global temporary number array which contains the
                     account id of these party members.
                     (only set when type is 2)

$@partymembercount   is the number of party members that were found.

The party members will (apparently) be found regardless of whether they are
online or offline. Note that the names come in no particular order.

Be sure to use $@partymembercount to go through this array, and not
'getarraysize', because it is not cleared between runs of 'getpartymember'. If
someone with 7 party members invokes this script, the array would have 7
elements. But if another person calls up the NPC, and he has a party of 5, the
server will not clear the array for you, overwriting the values instead. So in
addition to returning the 5 member names, the 6th and 7th elements from the last
call remain, and you will get 5+2 members, of which the last 2 don't belong to
the new guy's party. $@partymembercount will always contain the correct number,
(5) unlike 'getarraysize()' which will return 7 in this case.

If 'array_variable' is set, the result will be stored to that variable instead
using global variable.

Example 1: list party member names

	// get the party member names
	getpartymember getcharid(1),0;

	// It's a good idea to copy the global temporary $@partymember*****
	// variables to your own scope variables because if you have pauses in this
	// script (sleep, sleep2, next, close2, input, menu, select, or prompt),
	// another player could click this NPC, trigger 'getpartymember', and
	// overwrite the $@partymember***** variables.
	.@count = $@partymembercount;
	copyarray .@name$[0], $@partymembername$[0], $@partymembercount;

	// list the party member names
	for (.@i = 0; .@i < .@count; .@i++)
		mes (.@i +1) + ". ^0000FF" + .@name$[.@i] + "^000000";
	close;


Example 2: check party count (with a 'next' pause), before warping to event

	.register_num = 5; // How many party members are required?

	// get the charID and accountID of character's party members
	getpartymember getcharid(1), 1;
	getpartymember getcharid(1), 2;

	if ( $@partymembercount != .register_num ) {
		mes "Please form a party of " + .register_num + " to continue";
		close;
	}

	// loop through both and use 'isloggedin' to count online party members
	for ( .@i = 0; .@i < $@partymembercount; .@i++ )
		if ( isloggedin( $@partymemberaid[.@i], $@partymembercid[.@i] ) )
			.@count_online++;

	// We search accountID & charID because a single party can have multiple
	// characters from the same account. Without searching through the charID,
	// if a player has 2 characters from the same account inside the party but
	// only 1 char online, it would count their online char twice.

	if ( .@count_online != .register_num ) {
		mes "All your party members must be online to continue";
		close;
	}

	// copy the array to prevent players cheating the system
	copyarray .@partymembercid, $@partymembercid, .register_num;

	mes "Are you ready ?";
	next; // careful here
	select("Yes");

	// When a script hits a next, menu, sleep or input that pauses the script,
	// players can invite or /leave and make changes in their party. To prevent
	// this, we call getpartymember again and compare with the original values.

	getpartymember getcharid(1), 1;
	if ( $@partymembercount != .register_num ) {
		mes "You've made changes to your party !";
		close;
	}
	for ( .@i = 0; .@i < $@partymembercount; .@i++ ) {
		if ( .@partymembercid[.@i] != $@partymembercid[.@i] ) {
			mes "You've made changes to your party !";
			close;
		}
	}

	// Finally, it's safe to start the event!
	warpparty "event_map", 0,0, getcharid(1);

---------------------------------------

## getpartyleader
*getpartyleader(<party id>{,<type>})

This function returns some information about the given party-id's leader.
When type is omitted, the default information retrieved is the leader's name.
Possible types are:

	1: Leader account id
	2: Leader character id
	3: Leader's class
	4: Leader's current map name
	5: Leader's current level as stored on the party structure (may not be
	   current level if leader leveled up recently).

If retrieval fails (leader not found or party does not exist), this function
returns "null" instead of the character name, and -1 for the other types.

---------------------------------------

## is_party_leader
*is_party_leader({<party ID>})

This command will return true if the player attached to the script is the leader
of his/her party, or, if a party ID is specified, of that party.

---------------------------------------

## party_create
*party_create("<party name>"{,<character id>{,<item share>,<item share type>}});

Organizes a party with the attached or specified character as leader. If
successful, the command returns 1 and sets the global temporary variable
"$@party_create_id" to the ID of the party created.

Additionally, item sharing options can be provided:
 - Item Share: 0-Each Take (default), 1-Party Share
 - Item Share Type: 0-Each Take (default), 1-Even Share

These values are returned upon failure:
 0: Unknown error.
-1: Player not found.
-2: Player already has a party.
-3: Party name exists.

---------------------------------------

## party_destroy
*party_destroy(<party id>);

Disbands a party. The command returns 1 upon success and 0 upon failure.

---------------------------------------

## party_addmember
*party_addmember(<party id>,<character id>);

Adds a player to an existing party.

The command returns 1 upon success, and these values upon failure:
 0: Unknown error.
-1: Player not found.
-2: Player already has a party.
-3: Party not found.
-4: Party is full.
-5: Another character from the same account is already in the party.

---------------------------------------

## party_delmember
*party_delmember({<character id>,<party id>});

Removes a player from his/her party. If no player is specified, the command
will run for the invoking player. If that player is the only party member
remaining, the party will be disbanded.

The command returns 1 upon success, and these values upon failure:
 0: Unknown error.
-1: Player not found.
-2: Party not found.
-3: Player is not in the party.

---------------------------------------

## party_changeleader
*party_changeleader(<party id>,<character id>);

Transfers leadership of a party to the specified character.

The command returns 1 upon success, and these values upon failure:
 0: Unknown error.
-1: Party not found.
-2: Player not found.
-3: Player is not in the party.
-4: Player is already party leader.

---------------------------------------

## party_changeoption
*party_changeoption(<party id>,<option>,<flag>);

Changes a party option.

Valid options are:
 0 - Exp Share (flags: 0-Each Take, 1-Even Share)
 1 - Item Share (flags: 0-Each Take, 1-Party Share)
 2 - Item Share Type (flags: 0-Each Take, 1-Even Share)

The command returns 1 upon success, and these values upon failure:
 0: Invalid option.
-1: Party not found.

---------------------------------------

## opendressroom
*opendressroom(<flag>{,<char_id>});

This will open the Dress Room window on the client connected to the invoking character.

	mes "Close this window to open the Dress Room window.";
	close2;
	opendressroom(1);
	end;

Valid flag are:
 1 - Open the Dress Room window

---------------------------------------

## navigateto
*navigateto("<map>"{,<x>,<y>,<flag>,<hide_window>,<monster_id>,<char_id>});

Generates a navigation for attached or specified character. Requires client
2011-10-10aRagEXE or newer.

The flag specifies how the client will calculate the specific route.

Valid flags are:
 NAV_NONE - No services
 NAV_AIRSHIP_ONLY - Airship only
 NAV_SCROLL_ONLY - Scroll only
 NAV_AIRSHIP_AND_SCROLL - Airship and Scroll
 NAV_KAFRA_ONLY - Kafra only
 NAV_KAFRA_AND_AIRSHIP - Kafra and Airship
 NAV_KAFRA_AND_SCROLL - Kafra and Scroll
 NAV_ALL - All services

When flag is not specified, the default value is NAV_KAFRA_AND_AIRSHIP.

The hide_window specifies whether to display (0) or hide (1) the navigation window.
By default the window is hidden.

You can specify the monster_id in combination with a mapname to make the
navigation system tell you, that you have reached the desired mob.

Note:
The client requires custom monster spawns be in the navigation file
for using the embedded client Navigation feature to work properly. In this
instance sending the player to the map where the monster spawns is a simpler
solution rather than sending the map and the monster_id.

---------------------------------------

## hateffect
*hateffect(<Hat Effect ID>,<State>);

This will set a Hat Effect onto the player. The state field allows you to
enable (true) or disable (false) the effect on the player.
The Hat Effect constants can be found in 'src/map/script_constants.hpp' starting
with HAT_EF_*.

Requires client 2015-05-13aRagEXE or newer.

---------------------------------------

## getrandomoptinfo
*getrandomoptinfo(<type>);

Returns value of an attribute of current random option.

Valid attributes are:
ROA_ID - ID of current option
ROA_VALUE - Value field of current option
ROA_PARAM - Param field of current option

This script command is intended for using in random option scripts.

---------------------------------------

## getequiprandomoption
*getequiprandomoption(<equipment index>,<index>,<type>{,<char id>});

Returns value of an attribute of a random option on an equipped item.

See 'getequipid' for a full list of valid equipment slots.

index parameter can be 0 to MAX_ITEM_RDM_OPT-1 (default 0-4).

For valid attribute types, see `getrandomoptinfo` command reference.

---------------------------------------

## setrandomoption
*setrandomoption(<equipment slot>,<index>,<id>,<value>,<param>{,<char id>});

Sets <index+1>th random option for equipment equipped at <equipment slot>
to <id>, <value> and <param>.

See 'getequipid' for a full list of valid equipment slots.

index parameter can be 0 to MAX_ITEM_RDM_OPT-1 (default 0-4).

ID - ID of random option. See db/const.txt for constants.
Value - Value of random option
Param - Parameter of random option

---------------------------------------

## clan_join
*clan_join(<clan id>{,<char id>});

The attached player joins the clan with the <clan id>. On a successful join,
true is returned, else false if the join failed.
If <char id> is specified, the specified player is used rather than the attached one.

---------------------------------------

## clan_leave
*clan_leave({<char id>});

The attached player will leave their clan. On a successful leave, true is returned,
else false if the leave failed.
If <char id> is specified, the specified player is used rather than the attached one.

---------------------------------------

# 14. Channel commands

## channel_create
*channel_create "<chname>","<alias>"{,"<password>"{<option>{,<delay>{,<color>{,<char_id>}}}}};

Creates a public channel with <chname> as the channel name. To protect the
channel, use <password> or write "null" to create it without a password.
Channel name must start with '#' and cannot be the same as the map or ally
channel names.

<alias> will be used to change the channel name when the channel message
is displayed.

<option> values are:
	CHAN_OPT_BASE		    - Default option including CHAN_OPT_ANNOUNCE_SELF|CHAN_OPT_MSG_DELAY|CHAN_OPT_CAN_CHAT|CHAN_OPT_CAN_LEAVE
	CHAN_OPT_ANNOUNCE_SELF  - Show info for player itself if player has joined/leaves the channel
	CHAN_OPT_ANNOUNCE_JOIN  - Display message when player is joining the channel
	CHAN_OPT_ANNOUNCE_LEAVE - Display message when player is leaving the channel
	CHAN_OPT_MSG_DELAY	    - Enable chat delay for the channel
	CHAN_OPT_COLOR_OVERRIDE - Player's unique font color will override channel's color
	CHAN_OPT_CAN_CHAT	    - Player can chat in the channel
	CHAN_OPT_CAN_LEAVE	    - Player can leave the channel
	CHAN_OPT_AUTOJOIN	    - Players will auto join the channel at login

The <delay> is the minimum chat delay in millisecond for a single player before
the player can chat again in the same channel.

Use <color> hex code to set the color for this channel, if not defined, default
channel color will be used.

If <char_id> is defined, the channel will be a private channel and the player
will be the the channel owner.

Returns 1 on success.

	/**
	 * This example will shows the message on this channel as
	 * [rAthena] Admin : Hello world!
	 * instead of
	 * #rathena Admin : Hello world!
	 **/
	channel_create("#rathena","[rAthena]");
	channel_create("#vip","[VIP]","vipmemberonly");

---------------------------------------

## channel_setopt
*channel_setopt "<chname>",<option>,<value>;

Set option for the channel. Use 1 in <value> to set it, or 0 to unset.
The <option> values are the same as the 'channel_create' options.

For CHAN_OPT_MSG_DELAY, the delay in millisecond must be sent or use 0
to remove the delay at <value>.

Returns 1 on success.

	// Example to set delay
	channel_setopt("#global",CHAN_OPT_MSG_DELAY,5000);

Only for public and private channel.

---------------------------------------

## channel_setcolor
*channel_setcolor "<chname>",<color>;

To change channel color.
<color> uses hex RGB values.

Returns 1 on success.

---------------------------------------

## channel_setpass
*channel_setpass "<chname>","<password>";

To set, unset, or change password of a channel.
Use "null" to remove the password.

Returns 1 on success.
Only for public and private channel.

---------------------------------------

## channel_setgroup
*channel_setgroup "<chname>",<group_id>{,...,<group_id>};
## channel_setgroup2
*channel_setgroup2 "<chname>",<array_of_groups>;

Set group restriction for a channel. Only player with matching <group_id>
are allowed to to join the channel.

By using 0 in the first group channel, the group restriction will be
removed from the channel config.

'channel_setgroup2' receives input for group list as an array.

Returns 0 on failure, and 1 (or n groups count) on success.

	// Example 1: Remove groups
	channel_setgroup("#event",0);

	// Example 2: Multiple values
	channel_setgroup("#vip",2,5);

	// Example 3: Using array
	setarray .@staffs[0],2,3,4,10,99;
	channel_setgroup("#staff",.@staffs);

Only for public and private channel.

---------------------------------------

## channel_chat
*channel_chat "<chname>","<message>"{,<color>};

Sends message to the channel.
Returns 1 on success.

	// Example if channel doesn't have alias
	channel_chat(#rathena,"Hello World!"); // #rathena Hello World!

	// Example if channel has alias
	channel_chat(#rathena,"Hello World!"); // [rAthena] Hello World!

---------------------------------------

## channel_ban
*channel_ban "<chname>",<char_id>;

Ban player from a public or private channel.
Channel's owner or group with PC_PERM_CHANNEL_ADMIN cannot be banned.
Returns 1 on success.

---------------------------------------

## channel_unban
*channel_unban "<chname>",<char_id>;

Unban player from a public or private channel.
Returns 1 on success.

---------------------------------------

## channel_kick
*channel_kick "<chname>",<char_id>;
## channel_kick
*channel_kick "<chname>","<char_name>";

Kick player from a public or private channel.
Channel's owner or group with PC_PERM_CHANNEL_ADMIN cannot be kicked.
Returns 1 on success.

---------------------------------------

## channel_delete
*channel_delete "<chname>";

Delete an existing public or private channel. Cannot delete ally or
local map channel.
Returns 0 on success.

---------------------------------------

# 15. Achievement commands

## achievementadd
*achievementadd(<achievement id>{,<char id>})

This function will add an achievement to the player's log for the attached
player or the supplied <char id>. The objective requirements are not ignored
when using this function.
Returns true on success and false on failure.

---------------------------------------

## achievementremove
*achievementremove(<achievement id>{,<char id>})

This function will remove an achievement from the player's log for the attached
player or the supplied <char id>.
Returns true on success and false on failure.

---------------------------------------

## achievementinfo
*achievementinfo(<achievement id>,<type>{,<char id>})

This function will return the specified <type> value for an achievement of the
attached player or the supplied <char id>. If the player doesn't have the
achievement active (no progress has been made): if the achievement doesn't
exist -1 will be returned, or -2 will be returned on any other error such as
an invalid <type>.

Valid types:
- ACHIEVEINFO_COUNT1
- ACHIEVEINFO_COUNT2
- ACHIEVEINFO_COUNT3
- ACHIEVEINFO_COUNT4
- ACHIEVEINFO_COUNT5
- ACHIEVEINFO_COUNT6
- ACHIEVEINFO_COUNT7
- ACHIEVEINFO_COUNT8
- ACHIEVEINFO_COUNT9
- ACHIEVEINFO_COUNT10
- ACHIEVEINFO_COMPLETE
- ACHIEVEINFO_COMPLETEDATE
- ACHIEVEINFO_GOTREWARD
- ACHIEVEINFO_LEVEL (<achievement id> is useless for this)
- ACHIEVEINFO_SCORE (<achievement id> is useless for this)

---------------------------------------

## achievementcomplete
*achievementcomplete(<achievement id>{,<char id>})

This function will complete an achievement for the attached player or the supplied
<char id>. The objective requirements are ignored when using this function.
Returns true on success and false on failure.

---------------------------------------

## achievementexists
*achievementexists(<achievement id>{,<char id>});

This function will return if the achievement exists on the player or the supplied
<char id>.
Returns true on success and false on failure.

---------------------------------------

## achievementupdate
*achievementupdate(<achievement id>,<type>,<value>{,<char id>})

This function will update an achievement's value for an achievement of the attached
player or the supplied <char id>. If the player does not have the achievement active
(no progress has been made) it will be added to the player's log first before updating
the <type> value.
Returns true on success and false on failure.

See 'achievementinfo' for valid <type> values.
- ACHIEVEINFO_COMPLETE, ACHIEVEINFO_COMPLETEDATE, and ACHIEVEINFO_GOTREWARD require the
  specific value returned from 'gettimetick(2)'.
- Excludes ACHIEVEINFO_LEVEL and ACHIEVEINFO_SCORE.

---------------------------------------