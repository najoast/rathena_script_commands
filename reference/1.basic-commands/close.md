### close
```
*close;
```

This command will create a 'close' button in the message window for the invoking
character. If no window is currently on screen, the script execution will end. This is one
of the ways to end a speech from an NPC. Once the button is clicked, the NPC
script execution will end, and the message box will disappear.

```
	mes "[Woman]";
	mes "I am finished talking to you. Click the close button.";
	close;
	mes "This command will not run at all, since the script has ended.";
```

### close2
```
*close2;
```

This command will create a 'close' button in the message window for the invoking
character. WARNING: If no window is currently on screen, the script execution will halt
indefinitely! See 'close'. There is one important difference, though - even though
the message box will have closed, the script execution will not stop, and commands after
'close2' will still run, meaning an 'end' has to be used to stop the script, unless you
make it stop in some other manner.

```
	mes "[Woman]";
	mes "I will warp you now.";
	close2;
	warp "place",50,50;
	end;
```

Don't expect things to run smoothly if you don't make your scripts 'end'.