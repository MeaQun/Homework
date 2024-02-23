## Homework Week 7

### Section 7.1:
1. Using EVregistry, Write a query to select the ModelYear, Make, and Model off all of the vehicles in the registry.

```SQL
SELECT ModelYear, Make, Model FROM EVRegistry
```

2. Using the EVRegistry table, Write a query that lists all of the unique types of EVs. your reult set should have one column, ElectricVehicleType.

```SQL
SELECT DISTINCT ElectricVehicleType 
FROM EVRegistry
```

3. Using the EVRegistry, Write a query that shows all of the information on Battery Electric Vehicles (BEV) that are in the registry.

```SQL
SELECT * FROM EVRegistry
WHERE ElectricVehicleType = "Battery Electric Vehicle (BEV)"
```


4. Using the EVRegistry, write a query that returns the Make and Model of all of the EVs that have a BaseMSRP between 20000 and 35000?

```SQL
SELECT Make, Model FROM EVRegistry
WHERE BaseMSRP > 20000 and BaseMSRP < 35000
```
OR:

```SQL
SELECT Make, Model FROM EVRegistry
WHERE BaseMSRP BETWEEN 20000 AND 35000
```


---

### Section 7.2:

1. Using EVRegistry, write a query to find a record where the City attribute is NULL. Return all of the available columns.

```SQL
SELECT City FROM EVRegistry
WHERE City is NULL
```
OR:
```SQL
SELECT City FROM EVRegistry
WHERE City = '' or '0' or 'null'
```


2. Write a query to find the make, model, and ElectricVehicleType where the VIN number has that ends in '3E1EA1J'.

```SQL
SELECT Make, Model, ElectricVehicleType FROM EVRegistry
where VIN LIKE '%3E1EA1J'
```

3. Select the ModelYear, make, model, ElectricVehicleType, and range of the Tesla vehicles or cheverolet vehicles in the registry. Order the result set by Make and Model year in from newest to oldest.

```SQL
SELECT ModelYear, Make, Model, ElectricVehicleType, ElectricRange FROM EVRegistry
WHERE Make = 'CHEVROLET' or Make = 'TESLA'
ORDER BY Make, ModelYear DESC 
``` 


4. Using EVCharging, Write a query to find out how many times those stations were used. Order them by the most used to the least used and limit the output to 5 records.

```SQL
SELECT stationId, count(*) as uses
FROM EVCharging
GROUP by stationId
ORDER by uses DESC
LIMIT 5
```


5. Using EVCharging, For the folks who charged longer than 0.5 hours, show the min and max of the charging time for each user. Your output columns should be userid, minTime, and maxTime. Order this result set by the last two columns respectively.

```SQL
SELECT userId, 
	max(chargeTimeMins) as maxTime, 
	min(chargeTimeMins) as minTime 
from EVCharging
WHERE chargeTimeMins > 30
GROUP by userId
order by maxTime, minTime 
```

_I did the following example as practice, it wasn't in the homework, just wanted to save it somewhere I'd find it again_
```SQL
SELECT userId, 
	sum(dollars) as amount, 
	count(*) as frequency,
	round(sum(dollars)/count(*), 2) as amountPerCharge
FROM EVCharging
GROUP by userId
ORDER by 3 DESC
```
---

### Section 7.3:

1. Using EVCharging, which day of the week has the highest average charging time? Round the answer to 2 decimal points.

```SQL
SELECT weekday,
round(sum(chargeTimeHrs), 2) as avgChrgTime
FROM EVCharging
GROUP by weekday
ORDER by avgChrgTime DESC
LIMIT 1
```

2. Using EV charging find the total power consumed from charging EVs by each User. Return the userId and name the calculated column, totalPower. Round the answer to 2 decimal points and list the out put in highest to lowest order. Limit the order to the top 15 users.

```SQL
SELECT userId,
round(sum(kwhTotal), 2) as totalPower
FROM EVCharging
GROUP by userId
ORDER by totalPower DESC
LIMIT 15
```

3. Using dimfacility and factCharge, write a query to find out which type of facility (GROUP BY) has the most amount of charging stations. Return type Facility and name the calculated column numStation. Order the result set from highest to lowest number of charging stations.

```SQL
SELECT typeFacility, 
count(stationId) as numStation
FROM factCharge
INNER JOIN dimFacility
on factCharge.facilityID = dimFacility.FacilityKey
GROUP by typeFacility
ORDER by numStation DESC
```

4. In your own words, Briefly explain Primary Keys and Foreign Keys.

	*A primary key is a unique variable across all DBs. A foreign key is a key from a secondary DB which connects it to the main DB.*


1. Using EV Charging, For the folks who charged longer than one hour, show the min and max of the charging time for each user. Your output columns should be userid, minTime, and maxTime. Order this result set by the last two columns respectively. HINT: USE HAVING

*I don't quite understand the use of "having" here, but also don't understand why these two give me two different results. min(chargeTimeHrs) >1 and chargeTimeHrs > 1 should be the same? I did both below, not sure why they have different outputs.*

```SQL
SELECT userId,
min(chargeTimeHrs) as minTime,
max(chargeTimeHrs) as maxTIme
from EVCharging
GROUP by userId
HAVING minTime > 1
ORDER by 2,3 
```

```SQL
SELECT userId,
min(chargeTimeHrs) as shortest,
max(chargeTimeHrs) as longest
from EVCharging
GROUP by userId
ORDER by 2,3 
```