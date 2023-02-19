
### function
```
*function <function name>;
*<function name>{(<argument>,...<argument>)};
*function <function name> {
    <code>
}
```

This works like callfunc, and is used for cleaner and faster scripting. The function
must be defined and used within a script, and works like a label with arguments.
Note that the name may only contain alphanumeric characters and underscore.

Usage:

    1. Declare the function.
	function <function name>;
    2. Call the function anywhere within the script.
       It can also return a value when used with parentheses.
	<function name>;
    3. Define the function within the script.
	<function name> {<code>}

Example:
```
prontera,154,189,4	script	Item Seller	767,{
	/* Function declaration */
	function SF_Selling;

	if (Zeny > 50) {
		mes "Welcome!";
		/* Function call */
		SF_Selling;
	}
	else mes "You need 50z, sorry!";
	close;

	/* Function definition */
	function SF_Selling {
		mes "Would you like to buy a phracon for 50z?";
		next;
		if (select("Yes","No, thanks") == 1) {
			Zeny -= Zeny;
			getitem 1010,1;
			mes "Thank you!";
		}
		return;
	}
}
```

Example with parameters and return value:
```
prontera,150,150,0	script	TestNPC	123,{
	/* Function declaration */
	function MyAdd;

	mes "Enter two numbers.";
	next;
	input .@a;
	input .@b;
	/* Function call */
	mes .@a + " + " + .@b + " = " + MyAdd(.@a,.@b);
	close;

	/* Function definition */
	function MyAdd {
		return getarg(0)+getarg(1);
	}
}
```
