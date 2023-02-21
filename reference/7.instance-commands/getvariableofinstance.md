### getvariableofinstance
```
*getvariableofinstance(<variable>,<instance id>);
```

Returns a reference to an instance variable (' prefix) of the specific instance ID.
This can only be used to get ' variables.

Examples:
```c
// This will set the .@s variable to the value of 'var variable of the specific instance ID.
set .@s, getvariableofinstance('var, instance_id(IM_PARTY));

// This will set the 'var variable of the specific instance ID to 1.
set getvariableofinstance('var, instance_id(IM_GUILD)), 1;
```
