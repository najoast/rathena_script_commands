### cooking
```
*cooking <dish level>;
```

This command will open a produce window on the client connected to the invoking
character. The 'dish level' is the number which determines what kind of dish
level you can produce. You can see the full list of dishes that can be produced in
'db/produce_db.txt'.

The window will be shown empty if the invoking character does not have enough of
the required incredients to cook a dish.

Valid dish levels are:

* 11 - Level 1 Dish
* 12 - Level 2 Dish
* 13 - Level 3 Dish
* 14 - Level 4 Dish
* 15 - Level 5 Dish
* 16 - Level 6 Dish
* 17 - Level 7 Dish
* 18 - Level 8 Dish
* 19 - Level 9 Dish
* 20 - Level 10 Dish

Although it's required to set a dish level, it doesn't matter if you set it to 1
and you want to cook a level 10 dish, as long as you got the required incredients
to cook the dish the command works.
