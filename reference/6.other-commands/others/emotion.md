### emotion
```
*emotion <emotion number>{,<target>};
```

This command makes an object display an emotion sprite above their own as
if they were doing that emotion. For a full list of emotion numbers,
see 'src/map/script_constants.hpp' under 'ET_'. The not so obvious ones are 'ET_QUESTION'
(a question mark) and 'ET_SURPRISE' (the exclamation mark).

The optional target parameter specifies who will get the emotion on top of
their head. Use the target Game ID (GID).
