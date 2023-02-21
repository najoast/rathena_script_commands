### getpetinfo
```
*getpetinfo(<type>{,<char_id>})
```

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
