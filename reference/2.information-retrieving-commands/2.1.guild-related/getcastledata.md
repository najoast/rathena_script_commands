### getcastledata
```
*getcastledata("<map name>",<type of data>)
```

This function returns the castle ownership information for the castle referred
to by its map name. Castle information is stored in `guild_castle` SQL table.

Types of data correspond to `guild_castle` table columns:

```
CD_GUILD_ID          - Guild ID.
CD_CURRENT_ECONOMY   - Castle Economy score.
CD_CURRENT_DEFENSE   - Castle Defense score.
CD_INVESTED_ECONOMY  - Number of times the economy was invested in today.
CD_INVESTED_DEFENSE  - Number of times the defense was invested in today.
CD_NEXT_TIME         - unused
CD_PAY_TIME          - unused
CD_CREATE_TIME       - unused
CD_ENABLED_KAFRA     - Is 1 if a Kafra was hired for this castle, 0 otherwise.
CD_ENABLED_GUARDIAN0 - Is 1 if the 1st guardian is present (Soldier Guardian)
CD_ENABLED_GUARDIAN1 - Is 1 if the 2nd guardian is present (Soldier Guardian)
CD_ENABLED_GUARDIAN2 - Is 1 if the 3rd guardian is present (Soldier Guardian)
CD_ENABLED_GUARDIAN3 - Is 1 if the 4th guardian is present (Archer Guardian)
CD_ENABLED_GUARDIAN4 - Is 1 if the 5th guardian is present (Archer Guardian)
CD_ENABLED_GUARDIAN5 - Is 1 if the 6th guardian is present (Knight Guardian)
CD_ENABLED_GUARDIAN6 - Is 1 if the 7th guardian is present (Knight Guardian)
CD_ENABLED_GUARDIAN7 - Is 1 if the 8th guardian is present (Knight Guardian)
```

All types of data have their meaning determined by War of Emperium scripts,
with exception of:
* `CD_GUILD_ID` that is always considered ID of the guild that owns the castle,
* `CD_CURRENT_DEFENSE` that is used in Guardians & Emperium HP calculations,
* `CD_ENABLED_GUARDIANX` that is always considered to hold guardian presence bits.
