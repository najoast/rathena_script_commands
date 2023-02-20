### pushpc
```
*pushpc <direction>,<cells>;
```

This command will push the currently attached player to given direction by given
amount of square cells. Direction is the same as used when declaring NPCs, and
can be specified by using one of the DIR_* constants (src/map/script_constants.hpp).

The knock-back is not restricted by items or map flags, only obstacles are taken
into account. If there is not enough space to perform the push (e.g. due to a
wall), the character is pushed only up to the obstacle.

```c
// pushes the character 5 cells in 3 o'clock direction from its
// current position.
pushpc DIR_EAST, 5;
```
