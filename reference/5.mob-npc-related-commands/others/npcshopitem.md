### npcshopitem
```
*npcshopitem "<name>",<item id>,<price>{,<item id>,<price>{,<item id>,<price>{,...}}};
```

This command lets you override the contents of an existing NPC shop or cashshop. The
current sell list will be wiped, and only the items specified with the price
specified will be for sale.

The function returns 1 if shop was updated successfully, or 0 if not found.

NOTES:
- That you cannot use -1 to specify default selling price!
- If attached shop type is market shop, need an extra param after price, it's `<qty>` and make sure don't add duplication item!
