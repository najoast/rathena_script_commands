*select("<option>"{,"<option>",...})
*prompt("<option>"{,"<option>",...})

This function is a handy replacement for 'menu' for some specific cases where
you don't want a complex label structure - like, for example, asking simple yes-
no questions. It will return the number of menu option picked, starting with 1.
Like 'menu', it will also set the variable @menu to contain the option the user
picked.

    if (select("Yes:No" ) == 1)
		mes "You said yes, I know.";

And like 'menu', the selected option is consistent with grouped options
and empty options.

'prompt' works almost the same as select, except that when a character clicks
the Cancel button, this function will return 255 instead.