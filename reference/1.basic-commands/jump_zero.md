### jump_zero
*jump_zero (<condition>),<label>;

This command works kinda like an 'if'+'goto' combination in one go. (See 'if').
If the condition is false (equal to zero) this command will immediately jump to
the specified label like in 'goto'. While 'if' is more generally useful, for
some cases this could be an optimization.

The main reason for this command is that other control statements, like
'switch', 'for' or 'while', are disassembled into simple expressions together
with this command when a script is parsed.
