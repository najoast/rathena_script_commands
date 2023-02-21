
### channel_create
```
*channel_create "<chname>","<alias>"{,"<password>"{<option>{,<delay>{,<color>{,<char_id>}}}}};
```

Creates a public channel with `<chname>` as the channel name. To protect the
channel, use `<password>` or write "null" to create it without a password.
Channel name must start with '#' and cannot be the same as the map or ally
channel names.

`<alias>` will be used to change the channel name when the channel message
is displayed.

`<option>` values are:
* `CHAN_OPT_BASE`		    - Default option including CHAN_OPT_ANNOUNCE_SELF|CHAN_OPT_MSG_DELAY|CHAN_OPT_CAN_CHAT|CHAN_OPT_CAN_LEAVE
* `CHAN_OPT_ANNOUNCE_SELF`  - Show info for player itself if player has joined/leaves the channel
* `CHAN_OPT_ANNOUNCE_JOIN`  - Display message when player is joining the channel
* `CHAN_OPT_ANNOUNCE_LEAVE` - Display message when player is leaving the channel
* `CHAN_OPT_MSG_DELAY`	    - Enable chat delay for the channel
* `CHAN_OPT_COLOR_OVERRIDE` - Player's unique font color will override channel's color
* `CHAN_OPT_CAN_CHAT`	    - Player can chat in the channel
* `CHAN_OPT_CAN_LEAVE`	    - Player can leave the channel
* `CHAN_OPT_AUTOJOIN`	    - Players will auto join the channel at login

The `<delay>` is the minimum chat delay in millisecond for a single player before
the player can chat again in the same channel.

Use `<color>` hex code to set the color for this channel, if not defined, default
channel color will be used.

If `<char_id>` is defined, the channel will be a private channel and the player
will be the the channel owner.

Returns 1 on success.

```c
/**
	* This example will shows the message on this channel as
	* [rAthena] Admin : Hello world!
	* instead of
	* #rathena Admin : Hello world!
	**/
channel_create("#rathena","[rAthena]");
channel_create("#vip","[VIP]","vipmemberonly");
```
