### getequipcardid
```
*getequipcardid(<equipment slot>,<card slot>)
```

Returns value from equipped item slot in the indicated slot (0, 1, 2, or 3).

This function returns CARD ID, CARD0_FORGE, CARD0_CREATE, or CARD0_PET (for card 0, if the item is produced).
It's useful for when you want to check whether an item contains cards or if it's signed.
