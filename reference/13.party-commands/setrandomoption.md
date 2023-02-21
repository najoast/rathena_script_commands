### setrandomoption
```
*setrandomoption(<equipment slot>,<index>,<id>,<value>,<param>{,<char id>});
```

Sets <index+1>th random option for equipment equipped at <equipment slot>
to <id>, <value> and <param>.

See 'getequipid' for a full list of valid equipment slots.

index parameter can be 0 to MAX_ITEM_RDM_OPT-1 (default 0-4).

ID - ID of random option. See db/const.txt for constants.
Value - Value of random option
Param - Parameter of random option
