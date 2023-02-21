### setitemscript
```
*setitemscript(<item id>,<"{ new item script }">{,<type>});
```

Set a new script bonus to the Item. Very useful for game events.
You can remove an item's itemscript by leaving the itemscript argument empty.
Returns 1 on success, or 0 on fail (item_id not found or new item script is invalid).
Type can optionally be used indicates which script to set (default is 0):
* 0 - Script
* 1 - OnEquip_Script
* 2 - OnUnequip_Script

Example:
```c
setitemscript 2637,"{ if (isequipped(2236) == 0)end; if (getskilllv(26)){skill 40,1;}else{skill 26,1+isequipped(2636);} }";
setitemscript 2637,"";
```
