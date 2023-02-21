### achievementinfo
```
*achievementinfo(<achievement id>,<type>{,<char id>})
```

This function will return the specified `<type>` value for an achievement of the
attached player or the supplied `<char id>`. If the player doesn't have the
achievement active (no progress has been made): if the achievement doesn't
exist -1 will be returned, or -2 will be returned on any other error such as
an invalid `<type>`.

Valid types:
* `ACHIEVEINFO_COUNT1`
* `ACHIEVEINFO_COUNT2`
* `ACHIEVEINFO_COUNT3`
* `ACHIEVEINFO_COUNT4`
* `ACHIEVEINFO_COUNT5`
* `ACHIEVEINFO_COUNT6`
* `ACHIEVEINFO_COUNT7`
* `ACHIEVEINFO_COUNT8`
* `ACHIEVEINFO_COUNT9`
* `ACHIEVEINFO_COUNT10`
* `ACHIEVEINFO_COMPLETE`
* `ACHIEVEINFO_COMPLETEDATE`
* `ACHIEVEINFO_GOTREWARD`
* `ACHIEVEINFO_LEVEL` (`<achievement id>` is useless for this)
* `ACHIEVEINFO_SCORE` (`<achievement id>` is useless for this)
