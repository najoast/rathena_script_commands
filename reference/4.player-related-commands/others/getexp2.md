### getexp2
```
*getexp2 <base_exp>,<job_exp>{,<char_id>};
```

This command is safety version of 'set' command for BaseExp and JobExp. If using
'set' while the BaseExp or JobExp value is more than 2,147,483,647 (INT_MAX) will
causing overflow error.

Unlike 'getexp', this command ignores the adjustment factors!
