
### do
```
*do { <statement>; } while (<condition>);
```

The 'do...while' is the only post-test loop structure available in this script
language. With a post-test, the statements are executed once before the condition
is tested. When the condition is true, the statement(s) are repeated. When the
condition is false, control is transferred to the statement following the
'do...while' loop expression.

Example 1: sentinel-controlled loop
```
	mes "This menu will keep appearing until you pick Cancel";
	do {
		.@menu = select("One:Two:Three:Cancel");
	} while (.@menu != 4);
```

Example 2: counter-controlled loop
```
	mes "This will countdown from 10 to 1.";
	.@i = 10;
	do {
		mes .@i;
		.@i -= 1;
	} while (.@i > 0);
```

