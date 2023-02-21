### getguildalliance
```
*getguildalliance(<guild id1>, <guild id2>);
```

This command will return the relation between 2 guilds.

NOTE: This should be used in collaboration with 'requestguildinfo' as the
map-server needs to request for information from the char-server.

Return values:
* -2 - Guild ID1 does not exist
* -1 - Guild ID2 does not exist
*  0 - Both guilds have no relation OR guild ID aren't given
*  1 - Both guilds are allies
*  2 - Both guilds are antagonists
