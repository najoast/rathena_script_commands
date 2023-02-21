### adopt
```
*adopt("<parent_name>","<baby_name>");
*adopt(<parent_id>,<baby_id>);
```

This function will send the client adoption request to the specified baby
character. The parent value can be either parent. Both parents and the baby
need to be online in order for adoption to work.

Return values:
* `ADOPT_ALLOWED` - Sent message to Baby to accept or deny.
* `ADOPT_ALREADY_ADOPTED` - Character is already adopted.
* `ADOPT_MARRIED_AND_PARTY` - Parents need to be married and in a party with the baby.
* `ADOPT_EQUIP_RINGS` - Parents need wedding rings equipped.
* `ADOPT_NOT_NOVICE` - Baby is not a Novice.
* `ADOPT_CHARACTER_NOT_FOUND` - A parent or Baby was not found.
* `ADOPT_MORE_CHILDREN` - You cannot adopt more than 1 child. (client message)
* `ADOPT_LEVEL_70` - Parents need to be at least level 70 in order to adopt someone. (client message)
* `ADOPT_MARRIED` - You cannot adopt a married person. (client message)
