
### callshop
```
*callshop "<name>",<option>;
```

These are a series of commands used to create dynamic shops.
The 'callshop' function calls an invisible shop (view -1) as if the player clicked on it.

The options are:
* 0 = The normal window (buy, sell and cancel)
* 1 = The buy window
* 2 = The sell window

Note: The `<option>` parameter only works on the 'shop' type NPC.

A shop called with this command will trigger the labels "OnBuyItem" and "OnSellItem"
(as long as an npcshop* command is executed from that NPC, see note below). These
labels, if used, will replace how the shop handles the buying and selling of items,
allowing for the creation of dynamic shops.

The label "OnBuyItem" sets the following arrays:
* @bought_nameid   - item ID bought
* @bought_quantity - amount bought

The label "OnSellItem" sets the following arrays:
* @sold_nameid        - item ID sold
* @sold_quantity      - amount sold
* @sold_refine        - refine count
* @sold_attribute     - if the item is broken (1) or not (0)
* @sold_identify      - if the item is identified (1) or not (0)
* @sold_card1         - card slot 1
* @sold_card2         - card slot 2
* @sold_card3         - card slot 3
* @sold_card4         - card slot 4
* @sold_option_id1    - random option ID 1
* @sold_option_val1   - random option value 1
* @sold_option_param1 - random option param 1
* @sold_option_id2    - random option ID 2
* @sold_option_val2   - random option value 2
* @sold_option_param2 - random option param 2
* @sold_option_id3    - random option ID 3
* @sold_option_val3   - random option value 3
* @sold_option_param3 - random option param 3
* @sold_option_id4    - random option ID 4
* @sold_option_val4   - random option value 4
* @sold_option_param4 - random option param 4
* @sold_option_id5    - random option ID 5
* @sold_option_val5   - random option value 5
	@sold_option_param5 - random option param 5

Note: These labels will only be triggered if an npcshop* command is executed because these
commands set a special data on the shop NPC, named master_nd in the source. The above labels
are triggered in the NPC whose master_nd is given in the shop.

A full example of a dynamic shop can be found in doc/sample/npc_dynamic_shop.txt.
