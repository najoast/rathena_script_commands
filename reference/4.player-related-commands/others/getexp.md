### getexp
```
*getexp <base_exp>,<job_exp>{,<char_id>};
```

This command will give the invoking character a specified number of base and job
experience points. Used for a quest reward. Negative values won't work.

The EXP values are adjustted by 'quest_exp_rate' config value, VIP bonus, Guild
Tax and EXP boost items such Battle Manual, Bubble Gum, or items that have
SC_EXPBOOST or SC_ITEMBOOST.

	getexp 10000,5000;
