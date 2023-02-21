### itemskill
```
*itemskill <skill id>,<skill level>{,<keep requirement>};
*itemskill "<skill name>",<skill level>{,<keep requirement>};
```

This command is meant for item scripts to replicate single-use skills in usable
items. It will not work properly if there is a visible dialog window or menu.
If the skill is self or auto-targeting, it will be used immediately; otherwise a
target cursor is shown.

If `<keep requirement>` parameter is set to true, the skill's requirements will be checked.
By default, the requirements for item skills are not checked, and therefore the default value is false.

```c
// When Anodyne is used, it will cast Endure (8), Level 1, as if the actual
// skill has been used from skill tree.
605,Anodyne,Anodyne,11,2000,0,100,,,,,10477567,2,,,,,{ itemskill 8,1; },{}

// When Sienna_Execrate_Scroll_1_5 is used, it will cast Sienna Execrate Level 5 and consume 2 Red_Gemstones.
23194,Sienna_Execrate_Scroll_1_5,Level 5 Sienna Execrate,11,10,,10,,,,,0xFFFFFFFF,63,2,,,,,,{ itemskill "WL_SIENNAEXECRATE",5,true; },{},{}
```
