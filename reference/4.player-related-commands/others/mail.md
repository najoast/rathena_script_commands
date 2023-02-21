### mail
```
*mail <destination id>,"<sender name>","<title>","<body>"{,<zeny>{,<item id array>,<item amount array>{,<item card0 array>{,<item card1 array>{,<item card2 array>{,<item card3 array>
		{,<random option id0 array>, <random option value0 array>, <random option paramter0 array>{,<random option id1 array>, <random option value1 array>, <random option paramter1 array>
		{,<random option id2 array>, <random option value2 array>, <random option paramter2 array>{,<random option id3 array>, <random option value3 array>, <random option paramter3 array>
		{,<random option id4 array>, <random option value4 array>, <random option paramter4 array>}}}}}}};
```

This command will send mail to the `<destination id>` which is a character ID.
A `<sender name>` can be specified but does not have to be from the direct creator
of the mail and is limited to NAME_LENGTH (24) characters. Mail `<title>` is limited
to MAIL_TITLE_LENGTH (40) characters. Mail `<body>` is limited to MAIL_BODY_LENGTH
(200) characters for PACKETVER `< 20150513 or 500 characters for later clients.

Optional `<zeny>` and item data can be added to the mail as well. PACKETVER `< 20150513
is limited to 1 item while later clients are limited to MAIL_MAX_ITEM (5).

The `<item id array>`, `<item amount array>`, `<item card0 array>`, `<item card1 array>`,
`<item card2 array>`, and `<item card3 array>` should all be integer arrays.

For random options there can be 5 arrays in pairs of 3 (ids, values, parameters) right after the cards.
All of these arrays shall be integer arrays as well.

Example of sending mail with zeny:
```c
.@charid = getcharid(0);
.@sender$ = "Poring";
.@title$ = "Welcome";
.@body$ = "Hi! I'm a simple Poring from the Prontera fields! Welcome to Ragnarok!";
.@zeny = 5000;
mail .@charid, .@sender$, .@title$, .@body$, .@zeny;
```

Example of sending mail with items:
```c
.@charid = getcharid(0);
.@sender$ = "Angeling";
.@title$ = "Welcome";
.@body$ = "Hi! I'm a simple Angeling from the Prontera fields! Welcome to Ragnarok!";
.@zeny = 0;
setarray .@mailitem[0], 504, 505, 2220, 1214; // White Potion, Blue Potion, Hat, Dagger
setarray .@mailamount[0], 10, 5, 1, 1; // 10 White Potions, 5 Blue Potions, 1 Hat, 1 Dagger
setarray .@mailcard0[0], 0, 0, 4198, 4092; // Attach Maya Purple Card to the Hat, Attach Skeleton Worker Card to Dagger
setarray .@mailcard1[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
setarray .@mailcard2[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
mail .@charid, .@sender$, .@title$, .@body$, .@zeny, .@mailitem, .@mailamount, .@mailcard0, .@mailcard1, .@mailcard2;
```

Example of sending mail with items and random options:
```c
.@charid = getcharid(0);
.@sender$ = "Angeling";
.@title$ = "Welcome";
.@body$ = "Hi! I'm a simple Angeling from the Prontera fields! Welcome to Ragnarok!";
.@zeny = 0;
setarray .@mailitem[0], 504, 505, 2220, 1214; // White Potion, Blue Potion, Hat, Dagger
setarray .@mailamount[0], 10, 5, 1, 1; // 10 White Potions, 5 Blue Potions, 1 Hat, 1 Dagger
setarray .@mailcard0[0], 0, 0, 4198, 4092; // Attach Maya Purple Card to the Hat, Attach Skeleton Worker Card to Dagger
setarray .@mailcard1[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
setarray .@mailcard2[0], 0, 0, 0, 4092; // Attach Skeleton Worker Card to Dagger
setarray .@mailcard3[0], 0, 0, 0, 0; // Empty last slot
setarray .@mailrndopt_id0[0], 0, 0, 0, 0, RDMOPT_VAR_MAXHPAMOUNT; // Enchant the Dagger with increased HP option
setarray .@mailrndopt_val0[0], 0, 0, 0, 0, 1000; // Enchant the Dagger with increased HP option by 1000 points
setarray .@mailrndopt_prm0[0], 0, 0, 0, 0, 0; // Enchant the Dagger with increased HP option - does not need any parameter
mail .@charid, .@sender$, .@title$, .@body$, .@zeny, .@mailitem, .@mailamount, .@mailcard0, .@mailcard1, .@mailcard2, .@mailcard3, .@mailrndopt_id0, .@mailrndopt_val0, .@mailrndopt_prm0;
```
