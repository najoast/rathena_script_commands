### bg_get_data
```
*bg_get_data(<Battle Group>,<type>);
```

Retrieves data related to given Battle Group. Type can be one of the following:

	0 - Amount of players currently belonging to the group.
	1 - Store GID of players in <Battle Group> in a temporary global array $@arenamembers
		and returns amount of players currently belonging to the group.
