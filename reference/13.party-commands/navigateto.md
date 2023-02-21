
### navigateto
```
*navigateto("<map>"{,<x>,<y>,<flag>,<hide_window>,<monster_id>,<char_id>});
```

Generates a navigation for attached or specified character. Requires client
2011-10-10aRagEXE or newer.

The flag specifies how the client will calculate the specific route.

Valid flags are:
 NAV_NONE - No services
 NAV_AIRSHIP_ONLY - Airship only
 NAV_SCROLL_ONLY - Scroll only
 NAV_AIRSHIP_AND_SCROLL - Airship and Scroll
 NAV_KAFRA_ONLY - Kafra only
 NAV_KAFRA_AND_AIRSHIP - Kafra and Airship
 NAV_KAFRA_AND_SCROLL - Kafra and Scroll
 NAV_ALL - All services

When flag is not specified, the default value is NAV_KAFRA_AND_AIRSHIP.

The hide_window specifies whether to display (0) or hide (1) the navigation window.
By default the window is hidden.

You can specify the monster_id in combination with a mapname to make the
navigation system tell you, that you have reached the desired mob.

Note:
The client requires custom monster spawns be in the navigation file
for using the embedded client Navigation feature to work properly. In this
instance sending the player to the map where the monster spawns is a simpler
solution rather than sending the map and the monster_id.
