### setmadogear
```
*setmadogear {<flag>{,<char_id>}};
*checkmadogear({<char_id>});
```

If `<flag>` is 0 this command will remove the mount from the character.
Otherwise it gives the invoking character a Mado (if they are a Mechanic).

The accompanying function will return 1 if the invoking character has a
Mado and 0 if they don't.
