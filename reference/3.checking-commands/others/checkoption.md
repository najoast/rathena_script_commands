
### checkoption
```
*checkoption(<option number>{,<char_id>})
*checkoption1(<option number>{,<char_id>})
*checkoption2(<option number>{,<char_id>})
*setoption <option number>{,<flag>{,<char_id>}};
```

The 'setoption' series of functions check for a so-called option that is set on
the invoking character. 'Options' are used to store status conditions and a lot
of other non-permanent character data of the yes-no kind. For most common cases,
it is better to use 'checkcart','checkfalcon','checkriding' and other similar
functions, but there are some options which you cannot get at this way. They
return 1 if the option is set and 0 if the option is not set.

Option numbers valid for the first (option) version of this command are:

```
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
```

Option numbers valid for the second version (opt1) of this command are:

```
1 - Petrified.
2 - Frozen.
3 - Stunned.
4 - Sleeping.
6 - Petrifying (the state where you can still walk)
```

Option numbers valid for the third version (opt2) of this command are:

```
0x1  - Poisoned.
0x2  - Cursed.
0x4  - Silenced.
0x8  - Signum Crucis (plays a howl-like sound effect, but otherwise no visible effects are displayed)
0x10 - Blinded.
0x80 - Deadly poisoned.
```

Option numbers (except for opt1) are bit-masks - you can add them up to check
for several states, but the functions will return true if at least one of them
is in effect.

'setoption' will set options on the invoking character. There are no second and
third versions of this command, so you can only change the values in the first
list (cloak, cart, ruwach, etc). if flag is 1 (default when omitted),
the option will be added to what the character currently has; if 0, the option is removed.

This is definitely not a complete list of available option flag numbers. Ask a
core developer (or read the source: src/map/status.hpp) for the full list.
