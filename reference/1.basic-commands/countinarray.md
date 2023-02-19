### countinarray
*countinarray <array name>{[<start index>]},<array name>{[<start index>]};

This command will check for matches between the array values and return the number of matches.
While being optional, if [<start index>] is supplied, the search will begin from the given index value.

	setarray .@array[0], 100, 200, 300, 400, 500, 600;
	
	.@variable = 100;
	if(countinarray(.@array[0], .@variable))
		mes "The number 100 was found in the array .@array";
	
	countinarray(.@array[0], .@variable);
	//return 1 because the number 100 is an element of the array .@array
	
	setarray .@array2[0],100,500;
	countinarray(.@array[0], .@array2[0]);
	//return 2 because the numbers 100 and 500 are elements of the array .@array
	
	setarray .@array3[0],100,700;
	countinarray(.@array[0], .@array3[0]);
	//return 1 because the number 100 is an element of the array .@array
	//but the number 700 is not an element of the array .@array

	//also you can change the position between the arrays in the command
	if(countinarray(.@array[0], .@array3[0]) == countinarray(.@array3[0], .@array[0]))
		//This is true

For more details, see the sample in 'doc/sample/inarray.txt'.
