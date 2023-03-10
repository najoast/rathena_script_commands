### roclass
```
*roclass(<job number>{,<gender>})
```

Does the opposite of eaclass. That is, given an eA job-number, it returns the
corresponding RO class number. A gender is required because both Bard and Dancers
share the same eA job-number (EAJ_BARDDANCER), and uses the invoking player's
gender if none is given (if no player is attached, male will be used by default).
The command will return -1 if there is no valid class to represent the specified
job (for example, if you try to get the baby version of a Taekwon class).

```c
.@eac = eaclass();
//Check if class is already rebirth
if (.@eac&EAJL_UPPER) {
    mes "You look strong.";
    close;
}
.@eac = roclass(.@eac|EAJL_UPPER);
//Check if class has a rebirth version
if (.@eac != -1) {
    mes "Bet you can't wait to become a " + jobname(.@eac) + "!";
    close;
}
```
