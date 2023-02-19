
### callfunc
```
*callfunc "<function>"{,<argument>,...<argument>};
*callfunc("<function>"{,<argument>,...<argument>})
```

This command lets you call up a function NPC. A function NPC can be called from
any script on any map server. Using the 'return' command it will come back to
the place that called it.

```
	place,50,50,6%TAB%script%TAB%Woman%TAB%115,{
		mes "[Woman]"
		mes "Let's see if you win...";
		callfunc "funcNPC";
		mes "Well done, you have won!";
		close;
	}
	function%TAB%script%TAB%funcNPC%TAB%{
		.@win = rand(2);
		if (.@win == 0)
			return;
		mes "Sorry, you lost.";
		close;
	}
```

You can pass arguments to your function - values telling it what exactly to do -
which will be available there with getarg() (see 'getarg')
Notice that returning is not mandatory, you can end execution right there.

If you want to return a real value from inside your function NPC, it is better
to write it in the function form, which will also work and will make the script
generally cleaner:

```
	place,50,50,6%TAB%script%TAB%Man%TAB%115,{
		mes "[Man]"
		mes "Gimme a number!";
		next;
		input .@number;
		if (callfunc("OddFunc",.@number)) mes "It's Odd!";
		close;
	}
	function%TAB%script%TAB%OddFunc%TAB%{
		if (getarg(0)%2 == 0)
			return 0;// it's even
		return 1;// it's odd
	}
```

Alternately, as of rAthena revision 15979 and 15981, user-defined functions
may be called directly without the use of the 'callfunc' script command.

```
	function<tab>script<tab>SayHello<tab>{
		mes "Hello " + getarg(0);
		return 0;
	}

	place,50,50,6<tab>script<tab>Man<tab>115,{
		mes "[Man]";
		SayHello strcharinfo(0);
		close;
	}
```

Note:

```
 !! A user-defined function must be declared /before/ a script attempts to
 !! call it. That is to say, any functions should be placed above scripts or NPCs
 !! (or loaded in a separate file first) before attempting to call them directly.
```
