
# is_function
```
*is_function("<function name>")
```

This command checks whether a function exists.
It returns 1 if function is found, or 0 if it isn't.

Example:
```
function	script	try	{
	dothat;
}

-	script	test	-1,{
	.@try = is_function("try"); // 1
	.@not = is_function("not"); // 0
}
```
