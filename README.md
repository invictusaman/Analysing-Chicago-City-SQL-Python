# Analysing-Chicago-City-SQL-Python

*This project work shows I can deal with complex real-life datasets and come up with useful insights. These findings can help shape public policy and strategies to improve communities.* 

I used my SQL and Python skills to look into and get insights from three different datasets on the City of Chicago's Data Portal. Here are the datasets I worked with:

**Socioeconomic Indicators in Chicago:** This dataset shows six main socioeconomic indicators that are important to public health, along with a "hardship index" for various Chicago community areas from 2008 to 2012.

[Socioeconomic Indicators in Chicago](https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2/about_data)

**Chicago Public Schools:** This dataset has school-level performance data that was used to create Chicago Public Schools (CPS) Report Cards for the 2011-2012 school year.

[Chicago Public Schools](https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t)

**Chicago Crime Data:** This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days.

[Chicago Crime Data](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2)



### Questions & SQL Queries

#### Problem 1 - Find the total number of crimes recorded in the CRIME table.

```sql
-- Lets find the column names and types from Crime Table
%sql SELECT name, type, length(type) FROM PRAGMA_TABLE_INFO('CHICAGO_CRIME_DATA');
-- Query
%%sql 
SELECT COUNT(*) AS TOTAL_CRIME_COUNT FROM `CHICAGO_CRIME_DATA`;
```

#### Problem 2 - List community area names and numbers with per capita income less than 11000.

```sql
-- Lets find the column names and types from Census Table
%sql SELECT name, type, length(type) FROM PRAGMA_TABLE_INFO('CENSUS_DATA');
-- Query
%%sql
SELECT COMMUNITY_AREA_NAME, COMMUNITY_AREA_NUMBER, PER_CAPITA_INCOME
FROM `CENSUS_DATA`
WHERE PER_CAPITA_INCOME < 11000;
```

#### Problem 3 - List all case numbers for crimes involving minors?(children are not considered minors for the purposes of crime analysis)

```sql
%%sql
SELECT CASE_NUMBER, PRIMARY_TYPE, DESCRIPTION
FROM `CHICAGO_CRIME_DATA`
WHERE LOWER(PRIMARY_TYPE) LIKE '%minor%' OR
LOWER(DESCRIPTION) LIKE '%minor%'
```

#### Problem 4 - List all kidnapping crimes involving a child?

```sql
%%sql
SELECT PRIMARY_TYPE, DESCRIPTION
FROM `CHICAGO_CRIME_DATA`
WHERE LOWER(PRIMARY_TYPE) LIKE '%kidnap%' AND
LOWER(DESCRIPTION) LIKE '%child%'
```

#### Problem 5 - List the kind of crimes that were recorded at schools. (No repetitions)

```sql
%%sql
SELECT DISTINCT PRIMARY_TYPE, LOCATION_DESCRIPTION
FROM `CHICAGO_CRIME_DATA`
WHERE LOWER(LOCATION_DESCRIPTION) LIKE '%school%'
```

#### Problem 6 - List the type of schools along with the average safety score for each type.

```sql
-- Lets find the column names and types from Public School Table
%sql SELECT name, type, length(type) FROM PRAGMA_TABLE_INFO('CHICAGO_PUBLIC_SCHOOLS');
-- Query
%%sql
SELECT `Elementary, Middle, or High School` AS TYPE_OF_SCHOOL, AVG(SAFETY_SCORE) AS AVG_SAFETY_SCORE
FROM `CHICAGO_PUBLIC_SCHOOLS`
GROUP BY `Elementary, Middle, or High School`
```

#### Problem 7 - List 5 community areas with highest % of households below poverty line

```sql
%%sql
SELECT COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY
FROM `CENSUS_DATA`
ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC
LIMIT 5
```

#### Problem 8 - Which community area is most crime prone? Display the coumminty area number only.

```sql
%%sql
SELECT COMMUNITY_AREA_NUMBER
FROM `CHICAGO_CRIME_DATA`
WHERE COMMUNITY_AREA_NUMBER IS NOT NULL 
GROUP BY COMMUNITY_AREA_NUMBER
ORDER BY COUNT(*) DESC
LIMIT 1
```

#### Problem 9 - Use a sub-query to find the name of the community area with highest hardship index

```sql
%%sql
SELECT COMMUNITY_AREA_NAME, HARDSHIP_INDEX
FROM `CENSUS_DATA`
WHERE HARDSHIP_INDEX = (
    SELECT MAX(HARDSHIP_INDEX) FROM `CENSUS_DATA`
    )
```

#### Problem 10 - Use a sub-query to determine the Community Area Name with most number of crimes?

```sql
%%sql 
SELECT COMMUNITY_AREA_NAME
FROM `CENSUS_DATA` As Census
WHERE Census.COMMUNITY_AREA_NUMBER IN 
( SELECT COMMUNITY_AREA_NUMBER 
  FROM `CHICAGO_CRIME_DATA`
  GROUP BY COMMUNITY_AREA_NUMBER
  ORDER BY Count (*) Desc
  LIMIT 1
    )
```

#### Problem 5 - List the kind of crimes that were recorded at schools. (No repetitions)

```sql
%%sql
SELECT DISTINCT PRIMARY_TYPE, LOCATION_DESCRIPTION
FROM `CHICAGO_CRIME_DATA`
WHERE LOWER(LOCATION_DESCRIPTION) LIKE '%school%'
```

Those were 10 problem questions, along with SQL queries. 

This project was conducted as a part of IBM Certification course at Coursera.

---
##### Follow my data-analyst journey: [Portfolio_Link](https://www.amanbhattarai.com)
