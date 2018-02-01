# Date and Time Functions<a name="dp-pipeline-reference-functions-datetime"></a>

 The following functions are available for working with DateTime values\. For the examples, the value of `myDateTime` is `May 24, 2011 @ 5:10 pm GMT`\. 

**Note**  
The date/time format for AWS Data Pipeline is Joda Time, which is a replacement for the Java date and time classes\. For more information, see [Joda Time \- Class DateTimeFormat](http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html)\.


****  

| Function | Description | 
| --- | --- | 
|  `int day(DateTime myDateTime)`  |  Gets the day of the DateTime value as an integer\. Example: `#{day(myDateTime)}` Result: `24`  | 
|  `int dayOfYear(DateTime myDateTime)`  |  Gets the day of the year of the DateTime value as an integer\. Example: `#{dayOfYear(myDateTime)}` Result: `144`  | 
|  `DateTime firstOfMonth(DateTime myDateTime)`  |  Creates a DateTime object for the start of the month in the specified DateTime\. Example: `#{firstOfMonth(myDateTime)}` Result: `"2011-05-01T17:10:00z"`  | 
|  `String format(DateTime myDateTime,String format)`  |  Creates a String object that is the result of converting the specified DateTime using the specified format string\. Example: `#{format(myDateTime,'YYYY-MM-dd HH:mm:ss z')}` Result: `"2011-05-24T17:10:00 UTC"`  | 
|  `int hour(DateTime myDateTime)`  |  Gets the hour of the DateTime value as an integer\. Example: `#{hour(myDateTime)}` Result: `17`  | 
|  `DateTime makeDate(int year,int month,int day)`  |  Creates a DateTime object, in UTC, with the specified year, month, and day, at midnight\. Example: `#{makeDate(2011,5,24)}` Result: `"2011-05-24T0:00:00z"`  | 
|  `DateTime makeDateTime(int year,int month,int day,int hour,int minute)`  |  Creates a DateTime object, in UTC, with the specified year, month, day, hour, and minute\. Example: `#{makeDateTime(2011,5,24,14,21)}` Result: `"2011-05-24T14:21:00z"`  | 
|  `DateTime midnight(DateTime myDateTime)`  |  Creates a DateTime object for the current midnight, relative to the specified DateTime\. For example, where `MyDateTime` is `2011-05-25T17:10:00z`, the result is as follows\.  Example: `#{midnight(myDateTime)}` Result: `"2011-05-25T0:00:00z"`  | 
|  `DateTime minusDays(DateTime myDateTime,int daysToSub)`  |  Creates a DateTime object that is the result of subtracting the specified number of days from the specified DateTime\. Example: `#{minusDays(myDateTime,1)}` Result: `"2011-05-23T17:10:00z"`  | 
|  `DateTime minusHours(DateTime myDateTime,int hoursToSub)`  |  Creates a DateTime object that is the result of subtracting the specified number of hours from the specified DateTime\. Example: `#{minusHours(myDateTime,1)}` Result: `"2011-05-24T16:10:00z"`  | 
|  `DateTime minusMinutes(DateTime myDateTime,int minutesToSub)`  |  Creates a DateTime object that is the result of subtracting the specified number of minutes from the specified DateTime\. Example: `#{minusMinutes(myDateTime,1)}` Result: `"2011-05-24T17:09:00z"`  | 
|  `DateTime minusMonths(DateTime myDateTime,int monthsToSub)`  |  Creates a DateTime object that is the result of subtracting the specified number of months from the specified DateTime\. Example: `#{minusMonths(myDateTime,1)}` Result: `"2011-04-24T17:10:00z"`  | 
|  `DateTime minusWeeks(DateTime myDateTime,int weeksToSub)`  |  Creates a DateTime object that is the result of subtracting the specified number of weeks from the specified DateTime\. Example: `#{minusWeeks(myDateTime,1)}` Result: `"2011-05-17T17:10:00z"`  | 
|  `DateTime minusYears(DateTime myDateTime,int yearsToSub)`  |  Creates a DateTime object that is the result of subtracting the specified number of years from the specified DateTime\. Example: `#{minusYears(myDateTime,1)}` Result: `"2010-05-24T17:10:00z"`  | 
|  `int minute(DateTime myDateTime)`  |  Gets the minute of the DateTime value as an integer\. Example: `#{minute(myDateTime)}` Result: `10`  | 
|  `int month(DateTime myDateTime)`  |  Gets the month of the DateTime value as an integer\. Example: `#{month(myDateTime)}` Result: `5`  | 
|  `DateTime plusDays(DateTime myDateTime,int daysToAdd)`  |  Creates a DateTime object that is the result of adding the specified number of days to the specified DateTime\. Example: `#{plusDays(myDateTime,1)}` Result: `"2011-05-25T17:10:00z"`  | 
|  `DateTime plusHours(DateTime myDateTime,int hoursToAdd)`  |  Creates a DateTime object that is the result of adding the specified number of hours to the specified DateTime\. Example: `#{plusHours(myDateTime,1)}` Result: `"2011-05-24T18:10:00z"`  | 
|  `DateTime plusMinutes(DateTime myDateTime,int minutesToAdd)`  |  Creates a DateTime object that is the result of adding the specified number of minutes to the specified DateTime\. Example: `#{plusMinutes(myDateTime,1)}` Result: `"2011-05-24 17:11:00z"`  | 
|  `DateTime plusMonths(DateTime myDateTime,int monthsToAdd)`  |  Creates a DateTime object that is the result of adding the specified number of months to the specified DateTime\. Example: `#{plusMonths(myDateTime,1)}` Result: `"2011-06-24T17:10:00z"`  | 
|  `DateTime plusWeeks(DateTime myDateTime,int weeksToAdd)`  |  Creates a DateTime object that is the result of adding the specified number of weeks to the specified DateTime\. Example: `#{plusWeeks(myDateTime,1)}` Result: `"2011-05-31T17:10:00z"`  | 
|  `DateTime plusYears(DateTime myDateTime,int yearsToAdd)`  |  Creates a DateTime object that is the result of adding the specified number of years to the specified DateTime\. Example: `#{plusYears(myDateTime,1)}` Result: `"2012-05-24T17:10:00z"`  | 
|  `DateTime sunday(DateTime myDateTime)`  |  Creates a DateTime object for the previous Sunday, relative to the specified DateTime\. If the specified DateTime is a Sunday, the result is the specified DateTime\. Example: `#{sunday(myDateTime)}` Result: `"2011-05-22 17:10:00 UTC"`  | 
|  `int year(DateTime myDateTime)`  |  Gets the year of the DateTime value as an integer\. Example: `#{year(myDateTime)}` Result: `2011`  | 
|  `DateTime yesterday(DateTime myDateTime)`  |  Creates a DateTime object for the previous day, relative to the specified DateTime\. The result is the same as minusDays\(1\)\. Example: `#{yesterday(myDateTime)}` Result: `"2011-05-23T17:10:00z"`  | 