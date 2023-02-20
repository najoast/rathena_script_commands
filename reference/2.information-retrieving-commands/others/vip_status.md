### vip_status
```
*vip_status(<type>,{"<character name>"})
```

Returns various information about a player's VIP status.

Valid types:
* VIP_STATUS_ACTIVE - VIP status: true if the player is a VIP or false if not
* VIP_STATUS_EXPIRE - VIP expire timestamp if the player is VIP or 0 if not
* VIP_STATUS_REMAINING - VIP time remaining in seconds

NOTE: This command is only available if the VIP System is enabled.
