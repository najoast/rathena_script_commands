### next

*next;

This command will display a 'next' button in the message window for the
invoking character. Clicking on it will cause the window to clear and display
a new one. Used to segment NPC-talking, next is often used in combination with
'mes' and 'close'.

If no window is currently on screen, one will be created, but once the invoking
character clicks on it, a warning is thrown on the server console and the script
will terminate.

```c
mes "[Woman]";
mes "This would appear on the page";
next;
// This is needed since it is a new page and the top will now be blank
mes "[Woman]";
mes "This would appear on the 2nd page";
```
