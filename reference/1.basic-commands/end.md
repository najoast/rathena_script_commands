### end
```
*end;
```

This command will stop the execution for this particular script. The two
versions are perfectly equivalent. It is the normal way to end a script which
does not use 'mes'.

```c
if (BaseLevel <= 10)
	npctalk "Look at that you are still a n00b";
else if (BaseLevel <= 20)
	npctalk "Look at that you are getting better, but still a n00b";
else if (BaseLevel <= 30)
	npctalk "Look at that you are getting there, you are almost 2nd profession now right???";
else if (BaseLevel <= 40)
	npctalk "Look at that you are almost 2nd profession";
end;
```

Without the use of 'end' it would travel through the labels until the end of the
script. If you were lvl 10 or less, you would see all the speech lines, the use
of 'end' stops this, and ends the script.
