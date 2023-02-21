### guildopenstorage
```
*guildopenstorage()
```

This function works the same as 'openstorage' but will open a guild storage
window instead for the guild storage of the guild the invoking character belongs
to.

Return values:
* GSTORAGE_OPEN - Successfully opened.
* GSTORAGE_STORAGE_ALREADY_OPEN - Player storage is already open.
* GSTORAGE_ALREADY_OPEN - Guild storage is already open.
* GSTORAGE_NO_GUILD - Player is not in a guild.
* GSTORAGE_NO_STORAGE - Guild hasn't invested in the Guild Storage Expansion skill (only if OFFICIAL_GUILD_STORAGE is enabled).
* GSTORAGE_NO_PERMISSION - Player doesn't have permission to use the guild storage.
