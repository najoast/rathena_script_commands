### gettime
```
*gettime(<type>)
```

This function will return specified information about the current system time.

* DT_SECOND - Seconds (of the current minute)
* DT_MINUTE - Minutes (of the current hour)
* DT_HOUR - Hour (of the current day)
* DT_DAYOFWEEK - Week day (constants for MONDAY to SUNDAY are available)
* DT_DAYOFMONTH - Day of the current month
* DT_MONTH - Month (constants for JANUARY to DECEMBER are available)
* DT_YEAR - Year
* DT_DAYOFYEAR - Day of the year
* DT_YYYYMMDD - current date in the form YYYYMMDD

It will only return numbers. If another type is supplied -1 will be returned.

```c
if (gettime(DT_DAYOFWEEK) == SATURDAY)
    mes "It's a Saturday. I don't work on Saturdays.";
```
