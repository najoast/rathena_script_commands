### npcshopdelitem
```
*npcshopdelitem "<name>",<item id>{,<item id>{,<item id>{,...}}};
```

This command will remove items from the specified NPC shop or cashshop.
If the item to remove exists more than once on the shop, all instances will be
removed.

Note that the function returns 1 even if no items were removed. The return
value is only to confirm that the shop was indeed found.
