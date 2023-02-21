
### defpattern
```
*defpattern <set number>,"<regular expression pattern>","<event label>";
```
### activatepset
```
*activatepset <set number>;
```
### deactivatepset
```
*deactivatepset <set number>;
```
### deletepset
```
*deletepset <set number>;
```

This set of commands is only available if the server is compiled with regular
expressions library enabled. Default compilation and most binary distributions
aren't, which is probably bad, since these, while complex to use, are quite
fascinating.

They will make the NPC object listen for text spoken publicly by players and
match it against regular expression patterns, then trigger labels associated
with these regular expression patterns.

Patterns are organized into sets, which are referred to by a set number. You can
have multiple sets patterns, and multiple patterns may be active at once.
Numbers for pattern sets start at 1.

'defpattern' will associate a given regular expression pattern with an event
label. This event will be triggered whenever something a player says is matched
by this regular expression pattern, if the pattern is currently active.

'activatepset' will make the pattern set specified active. An active pattern
will enable triggering labels defined with 'defpattern', which will not happen
by default.
'deactivatepset' will deactivate a specified pattern set. Giving -1 as a pattern
set number in this case will deactivate all pattern sets defined.

'deletepset' will delete a pattern set from memory, so you can create a new
pattern set in its place.

Using regular expressions is high wizardry. But with this high wizardry comes
unparalleled power of text manipulation. For an explanation of what a regular
expression pattern is, see a few web pages:

http://www.regular-expressions.info/
http://www.weitz.de/regex-coach/

For an example of this in use, see doc/sample/npc_test_pcre.txt

With this you could, for example, automatically punish players for asking for
Zeny in public places, or alternatively, automatically give them Zeny instead if
they want it so much.
