### getitem2
```
*getitem2 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*getitem2 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>{,<account ID>};
*getitem3 <item id>,<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
*getitem3 "<item name>",<amount>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>{,<account ID>};
```

This command will give an amount of specified items to the invoking character.
If an optional account ID is specified, and the target character is currently
online, items will be created in their inventory instead. If they are not
online, nothing will happen. It works essentially the same as 'getitem' but is
a lot more flexible.

Those parameters that are different from 'getitem' are:

```
identify    - Whether you want the item to be identified (1) or not (0).
refine      - For how many pluses will it be refined.
              It will not let you refine an item higher than the max refine.
attribute   - Whether the item is broken (1) or not (0).
card1,2,3,4 - If you want a card compound to it, place the card ID number into
              the specific card slot.
```

Card1-card4 values are also used to store name information for named items, as
well as the elemental property of weapons and armor. You can create a named item
in this manner, however, if you just need a named piece of standard equipment,
it is much easier to the 'getnameditem' function instead.

You will need to keep these values if you want to destroy and then perfectly
recreate a named item, for this see 'getinventorylist'.

If you still want to try creating a named item with this command because
'getnameditem' won't do it for you cause it's too limited, you can do it like
this. Careful, minor magic ahead.

```c
// First, let's get an ID of a character who's name will be on the item.
// Only an existing character's name may be there.
// Let's assume our character is 'Adam' and find his ID.
.@charid = getcharid(0,"Adam");

// Now we split the character ID number into two portions with a binary
// shift operation. If you don't understand what this does, just copy it.
.@card3 = .@charid & 65535;
.@card4 = .@charid >> 16;

// If you're inscribing non-equipment, .@card1 must be 254.
// Arrows are also not equipment.
.@card1 = 254;

// For named equipment, card2 means the Star Crumbs and elemental
// crystals used to make this equipment. For everything else, it's 0.
.@card2 = 0;

// Now, let's give the character who invoked the script some
// Adam's Apples:
getitem2 512,1,1,0,0,.@card1,.@card2,.@card3,.@card4;
```

This wasn't tested with all possible items, so I can't give any promises,
experiment first before relying on it.

To create equipment, continue this example it like this:

```c
// We've already have card3 and card4 loaded with correct
// values so we'll just set up card1 and card2 with data
// for an Ice Stiletto.

// If you're inscribing equipment, .@card1 must be 255.
.@card1 = 255;

// That's the number of star crumbs in a weapon.
.@sc = 2;

// That's the number of elemental property of the weapon.
.@ele = 1;

// And that's the wacky formula that makes them into
// a single number.
.@card2 = .@ele+((.@sc*5)<<8);

// That will make us an Adam's +2 VVS Ice Stiletto:
getitem2 1216,1,1,2,0,.@card1,.@card2,.@card3,.@card4;
```

Experiment with the number of star crumbs - I'm not certain just how much will
work most and what it depends on. The valid element numbers are:
* 1 - Ice
* 2 - Earth
* 3 - Fire
* 4 - Wind.

You can, apparently, even create duplicates of the same pet egg with this
command, creating a pet which is the same, but simultaneously exists in two
eggs, and may hatch from either, although, I'm not sure what kind of a mess will
this really cause.

'getitem3' is advance version of 'getitem2' that also use Item Random Option as additional values.
* `<RandomIDArray>`    : Array variable of ID for item random option, see `db/[pre-]re/item_randomopt_db.txt`
* `<RandomValueArray>` : Array variable of item random option's value.
* `<RandomParamArray>` : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
```c
// +9 Crimson Dagger [2]
setarray .@OptID[0],RDMOPT_WEAPON_ATTR_TELEKINESIS;
setarray .@OptVal[0],0;
setarray .@OptParam[0],0;
getitem3 28705,1,1,9,0,0,0,0,0,.@OptID,.@OptVal,.@OptParam;
```
