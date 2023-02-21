### gettimestr
```
*gettimestr(<"time format">,<max length>{,<time_tick>})
```

This function will return a string containing time data as specified by the
time format.

This uses the C function 'strfmtime', which obeys special format characters. For
a full description see, for example, the description of 'strfmtime' at
http://www.delorie.com/gnu/docs/glibc/libc_437.html
All the format characters given in there should properly work.
Max length is the maximum length of a time string to generate.

The example given in rAthena sample scripts works like this:

```c
mes gettimestr("%Y-%m/%d %H:%M:%S",21);
```

The example above will print the current date and time like 'YYYY-MM/DD HH:MM:SS'.
The following example will print the date and time when the player's VIP status
expires by the given `<time_tick>`:

```c
mes gettimestr("%Y-%m/%d %H:%M:%S",21,vip_status(VIP_STATUS_EXPIRE));
```
