# Analyzing_Big_Data_with_SQL_Assignment
This is a peer graded assignment completed as a part of Analyzing Big Data with SQL course on Cloudera. 
This assignment involves working with Impala/Hive to analyze the 'fly' database using SQL.
Peer-Graded Assignment: Analyzing Big Data with SQL
Name: Jaysree Umamaheswaran
Date: 09/03/2019

A company plans to disrupt the airline industry by building an underground high-speed passenger rail tunnel between two airports with large volume of passengers between them.

OBJECTIVE: Analyze and recommend which two airports that should be connected by high-speed underground passenger rail based on range and passenger volume.
Recommendation:
I recommend the following tunnel route:


                                                        First Direction             Second Direction
Three-letter airport code for origin                    SFO                         LAX
Three-letter airport code for destination               LAX                         SFO
Average flight distance in miles                        337                         337
Average number of flights per year                      14712                       14540
Average annual passenger capacity                       19965970                    19810585
Average arrival delay in minutes                        10.32                       13.76

Method:
I identified this route by running the following SELECT statement using IMPALA on the VM:

SELECT origin,
 dest,
 ROUND(COUNT(flight)/10.0) as flights,
 SUM(p.seats) as seats,
 ROUND(AVG(f.arr_delay),2) as avg_delay
FROM flights f
LEFT OUTER JOIN planes p
    ON f.tailnum=p.tailnum
WHERE distance BETWEEN 300
 AND 400
GROUP BY origin, 
       dest 
HAVING COUNT (flight)/10 > 5000 
ORDER BY SUM(p.seats) DESC
LIMIT 100;


(Database used: fly
Tables used: flights, planes)

NOTES:

Conditions that were used to identify the pairs of airport:
 pairs of airports that are between 300 and 400 miles apart
 pairs of airports had at least 5,000 (five thousand) flights per year on average in each   direction between them
 pairs of airports that has the largest total number of seats on the planes that flew  between them
