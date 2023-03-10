### soundeffect
```
*soundeffect "<effect filename>",<type>;
```
### soundeffectall
```
*soundeffectall "<effect filename>",<type>{,"<map name>"}{,<x0>,<y0>,<x1>,<y1>};
```

These two commands will play a sound effect to either the invoking character
only ('soundeffect') or multiple characters ('soundeffectall'). If the running
code does not have an object ID (a 'floating' NPC) or is not running from an NPC
object at all (an item script) the sound will be centered on the character who's
RID got attached to the script, if any. If it does, it will be centered on that
object. (an NPC sprite)

Effect filename is the filename in a GRF. It must have the .wav extension.

It's not quite certain what the 'type' actually does, it is sent to the client
directly. It probably determines which directory to play the effect from.
It's certain that giving 0 for the number will play sound files from '\data\wav\',
but where the other numbers will read from is unclear.

The sound files themselves must be in the PCM format, and file names should also
have a maximum length of 23 characters including the .wav extension:

```c
soundeffect "1234567890123456789.wav", 0; // this will play the soundeffect
soundeffect "12345678901234567890.wav", 0; // throw gravity error
```

You can add your own effects this way, naturally.
