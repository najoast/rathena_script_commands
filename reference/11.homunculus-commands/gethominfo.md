### gethominfo
```
*gethominfo(<type>{,<char_id>})
```

This function will return Homunculus information for the Homunculus of the
invoking character, regardless of its vaporize state. It returns zero or
"null" if the player does not own a Homunculus.

Valid types are:
 0 - Homunculus ID
 1 - Homunculus Class
 2 - Homunculus Name
 3 - Homunculus friendly level (intimacy score). 100000 is full loyalty.
 4 - Homunculus hungry level. 100 is completely full.
 5 - Homunculus rename flag. 0 means this homunculus has not been named yet.
 6 - Homunculus level
 7 - Homunculus Game ID
