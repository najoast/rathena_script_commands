### setbattleflag
```
*setbattleflag "<battle flag>",<value>{,<reload>};
```
### getbattleflag
```
*getbattleflag("<battle flag>")
```

Sets or gets the value of the given battle flag.
Battle flags are the flags found in the battle / *.conf files and is also used in Lupus' variable rates script.
If the reload value is given then the server will attempt to reload monster data
to properly apply the new rates. This applies to EXP/Drop type configs. The server
will only attempt to reload specific configs.

Examples:

```c
// Will set the base experience rate to 20x (2000%) - Monster data will continue to use previous rates at server start
setBattleFlag "base_exp_rate",2000;

// Will set the base experience rate to 20x (2000%) - Monster data will be reloaded to new value
setBattleFlag "base_exp_rate",2000,true;

// Will return the value of the base experience rate (when used after the above example, it would print 2000).
mes getBattleFlag("base_exp_rate");
```
