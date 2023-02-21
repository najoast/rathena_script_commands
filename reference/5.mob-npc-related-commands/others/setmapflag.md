### setmapflag
```
*setmapflag "<map name>",<flag>{,<zone>{,<type>}};
```

This command marks a specified map with the given map flag, which will alter the
behavior of the map. A full list of mapflags is located in 'src/map/script_constants.hpp' with
the 'mf_' prefix, and documentation can be found in 'doc/mapflags.txt'.

The map flags alter the behavior of the map regarding teleporting (mf_nomemo,
mf_noteleport, mf_nowarp, mf_nogo), storing location when disconnected
(mf_nosave), dead branch usage (mf_nobranch), penalties upon death
(mf_nopenalty, mf_nozenypenalty), PVP behavior (mf_pvp, mf_pvp_noparty,
mf_pvp_noguild), WoE behavior (mf_gvg,mf_gvg_noparty), ability to use
skills or open up trade deals (mf_notrade, mf_novending, mf_noskill, mf_noicewall),
current weather effects (mf_snow, mf_fog, mf_sakura, mf_leaves, mf_rain, mf_clouds,
mf_fireworks) and whether night will be in effect on this map (mf_nightenabled).

The optional parameter `<zone>` is used to set the zone for 'restricted' mapflags,
GM level bypass for 'nocommand', base/job experience for 'bexp'/'jexp', and
flag for 'battleground'.

For 'skill_damage' mapflag:
- Setting the flag here will adjust the global (all skills) damage on the map.
- `<zone>` is the -100 to 100000 damage adjustment value of the skills.
- See 'getmapflag' for the different `<type>` values.

For 'skill_duration' mapflag:
- `<zone>` is the skill ID to adjust.
- `<type>` is the percentage of adjustment from 0 to 100000.
