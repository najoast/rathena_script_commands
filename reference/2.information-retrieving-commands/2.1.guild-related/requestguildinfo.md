### requestguildinfo
```
*requestguildinfo <guild id>{,"<event label>"};
```

This command requests the guild data from the char server and merrily continues
with the execution. Whenever the guild information becomes available (which
happens instantly if the guild information is already in memory, or later, if it
isn't and the map server has to wait for the char server to reply) it will run
the specified event as in a 'donpcevent' call.
