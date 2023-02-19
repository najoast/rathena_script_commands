### set
*set <variable>,<expression>{,<char_id>};
*set(<variable>,<expression>{,<char id>})

This command will set a variable to the value that the expression results in.
Variables may either be set through this command or directly, much like any
other programming language (refer to the "Assigning variables" section).

This is the most basic script command and is used a lot whenever you try to do
anything more advanced than just printing text into a message box.

	set .@x,100;

will make .@x equal 100.

	set .@x,1+5/8+9;

will compute 1+5/8+9 (which is, surprisingly, 10 - remember, all numbers are
integer in this language) and make .@x equal it.

Returns the variable reference (since trunk r12870).