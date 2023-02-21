### getitembound
```
*getitembound <item id>,<amount>,<bound type>{,<account ID>};
*getitembound "<item name>",<amount>,<bound type>{,<account ID>};
```

This command behaves identically to 'getitem', but the items created will be
bound to the target character as specified by the bound type. All items created
in this manner cannot be dropped, sold, vended, auctioned, or mailed, and in
some cases cannot be traded or stored.

Valid bound types are:
* `Bound_Account` : Account Bound item
* `Bound_Guild`   : Guild Bound item
* `Bound_Party`   : Party Bound item
* `Bound_Char`    : Character Bound item
