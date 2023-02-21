### day
```
*day;
```
### night
```
*night;
```

These two commands will switch the entire server between day and night mode
respectively. If your server is set to cycle between day and night by
configuration, it will eventually return to that cycle.

Example:

```c
-	script	DayNight	-1,{
OnClock0600:
	day;
	end;
OnInit:
	// setting correct mode upon server start-up
	if (gettime(DT_HOUR)>=6 && gettime(DT_HOUR)<18) end;
OnClock1800:
	night;
	end;
}
```

This script allows to emulate the day/night cycle as the server does, but also
allows triggering additional effects upon change, like announces, gifts, etc.
The day/night cycle set by configuration should be disabled when this script is used.
