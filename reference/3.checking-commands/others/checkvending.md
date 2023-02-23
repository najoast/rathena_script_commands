### checkvending
```
*checkvending({"<Player Name>"})
```

Checks if the player is vending or has has a buyingstore. Additionally
it gives you the information whether the player uses autotrade or not.
Name is optional, and defaults to the attached player if omitted.

The returned value is bitmask of.
* 0 = doesn't have a vending or buyingstore (which also means he can't use autotrade)
* 1 = normal vending
* 2 = using @autotrade
* 4 = has a buyingstore

Examples:
```c
//This will check Aaron's state
.@state = checkvending("Aaron");
if (.@state&1)
    mes "Aaron is currently vending!";
if (.@state&4)
    mes "Aaron has a buying store!";
if (.@state&2)
    mes "Aaron is autotrading!";
```
