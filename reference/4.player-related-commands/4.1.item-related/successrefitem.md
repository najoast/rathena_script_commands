### successrefitem
```
*successrefitem <equipment slot>{,<count>{,<char_id>}};
```

This command will refine an item in the specified equipment slot of the invoking
character by +1, or a count if given. For a list of equipment slots see 'getequipid'.
This command will also display a 'refine success' effect on the character and put
appropriate messages into their chat window. It will also give the character fame
points if a weapon reached +10 this way, even though these will only take effect for
blacksmith who will later forge a weapon.
