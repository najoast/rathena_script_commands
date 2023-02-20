### setriding
```
*setriding {<flag>{,<char_id>}};
*checkriding({<char_id>});
```

If `<flag>` is 0 this command will remove the mount from the character.
Otherwise it gives the invoking character a PecoPeco (if they are a Knight
series class), a GrandPeco (if they are a Crusader series class), or
a Gryphon (if they are a Royal Guard). Unlike 'setfalcon' and 'setcart'
this will not work at all if they aren't of a class which can ride.
Note: the character needs to have the skill KN_RIDING to gain a mount

The accompanying function will return 1 if the invoking character is riding a
bird and 0 if they aren't.

```c
if (checkriding())
    mes "PLEASE leave your bird outside! No riding birds on the floor here!";
```