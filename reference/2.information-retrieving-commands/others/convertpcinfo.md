### convertpcinfo
```
*convertpcinfo(<char_id>,<type>)
*convertpcinfo(<account_id>,<type>)
*convertpcinfo(<player_name>,<type>)
```

This function will return the information `<type>` for the
specified character. Whatever it returns is determined by type.

* CPC_NAME    - Character's name.
* CPC_CHAR    - Character ID.
* CPC_ACCOUNT - Account ID.

If a character is not found (or not online) when requesting that information,
an empty string will be returned for CPC_NAME, 0 for other `<type>`.
