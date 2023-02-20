### basicskillcheck
*basicskillcheck()

This function will return the state of the configuration option
'basic_skill_check' in 'battle_athena.conf'. It returns 1 if the option is
enabled and 0 if it isn't. If the 'basic_skill_check' option is enabled, which
it is by default, characters must have a certain number of basic skill levels to
sit, request a trade, use emotions, etc. Making your script behave differently
depending on whether the characters must actually have the skill to do all these
things might in some cases be required.
