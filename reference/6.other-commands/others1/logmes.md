### logmes
```
*logmes "<message>";
```

This command will write the message given to the map server NPC log file, as
specified in 'conf/log_athena.conf'. In the TXT version of the server, the log
file is 'log/npclog.log' by default. In the SQL version, if SQL logging is
enabled, the message will go to the 'npclog' table, otherwise, it will go to the
same log file.

If logs are not enabled, nothing will happen.
