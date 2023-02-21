
# if
```
*if (<condition>) <statement>;
```

This is the basic conditional statement command, and just about the only one
available in this scripting language.

The condition can be any expression. All expressions resulting in a non-zero
value will be considered True, including negative values. All expressions
resulting in a zero are false.

If the expression results in True, the statement will be executed. If it isn't
true, nothing happens and we move on to the next line of the script.

```c
if (1)  mes "This will always print.";
if (0)  mes "And this will never print.";
if (5)  mes "This will also always print.";
if (-1) mes "Funny as it is, this will also print just fine.";
```

For more information on conditional operators see the operators section above.
Anything that is returned by a function can be used in a condition check without
bothering to store it in a specific variable:

```c
if (strcharinfo(0) == "Daniel Jackson") mes "It is true, you are Daniel!";
```

More examples of using the 'if' command in the real world:

## Example 1:
```c
.@answer = 1;
input .@input;
if (.@input == .@answer)
	close;
mes "Sorry, your answer is incorrect.";
close;
```

## Example 2:
```c
.@answer = 1;
input .@input;
if (.@input != .@answer)
	mes "Sorry, your answer is incorrect.";
close;
```
Notice that examples 1 and 2 have the same effect.

## Example 3:
```c
.@count++;
mes "[Forgetful Man]";
if (.@count == 1) mes "This is the first time you have talked to me.";
if (.@count == 2) mes "This is the second time you have talked to me.";
if (.@count == 3) mes "This is the third time you have talked to me.";
if (.@count == 4) {
	mes "This is the fourth time you have talked to me.";
	mes "I think I am getting amnesia, I have forgotten about you...";
	.@count = 0;
}
close;
```

## Example 4:
```c
mes "[Quest Person]";
if (countitem(512) < 1) {  // 512 is the item ID for Apple, found in item_db
	mes "Can you please bring me an apple?";
	close;
}
mes "Oh, you brought an Apple!";
mes "I didn't want it, I just wanted to see one.";
close;
```

## Example 5:
```c
mes "[Person Checker]";
if ($@name$ == "") {  // global variable not yet set
	mes "Please tell me someones name";
	next;
	input $@name$;
	$@name2$ = strcharinfo(0);
	mes "[Person Checker]";
	mes "Thank you.";
	close;
}
if ($@name$ == strcharinfo(0)) {  // player name matches $@name$
	mes "You are the person that " + $@name2$ + " just mentioned.";
	mes "Nice to meet you!";

	// reset the global variables
	$@name$ = "";
	$@name2$ = "";

	close;
}
mes "You are not the person that " + $name2$ + " mentioned.";
close;
```
See 'strcharinfo' for an explanation of what this function does.

## Example 6: Using complex conditions.
```c
mes "[Multiple Checks]";
if (@queststarted == 1 && countitem(512) >= 5) {
	mes "Well done, you have started the quest and brought me 5 Apples.";
	@queststarted = 0;
	delitem 512,5;
	close;
}
mes "Please bring me 5 apples.";
@queststarted = 1;
close;
```

The script engine also supports nested 'if' statements:
```c
if (<condition>)
	dothis;
else
	dothat;
```

If the condition isn't met, it'll do the action following the 'else'.
We can also group several actions depending on a condition:
```c
if (<condition>) {
	dothis1;
	dothis2;
} else {
	dothat1;
	dothat2;
	dothat3;
}
```

Remember that if you plan to do several actions upon the condition being false, and
you forget to use the curly braces (the { } ), the second action will be executed regardless
the output of the condition, unless of course, you stop the execution of the script if the
condition is true (that is, in the first grouping using a return; , and end; or a close; )

Also, you can have multiple conditions nested or chained.

```c
if (<condition 1>)
	dothis;
else if (<condition 2>) {
	dothat;
	end;
} else
	dothis;
```
