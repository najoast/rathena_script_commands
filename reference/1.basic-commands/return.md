### return
*return {<value>};

This command causes the script execution to leave previously called function
with callfunc or script with callsub and return to the location, where the call
originated from. Optionally a return value can be supplied, when the call was
done using the function form.

Using this command outside of functions or scripts referenced by callsub will
result in error and termination of the script.

	callfunc "<your function>";// when nothing is returned
	set <variable>,callfunc("<your function>");// when a value is being returned
