### getstatus
```
*getstatus(<effect type>{,<type>{,<char_id>}})
```

Retrieve information about a specific status effect when called. Depending on `<type>`
specified the function will return different information.

Possible `<type>` values:
- 0 or undefined: whether the status is active
- 1: the val1 of the status
- 2: the val2 of the status
- 3: the val3 of the status
- 4: the val4 of the status
- 5: the amount of time in milliseconds that the status has remaining

If `<type>` is not defined or is set to 0, then the script function will either
return 1 if the status is active, or 0 if the status is not active. If the status
is not active when any of the `<type>` fields are provided, this script function
will always return 0.
