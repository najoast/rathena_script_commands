### setpcblock
```
*setpcblock `<type>`,<state>{,<account ID>};
```
### getpcblock
```
*getpcblock {<account ID>};
```

'setpcblock' command prevents/allows the player from doing the given `<type>` of action according
to the `<state>` during the player session (note: @reloadscript removes all `<type>` except PCBLOCK_IMMUNE).
The `<type>` values are bit-masks, multiples of `<type>` can be added to change the player action.

The action is blocked when the `<state>` is true, while false allows the action again.

'getpcblock' command return the bit-mask value of the currently
enabled block flags.

Available `<type>`:
* `PCBLOCK_MOVE`		Prevent the player from moving.
* `PCBLOCK_ATTACK`		Prevent the player from attacking.
* `PCBLOCK_SKILL`		Prevent the player from using skills/itemskills.
* `PCBLOCK_USEITEM`		Prevent the player from using usable items.
* `PCBLOCK_CHAT`		Prevent the player from sending global/guild/party/whisper messages.
* `PCBLOCK_IMMUNE`		Prevent the player from being hit by monsters.
* `PCBLOCK_SITSTAND`	Prevent the player from sitting/standing.
* `PCBLOCK_COMMANDS`	Prevent the player from using atcommands/charcommands.
* `PCBLOCK_NPCCLICK`	Prevent the player from clicking/touching any NPC/shop/warp.
* `PCBLOCK_EMOTION`		Prevent the player from using emotions.
* `PCBLOCK_NPC`			Simulate NPC interaction. Useful for NPC with no mes window. Sum of PCBLOCK_MOVE|PCBLOCK_SKILL|PCBLOCK_USEITEM|PCBLOCK_COMMANDS|PCBLOCK_NPCCLICK.
* `PCBLOCK_ALL`			Sum of all the flags.

Examples:

```c
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
```
