### getequipisenableref
```
*getequipisenableref(<equipment slot>{,<char_id>})
```

Will return 1 if the item equipped on the invoking character in the specified
equipment slot is refinable, and 0 if it isn't. For a list of equipment slots
see 'getequipid'.
```c
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
