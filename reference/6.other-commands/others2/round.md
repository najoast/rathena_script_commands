### round
```
*round(<number>,<precision>);
```
### ceil
```
*ceil(<number>,<precision>);
```
### floor
```
*floor(<number>,<precision>);
```

Returns `<number>` rounded to multiple of `<precision>`.

`round` function will round the `<number>` up if its division with `<precision>` yield a remainder
with a value equals to or more than half of `<precision>`. Otherwise, it rounds the `<number>` down.
`ceil` always round the `<number>` up.
`floor` always round the `<number>` down.
