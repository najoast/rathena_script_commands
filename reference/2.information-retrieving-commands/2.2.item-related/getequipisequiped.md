### getequipisequiped
```
*getequipisequiped(<equipment slot>{,<char_id>})
```

This functions will return 1 if there is an equipment placed on the specified
equipment slot and 0 otherwise. For a list of equipment slots
see 'getequipid'. Function originally used by the refining NPCs:
```c
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
