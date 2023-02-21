### channel_setgroup
```
*channel_setgroup "<chname>",<group_id>{,...,<group_id>};
```
### channel_setgroup2
```
*channel_setgroup2 "<chname>",<array_of_groups>;
```

Set group restriction for a channel. Only player with matching <group_id>
are allowed to to join the channel.

By using 0 in the first group channel, the group restriction will be
removed from the channel config.

'channel_setgroup2' receives input for group list as an array.

Returns 0 on failure, and 1 (or n groups count) on success.

	// Example 1: Remove groups
	channel_setgroup("#event",0);

	// Example 2: Multiple values
	channel_setgroup("#vip",2,5);

	// Example 3: Using array
	setarray .@staffs[0],2,3,4,10,99;
	channel_setgroup("#staff",.@staffs);

Only for public and private channel.
