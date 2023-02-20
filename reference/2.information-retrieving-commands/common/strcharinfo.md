### strcharinfo
```c
*strcharinfo(<type>{,<char_id>})
```

This function will return either the name, party name or guild name for the
invoking character. Whatever it returns is determined by type.

* 0 - Character's name.
* 1 - The name of the party they're in if any.
* 2 - The name of the guild they're in if any.
* 3 - The name of the map the character is in.

If a character is not a member of any party or guild, an empty string will be
returned when requesting that information.