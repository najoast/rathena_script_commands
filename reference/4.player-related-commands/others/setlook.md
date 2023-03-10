### setlook/changelook
```
*setlook <look type>,<look value>{,<char_id>};
*changelook <look type>,<look value>{,<char_id>};
```

'setlook' will alter the look data for the invoking character. It is used
mainly for changing the palette used on hair and clothes: you specify which look
type you want to change, then the palette you want to use. Make sure you specify
a palette number that exists/is usable by the client you use.
'changelook' works the same, but is only client side (it doesn't save the look value).

```c
// This will change your hair color, so that it uses palette 8, what ever your
// palette 8 is, your hair will use that color
setlook LOOK_HAIR_COLOR,8;

// This will change your clothes color, so they are using palette 1, whatever
// your palette 1 is, your clothes will then use that set of colors.
setlook LOOK_CLOTHES_COLOR,1;
```

Here are the possible look types:

* LOOK_BASE - Base sprite
* LOOK_HAIR - Hairstyle
* LOOK_WEAPON - Weapon
* LOOK_HEAD_BOTTOM - Head bottom
* LOOK_HEAD_TOP - Head top
* LOOK_HEAD_MID - Head mid
* LOOK_HAIR_COLOR - Hair color
* LOOK_CLOTHES_COLOR - Clothes color
* LOOK_SHIELD - Shield
* LOOK_SHOES - Shoes
* LOOK_BODY2 - Body style

Whatever 'shoes' means is anyone's guess, ask Gravity - the client does nothing
with this value. It still wants it from the server though, so it is kept, but
normally doesn't do a thing.

Only the look data for hairstyle, hair color and clothes color are saved to the
char server's database and will persist. Body style will also persist if 'save_body_style'
configuration is enabled in '/conf/battle/client.conf'. The rest freely change as the character
puts on and removes equipment, changes maps, logs in and out and otherwise you
should not expect to set them. In fact, messing with them is generally
hazardous, do it at your own risk, it is not tested what will this actually do -
it won't cause database corruption and probably won't cause a server crash, but
it's easy to crash the client with just about anything unusual.

However, it might be an easy way to quickly check for empty view IDs for
sprites, which is essential for making custom headgear.

Since a lot of people have different palettes for hair and clothes, it's
impossible to tell you what all the color numbers are. If you want a serious
example, there is a Stylist script inside the default rAthena installation that
you can look at: 'npc/custom/stylist.txt'
