
### switch
*switch (expression);

The switch statement is similar to a series of if statements on the same expression.
In many occasions, you may want to compare the same variable (or expression)
with many different values, and execute a different piece of code depending
on which value it equals to. This is exactly what the switch statement is for.

It is important to understand how the switch statement is executed in order
to avoid mistakes. The switch statement executes line by line (actually, statement by statement).
In the beginning, no code is executed. Only when a case statement is found
with a value that matches the value of the switch expression the case statement(s)
will to executed. The parser continues to execute the statements until the end
of the switch block, or the first time it sees a break statement. If you don't
write a break statement at the end of a case's statement list, the parser will
go on executing the statements of the following case (fall-through).

Example 1:
```
	switch(select("Yes:No")) {
		case 1:
			mes "You said yes!";
			break;
		case 2:
			mes "Aww, why?";
			break;
	}
	close;
```
The example above would work like a menu and would go to the first case if
the user selects option, otherwise, would go to the second one.

Example 2:
```
	switch(getgroupid()) {
		case 1:
			mes "Wow, you're super!";
			break;
		case 2:
			mes "A helping hand!";
			break;
		case 3:
			mes "10001010010011";
			break;
		case 4:
			mes "Yes, milord?";
			break;
		default:
			mes "Hello there!";
			break;
	}
```
The example above would print a message depending on the player's groupid.
If there is no statement declared for the corresponding groupid, the script
would use the 'default' statement that applies to rest of possible values,
similar to 'else' in the if-else statement.
