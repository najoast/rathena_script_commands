### channel_setopt
```
*channel_setopt "<chname>",<option>,<value>;
```

Set option for the channel. Use 1 in <value> to set it, or 0 to unset.
The <option> values are the same as the 'channel_create' options.

For CHAN_OPT_MSG_DELAY, the delay in millisecond must be sent or use 0
to remove the delay at <value>.

Returns 1 on success.

	// Example to set delay
	channel_setopt("#global",CHAN_OPT_MSG_DELAY,5000);

Only for public and private channel.
