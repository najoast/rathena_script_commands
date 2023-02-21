These commands will only work if the invoking character has a pet, and are meant
to be executed from pet scripts. They will modify the pet AI decision-making for
the current pet of the invoking character, and will NOT have any independent
effect by themselves, which is why only one of them each may be in effect at any
time for a specific pet. A pet may have 'petloot', 'petskillbonus',
'petskillattack' OR 'petpetskillattack2' and 'petskillsupport'.

All commands with delays and durations will only make the behavior active for
the specified duration of seconds, with a delay of the specified number of
seconds between activations. Rates are a chance of the effect occurring and are
given in percent. 'bonusrate' is added to the normal rate if the pet intimacy is
at the maximum possible.

The behavior modified with the below mentioned commands will only be exhibited if
the pet is loyal and appropriate configuration options are set in
'battle_athena.conf'.

Pet scripts in the database normally run whenever a pet of that type hatches
from the egg. Other commands usable in item scripts (see 'bonus') will also
happily run from pet scripts. Apparently, the pet-specific commands will also
work in NPC scripts and modify the behavior of the current pet up until the pet
is hatched again. (Which will also occur when the character is logged in again
with the pet still out of the egg.) It is not certain for how long the effect of
such command running from an NPC script will eventually persist, but apparently,
it is possible to usefully employ them in usable item scripts to create pet
buffing items.

Nobody tried this before, so you're essentially on your own here.
