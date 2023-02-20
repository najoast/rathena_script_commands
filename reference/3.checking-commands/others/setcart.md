### setcart/checkcart
```
*setcart {<type>{,<char_id>}};
*checkcart({<char_id>});
```

If `<type>` is 0 this command will remove the cart from the character.
Otherwise it gives the invoking character a cart. The cart given will be
cart number `<type>` and will work regardless of whether the character is a
merchant class or not.
Note: the character needs to have the skill MC_PUSHCART to gain a cart

The accompanying function will return 1 if the invoking character has a cart
(any kind of cart) and 0 if they don't.

```c
if (checkcart())
    mes "But you already have a cart!";
```
