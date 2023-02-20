### gettimetick
```
*gettimetick(<tick type>)
```

This function will return a tick depending on `<tick type>`:
* 0: The server's tick, a measurement in milliseconds used by the server's timer system. This tick is an unsigned int which loops every ~50 days.
* 1: The time, in seconds, since the start of the current day.
* 2: The system time in UNIX epoch time, or the number of seconds elapsed since January 1st, 1970. Useful for reliably measuring time intervals.
