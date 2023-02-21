### transform
```
*transform <monster ID>,<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
*transform "<monster name>",<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
```
### active_transform
```
*active_transform <monster ID>,<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
*active_transform "<monster name>",<duration>{,<sc type>,<val1>,<val2>,<val3>,<val4>};
```

This command will turn a player into a monster for a given duration and can grant
a SC attribute effect while transformed. Note that players cannot be transformed
during War of Emperium or if already disguised.
Can only be removed when you die or the duration ends.

'transform' and 'active_transform' can stack on each other but using 'transform' or
'active_transform' twice will not stack (it will cancel the previous bonus for the new).
'active_transform' will take priority over transform for its duration.
