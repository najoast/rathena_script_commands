### changecharsex
```
*changecharsex({<char_id>});
```

This command will change the gender of the attached character. If it
was male, it will become female, if it was female, it will become male. The
change will be written to the character server, the player will receive the
message: "Need disconnection to perform change-sex request..." and the player
will be immediately kicked to the login screen. When they log back in, they will
be the opposite sex.

If the character being changed is a Dancer/Gypsy or Bard/Clown class type,
the character will also have their skills reset upon 'changecharsex'.
