### makeitem2
```
*makeitem2 <item id>,<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>;
*makeitem2 "<item name>",<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>;
*makeitem3 <item id>,<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>;
*makeitem3 "<item name>",<amount>,"<map name>",<X>,<Y>,<identify>,<refine>,<attribute>,<card1>,<card2>,<card3>,<card4>,<RandomIDArray>,<RandomValueArray>,<RandomParamArray>;
```

This command will create an item on the specified cell of a map. See 'makeitem' for
further details.

See 'getitem2' for an explanation of the expanded parameters.

'makeitem3' is advance version of 'makeitem2' that also use Item Random Option as additional values.
* `<RandomIDArray>`    : Array variable of ID for item random option, see `db/[pre-]re/item_randomopt_db.txt`
* `<RandomValueArray>` : Array variable of item random option's value.
* `<RandomParamArray>` : Array variable of item random option's param.

Example to get Crimson Weapon with Ghost property:
```c
// 0.5% chance to get +0 Valkyrie Shield [1]
// with Neutral Resistance +10% and 5% damage reduction from Demi-Human or Player
// when Valkyrie Randgris killed
OnNPCKillEvent:
    if (killedrid == 1751 && rand(0,1000) > 950) { // Valkyrie Randgris
        getmapxy(.@map$,.@x,.@y,BL_PC);
        setarray .@OptID[0],RDMOPT_ATTR_TOLERACE_NOTHING,RDMOPT_RACE_TOLERACE_HUMAN;
        setarray .@OptVal[0],10,5;
        setarray .@OptParam[0],0;
        makeitem3 2115,1,.@map$,.@x,.@y,0,0,0,0,0,0,0,.@OptID,.@OptVal,.@OptParam;
    }
    end;
```
