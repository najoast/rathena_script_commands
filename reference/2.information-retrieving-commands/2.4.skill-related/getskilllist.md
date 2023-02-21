### getskilllist
```
*getskilllist({<char_id>});
```

This command sets a bunch of arrays with a complete list of skills the
invoking character has. Here's what you get:

```
@skilllist_id[]   - skill ids.
@skilllist_lv[]   - skill levels.
@skilllist_flag[] - see 'skill' for the meaning of skill flags.
@skilllist_count  - number of skills in the above arrays.
```

While 'getskillv' is probably more useful for most situations, this is the
easiest way to store all the skills and make the character something else for a
while. Advanced job for a day? This could also be useful to see how many
skills a character has.

This command does not count skills which are set as flag 4 (permament granted) (ALL_BUYING_STORE/ALL_INCCARRY)
