### ignoretimeout
```c
*ignoretimeout <flag>{,<char_id>};
```

Disables the SECURE_NPCTIMEOUT function on the character invoking the script,
or by the given character ID/character name.

Valid flag:
* 0 - Enabled SECURE_NPCTIMEOUT.
* 1 - Disable SECURE_NPCTIMEOUT.

Note: SECURE_NPCTIMEOUT must be enabled for this to work.
