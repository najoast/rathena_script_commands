### npcshopattach
```
*npcshopattach "<name>"{,<flag>};
```

This command will attach the current script to the given NPC shop.
When a script is attached to a shop, the events "OnBuyItem" and "OnSellItem"
of your script will be executed whenever a player buys/sells from the shop.
Additionally, the arrays @bought_nameid[], @bought_quantity[] or @sold_nameid[]
and @sold_quantity[] will be filled up with the items and quantities
bought/sold.

The optional parameter specifies whether to attach ("1") or detach ("0") from
the shop (the default is to attach). Note that detaching will detach any NPC
attached to the shop, even if it's from another script, while attaching will
override any other script that may be already attached.

The function returns 0 if the shop was not found, 1 otherwise.

NOTES:
- If attached shop type is market shop, will be default to call the 'buy' window.
