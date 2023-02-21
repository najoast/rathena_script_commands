### achievementupdate
```
*achievementupdate(<achievement id>,<type>,<value>{,<char id>})
```

This function will update an achievement's value for an achievement of the attached
player or the supplied <char id>. If the player does not have the achievement active
(no progress has been made) it will be added to the player's log first before updating
the <type> value.
Returns true on success and false on failure.

See 'achievementinfo' for valid <type> values.
- ACHIEVEINFO_COMPLETE, ACHIEVEINFO_COMPLETEDATE, and ACHIEVEINFO_GOTREWARD require the
  specific value returned from 'gettimetick(2)'.
- Excludes ACHIEVEINFO_LEVEL and ACHIEVEINFO_SCORE.
