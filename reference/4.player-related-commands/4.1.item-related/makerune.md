### makerune
```
*makerune <% success bonus>{,<char_id>};
```

This command will open a rune crafting window on the client connected to the
invoking character. Since this command is officially used in rune ores, a bonus
success rate must be specified (which adds to the base formula).

You can see the full list of runes that can be produced in 'db/produce_db.txt'.
The window will not be empty only if the invoking character can actually produce
a rune and has the appropriate raw materials in their inventory.
