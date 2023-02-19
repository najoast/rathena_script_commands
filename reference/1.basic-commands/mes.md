### mes
*mes "<string>"{,"<string>"{,...}};

This command will display a box on the screen for the invoking character, if no
such box is displayed already, and will print the string specified into that
box. There is normally no 'close' or 'next' button on this box, unless you
create one with 'close' or 'next', and while it's open the player can't do much
else, so it's important to create a button later. If the string is empty, it
will show up as an empty line.

	mes "Text that will appear in the box";

### Colors

Inside the string you may put color codes, which will alter the color of the
text printed after them. The color codes are all `'^<R><G><B>'` and contain three
hexadecimal numbers representing colors as if they were HTML colors - `^FF0000` is
bright red, `^00FF00` is bright green, `^0000FF` is bright blue, `^000000` is black.
^FF00FF is a pure magenta, but it's also a color that is considered transparent
whenever the client is drawing windows on screen, so printing text in that color
will have kind of a weird effect. Once you've set a text's color to something,
you have to set it back to black unless you want all the rest of the text be in
that color:
```
	mes "This is ^FF0000 red ^000000 and this is ^00FF00 green, ^000000 so.";
```
Notice that the text coloring is handled purely by the client. If you use non-
English characters, the color codes might get screwed if they stick to letters
with no intervening space. Separating them with spaces from the letters on
either side solves the problem.

### Multiple Lines

To display multiple lines of message while only using a single 'mes' command,
use the script command in the following format:
```
	mes "Line 1", "Line 2", "Line 3";
```
This will display 3 different lines while only consuming a single line in
the relevant script file.

### Navigation

For clients dated 2011-10-10aRagexe onwards, you can generate navigation links
using HTML-like labels:
```
	<NAVI>Display Name<INFO>mapname,x,y,0,000,flag</INFO></NAVI>
```

The "flag" parameter can be:
* 0: Do not open Navigation Window (default).
* 1: Open Navigation Window.

The example below will make the [Tool Shop] text clickable and begin navigation
to alberta (98,154) when clicked.
```
	mes "Have you checked out the <NAVI>[Tool Shop]<INFO>alberta,98,154,0,000,0</INFO></NAVI>?";
```
See also [navigateto](##navigateto), which can be used for certain NPC events.

### Items

You can refer to items by using HTML-like links to certain items:

	<ITEMLINK>Display Name<INFO>Item ID</INFO></ITEMLINK>

Where <Display Name> is the name that will be displayed for your link and
<Item ID> being the ID of the item you want to link to when clicked.

In 2015 the tag name was changed to <ITEM> resulting in the following syntax:

	<ITEM>Display Name<INFO>Item ID</INFO></ITEM>

The following sample will open a preview window for Red Potion:

	mes "Did you ever consume a <ITEMLINK>Red Potion<INFO>501</INFO></ITEMLINK>?";
	// Or in 2015:
	mes "Did you ever consume a <ITEM>Red Potion<INFO>501</INFO></ITEM>?";

NOTE: Be aware that item links are rendered incorrectly in 2015+ clients at the moment.

### URLs

Similarly, you can create links to websites that launch in a new window:

	<URL>Display Name<INFO>http://www.example.com/</INFO></URL>";
