### setquestinfo_req
```
*setquestinfo_req <quest_id>,<req_quest_id>,<state>{,<req_quest_id>,<state>,...};
```

Add 'req_quest_id' as requirement for quest info with quest id 'quest_id'.

Value os 'state' are:
* 0: Player doesn't started 'req_quest_id'.
* 1: Player has 'req_quest_id' (state is either "inactive" or "active").
* 2: Player has 'req_quest_id' completed

This command must be used after 'questinfo'.
