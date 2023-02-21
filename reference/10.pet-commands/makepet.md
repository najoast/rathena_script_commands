### makepet
```
*makepet <pet id>;
```

This command will create a pet egg and put it in the invoking character's
inventory. The kind of pet is specified by pet ID numbers listed in
'db/(pre-)re/pet_db.yml'. The egg is created exactly as if the character just successfully
caught a pet in the normal way.

```c
// This will make you a poring:
makepet 1002;
```

Notice that you absolutely have to create pet eggs with this command. If you try
to give a pet egg with 'getitem', pet data will not be created by the char
server and the egg will disappear when anyone tries to hatch it.
