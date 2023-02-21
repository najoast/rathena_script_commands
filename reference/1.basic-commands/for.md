
### for
```
*for (<variable initialization>; <condition>; <variable update>) <statement>;
```

Another pretest looping structure is the 'for' statement. It is considered a
specialized form of the 'while' statement, and is usually associated with counter-
controlled loops. Here are the steps of the 'for' statement: the initialize
statement is executed first and only once. The condition test is performed.
When the condition evaluates to false, the rest of the for statement is skipped.
When the condition evaluates to true, the body of the loop is executed, then the
update statement is executed (this usually involves incrementing a variable).
Then the condition is reevaluated and the cycle continues.

Example 1:
```c
for( .@i = 1; .@i <= 5; .@i++ )
	mes "This line will print 5 times.";
```

Example 2:
```c
mes "This will print the numbers 1 - 5.";
for( .@i = 1; .@i <= 5; .@i++ )
	mes "Number: " + .@i;
```
