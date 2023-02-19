
### setarray
*setarray <array name>[<first value>],<value>{,<value>...<value>};

This command will allow you to quickly fill up an array in one go. Check the
Kafra scripts in the distribution to see this used a lot.

    setarray .@array[0], 100, 200, 300, 400, 500, 600;

First value is the index of the first element of the array to alter. For
example:

    setarray .@array[0],200,200,200;
    setarray .@array[1],300,150;

will produce:

 .@array[0]=200
 .@array[1]=300
 .@array[2]=150
