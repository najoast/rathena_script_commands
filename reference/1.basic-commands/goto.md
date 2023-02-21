### goto
```
*goto <label>;
```

This command will make the script jump to a label, usually used in conjunction
with other command, such as "if", but often used on its own.

```c
	...
	goto Label;

	mes "This will not be seen";
	end;
Label:
	mes "This will be seen";
	end;
```

This command should be avoided and only used if there is no other option.
