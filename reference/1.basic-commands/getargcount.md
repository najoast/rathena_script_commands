### getargcount
```
*getargcount()
```

This function is used when you use the 'callsub' or 'callfunc' commands. In the
call you can specify arguments. This function will return the number of arguments
provided.

Example:
```c
	callfunc "funcNPC",5,4,3;
	...
	function%TAB%script%TAB%funcNPC%TAB%{
		.@count = getargcount(); // 3
		...
	}
```
