### input
```
*input(<variable>{,<min>{,<max>}})
```

This command will make an input box pop up on the client connected to the
invoking character, to allow entering of a number or a string. This has many
uses, one example would be a guessing game, also making use of the 'rand'
function:

```c
mes "[Woman]";
mes "Try and guess the number I am thinking of.";
mes "The number will be between 1 and 10.";
next;
.@number = rand(1,10);
input .@guess;
if (.@guess == .@number) {
	mes "[Woman]";
	mes "Well done, that was the number I was thinking of!";
	close;
} else {
	mes "[Woman]";
	mes "Sorry, that wasn't the number I was thinking of.";
	close;
}
```

If you give the input command a string variable to put the input in, it will
allow the player to enter text. Otherwise, only numbers will be allowed.

```c
mes "[Woman]";
mes "Please say HELLO";
next;
input .@var$;
if (.@var$ == "HELLO") {
	mes "[Woman]";
	mes "Well done, you typed it correctly.";
	close;
} else {
	mes "[Woman]";
	mes "Sorry, you got it wrong.";
	close;
}
```

Normally you may not input a negative number with this command.
This is done to prevent exploits in badly written scripts, which would
let people, for example, put negative amounts of Zeny into a bank script and
receive free Zeny as a result.

Since trunk r12192 the command has two optional arguments and a return value.
The default value of 'min' and 'max' can be set with 'input_min_value' and
'input_max_value' in script_athena.conf.
For numeric inputs the value is capped to the range `[min,max]`. Returns 1 if
the value was higher than 'max', -1 if lower than 'min' and 0 otherwise.
For string inputs it returns 1 if the string was longer than 'max', -1 is
shorter than 'min' and 0 otherwise.
