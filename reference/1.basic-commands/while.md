
### while
```
*while (<condition>) <statement>;
```

This is probably the simplest and most frequently used loop structure. The 'while'
statement can be interpreted as `while <condition> is true, perform <statement>`.
It is a pretest loop, meaning the conditional expression is tested before any of the
statements in the body of the loop are performed. If the condition evaluates to
false, the statement(s) in the body of the loop is/are never executed. If the
condition evaluates to true, the statement(s) are executed, then control transfers
back to the conditional expression, which is reevaluated and the cycle continues.

Multiple statements can be grouped with { }, curly braces, just like with the 'if' statement.

Example 1:
```c
while (switch(select("Yes:No") == 2 ))
	mes "You picked no.";
close;
```

Example 2: multiple statements
```c
while (switch(select("Yes:No") == 2 )) {
	mes "Why did you pick no?";
	mes "You should pick yes instead!";
}
close;
```

Example 3: counter-controlled loop
```c
.@i = 1;
while (.@i <= 5) {
	mes "This line will print 5 times.";
	.@i += 1;
}
close;
```

Example 4: sentinel-controlled loop
```c
mes "Input 0 to stop";
input .@num;
while (.@num != 0) {
	mes "You entered " + .@num;
	input .@num;
}
close;
```
