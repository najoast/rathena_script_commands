### doevent
```
*doevent "<NPC object name>::<event label>";
```

This command will start a new execution thread in a specified NPC object at the
specified label. The execution of the script running this command will not stop,
and the event called by the 'doevent' command will not run until the invoking
script has terminated. No parameters may be passed with a doevent call.

The script of the NPC object invoked in this manner will run as if it's been
invoked by the RID that was active in the script that issued a 'doevent'. As
such, the command will not work if an RID is not attached.

```c
place,100,100,1%TAB%script%TAB%NPC%TAB%53,{
    mes "This is what you will see when you click me";
    close;
OnLabel:
    mes "This is what you will see if the doevent is activated";
    close;
}

....

doevent "NPC::OnLabel";
```
