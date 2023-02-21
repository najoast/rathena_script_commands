### openstorage2
```
*openstorage2 <storage_id>,<mode>{,<account_id>};
```

Just like the 'openstorage' command, except this command can open additional storages
by the specified <storage_id>. For <storage_id>, please read the conf/inter_server.yml
for storage groups.

Values for <mode> are:
* STOR_MODE_NONE : Player only can read the storage entries.
* STOR_MODE_GET  : Player can get items from the storage.
* STOR_MODE_PUT  : Player can put items in the storage.

Example:
```c
if (vip_status(VIP_STATUS_ACTIVE)) {
    mes "I will open your Premium storage.";
    mes "Thank you for using our service.";
    close2;
    openstorage2 1,STOR_MODE_GET|STOR_MODE_PUT;
} else {
    mes "Sorry, your Premium status is expired.";
    mes "Storage will be opened but you can't put any item into it.";
    close2;
    openstorage2 1,STOR_MODE_GET;
}
end;
```
