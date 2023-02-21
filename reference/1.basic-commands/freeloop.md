
### freeloop
```
*freeloop({<toggle>})
```

Toggling this to enabled (1) allows the script instance to bypass the infinite loop
protection, allowing your script to loop as much as it may need. Disabling (0) will
warn you if an infinite loop is detected.

The command will return the state of freeloop for the attached script, even if no
argument is provided.

Example:
```c
freeloop(1); // enable script to loop freely

// be careful with what you do here
for ( .@i = 0; .@i < .@bigloop; .@i++ ) {
	dothis;
	// will sleep the script for 1ms when detect an infinity loop to
	// let rAthena do what it needs to do (socket, timer, process, etc.)
}

freeloop(0); // disable freeloop

for ( .@i = 0; .@i < .@bigloop; .@i++ ) {
	dothis;
	// throw an infinity loop error
}
```
