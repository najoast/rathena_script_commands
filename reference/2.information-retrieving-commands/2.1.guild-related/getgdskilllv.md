### getgdskilllv
```
*getgdskilllv(<guild id>,<skill id>)
*getgdskilllv(<guild id>,"<skill name>")
```

This function returns the level of the skill `<skill id>` of the guild `<guild id>`.
If the guild does not have that skill, 0 is returned.
If the guild does not exist, -1 is returned.
Refer to 'db/(pre-)re/skill_db.txt' for the full list of skills. (GD_* are guild skills)
