### npcshopadditem
```
*npcshopadditem "<name>",<item id>,<price>{,<item id>,<price>{,<item id>,<price>{,...}}};
```

This command will add more items at the end of the selling list for the
specified NPC shop or cashshop. If you specify an item already for sell, that item will
appear twice on the sell list.

The function returns 1 if shop was updated successfully, or 0 if not found.

NOTES:
- That you cannot use -1 to specify default selling price!
- If attached shop type is market shop, need an extra param after price, it's `<qty>` and make sure don't add duplication item!
