### changesex
```
*changesex({<char_id>});
```

This command will change the gender for the attached character's account. If it
was male, it will become female, if it was female, it will become male. The
change will be written to the character server, the player will receive the
message: "Need disconnection to perform change-sex request..." and the player
will be immediately kicked to the login screen. When they log back in, they will
be the opposite sex.

If there are any Dancer/Gypsy or Bard/Clown characters on the account,
they will also have their skills reset upon 'changesex'.
