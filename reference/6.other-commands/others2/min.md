
### min
```
*min(<number or array>{,<number or array>,...})
```
### minimum
```
*minimum(<number or array>{,<number or array>,...})
```
### max
```
*max(<number or array>{,<number or array>,...})
```
### maximum
```
*maximum(<number or array>{,<number or array>,...})
```

Returns the smallest (or biggest) from the set of given parameters.
These parameters have to be either numbers or number arrays.

Example:
```c
.@minimum = min( 1, -6, -2, 8, 2 ); // .@minimum will be equal to -6
.@maximum = max( 0, 5, 10, 4 ); // .@maximum will be equal to 10
.@level = min( BaseLevel, 70 ); // .@level will be the character's base level, capped to 70

setarray .@testarray, 4, 5, 12, 6, 7, 3, 8, 9, 10;

.@minimum = min( .@testarray ); // .@minimum will be equal to 3
.@maximum = max( .@testarray ); // .@maximum will be equal to 12

.@minimum = min( -6, 1, 2, 3, .@testarray ); // .@minimum will be equal to -6
.@maximum = max( -6, 1, 2, 3, .@testarray ); // .@maximum will be equal to 12
```
