### setd
*setd "<variable name>",<value>{,<char_id>};

Works almost identically as set, except the variable name is identified as a string
and can thus be constructed dynamically.

This command is equivalent to:
	set getd("variable name"),<value>;

Examples:

	setd ".@var$", "Poporing";
	mes .@var$; // Displays "Poporing".

	setd ".@" + .@var$ + "123$", "Poporing is cool";
	mes .@Poporing123$; // Displays "Poporing is cool".

NOTE:
	'char_id' only works for non-server variables.
	Player with Character ID 'char_id' must be online.
