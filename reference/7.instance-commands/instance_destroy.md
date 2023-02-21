
### instance_destroy
```
*instance_destroy {<instance id>};
```

Destroys instance with the ID `<instance id>`. If no ID is specified, the instance
the script is attached to is used. If that fails, the script will come to a halt.
This will also trigger the "OnInstanceDestroy" label in all NPCs inside the instance.
