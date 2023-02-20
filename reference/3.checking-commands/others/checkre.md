### checkre
```
*checkre(<type>)
```

Checks if a renewal feature is enabled or not in renewal.hpp, and returns 1 if
enabled and 0 for disabled.

The renewal feature to check is determined by type.
```
 0 - RENEWAL (game renewal server mode)
 1 - RENEWAL_CAST (renewal cast time)
 2 - RENEWAL_DROP (renewal drop rate algorithms)
 3 - RENEWAL_EXP (renewal exp rate algorithms)
 4 - RENEWAL_LVDMG (renewal level modifier on damage)
 5 - RENEWAL_ASPD (renewal ASPD)
```
