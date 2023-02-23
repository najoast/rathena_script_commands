### getequiprefinecost
```
*getequiprefinecost(<equipment slot>,<type>,<information>{,<char id>})
```

This function returns refine cost for equipment in `<equipment slot>` based on
passed arguments `<type>` and `<information>`.

Valid cost types are:
```
REFINE_COST_NORMAL     - For normal refining
REFINE_COST_OVER10     - For refining over +10
REFINE_COST_HD         - For refining with HD ores
REFINE_COST_ENRICHED   - For refining with enriched ores
REFINE_COST_OVER10_HD  - For refining over +10 with HD ores
REFINE_COST_HOLINK     - For refining at Holink in Malangdo
REFINE_COST_WAGJAK     - For refining at Refining Machine Wagjak in the Novice Academy
```

This function will return required cost for refining based on `<information>` argument.

Valid information types are:

```
REFINE_ZENY_COST       - Zeny
REFINE_MATERIAL_ID     - Material Item ID
```

This function will return -1 on failure. The function fails if the cost type
is invalid or if there is no item in the equipment slot. 
