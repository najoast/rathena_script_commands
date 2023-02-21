### pet
```
*pet <pet id>;
```
### catchpet
```
*catchpet <pet id>;
```

This command is used in all the item scripts for taming items. Running this
command will make the pet catching cursor appear on the client connected to the
invoking character, usable on the monsters with the specified pet ID number. It
will still work outside an item script.

If the `<pet id>` is `PET_CATCH_UNIVERSAL` the item will attempt to catch the targeted
monster as long as it is in the pet database and does not have the MD_STATUS_IMMUNE
monster mode.

If the `<pet id>` is `PET_CATCH_UNIVERSAL_ITEM` the item will attempt to catch the targeted
monster as long as it is in the pet database and the targeted monster requires the lure
item used.

See 'doc/mob_db_mode_list.txt' for more information about monster modes.

A full list of pet IDs can be found inside 'db/(pre-)re/pet_db.yml'.
