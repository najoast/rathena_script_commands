### channel_chat
```
*channel_chat "<chname>","<message>"{,<color>};
```

Sends message to the channel.
Returns 1 on success.

	// Example if channel doesn't have alias
	channel_chat(#rathena,"Hello World!"); // #rathena Hello World!

	// Example if channel has alias
	channel_chat(#rathena,"Hello World!"); // [rAthena] Hello World!
