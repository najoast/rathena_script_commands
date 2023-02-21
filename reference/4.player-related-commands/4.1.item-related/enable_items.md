### enable_items
```
*enable_items;
*disable_items;
```

These commands toggle the ability to change equipment while interacting with
an NPC. To avoid possible exploits, the commands affect the particular script
instance only. Note that if a different script also calls enable_items, it
will override the last call (so you may want to call this command at the start
of your script without assuming it is still in effect).

The default setting, 'item_enabled_npc', is defined in 'conf/battle/items.conf'.
