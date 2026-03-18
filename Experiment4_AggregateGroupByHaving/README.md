# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
How many appointments are scheduled for each doctor?

```sql
SELECT DoctorID, COUNT(*) AS TotalAppointments
FROM Appointments
GROUP BY DoctorID
ORDER BY DoctorID;
```

**Output:**

<img width="715" height="614" alt="image" src="https://github.com/user-attachments/assets/c18d45a4-a3ba-4a18-85e8-ba196d3d1926" />


**Question 2**
---
What is the average dosage prescribed for each medication?

```sql
SELECT Medication,AVG(Dosage) AS AvgDosage
FROM Prescriptions
GROUP BY Medication
ORDER BY Medication;
```

**Output:**

<img width="615" height="726" alt="image" src="https://github.com/user-attachments/assets/6db2c2a3-39da-4079-a08e-21df8c5e6ba1" />


**Question 3**
---
How many patients are there in each city?

```sql
SELECT Address,COUNT(*) AS TotalPatients
FROM Patients
GROUP BY Address
ORDER BY Address;
```

**Output:**

<img width="615" height="391" alt="image" src="https://github.com/user-attachments/assets/48eb105a-0c86-47e0-8375-180f508c91fa" />


**Question 4**
---
Write a SQL query to return the total number of rows in the 'customer' table where the city is not Noida

```sql
SELECT COUNT(*) AS COUNT FROM customer
WHERE city!='Noida';
```

**Output:**

<img width="325" height="271" alt="image" src="https://github.com/user-attachments/assets/a5f4416a-536b-44dc-a4a9-72c96ac76a13" />

**Question 5**
---
Write a SQL query to calculate the average purchase amount of all orders. Return average purchase amount.

Sample table: orders

ord_no purch_amt ord_date customer_id salesman_id

```sql
SELECT AVG(purch_amt) AS AVERAGE
FROM orders;
```

**Output:**

<img width="343" height="298" alt="image" src="https://github.com/user-attachments/assets/8bdb5361-29f5-4438-82df-93e8aff2de7e" />


**Question 6**
---
Write a SQL query to determine the number of customers who received at least one grade for their activity.

Sample table: customer

customer_id | cust_name | city | grade | salesman_id
```
    3002 | Nick Rimando   | New York   |   100 |        5001

    3007 | Brad Davis     | New York   |   200 |        5001

    3005 | Graham Zusi    | California |   200 |        5002
```

```sql
SELECT COUNT(*) AS COUNT FROM customer
WHERE grade IS NOT NULL;
```

**Output:**
<img width="327" height="289" alt="image" src="https://github.com/user-attachments/assets/79131579-19c2-422d-8b9c-605d61c54f66" />


**Question 7**
---
Write a SQL query to find how many employees have an income greater than 50K?

Table: employee

name type

```sql
SELECT COUNT(*) AS employees_count FROM employee
WHERE income>50000;
```

**Output:**
<img width="417" height="299" alt="image" src="https://github.com/user-attachments/assets/35ae2b6f-7757-4b71-a04e-9f21b5f924fd" />


**Question 8**
---
Write the SQL query that achieves the grouping of data by age intervals using the expression (age/5)5, calculates the total salary sum for each group, and excludes groups where the total salary sum is not greater than 5000

```sql
SELECT (age/5)*5 AS age_group,SUM(salary)
FROM customer1
GROUP BY age_group
HAVING SUM(salary)>5000;
```

**Output:**

<img width="584" height="337" alt="image" src="https://github.com/user-attachments/assets/e59c8c44-3b8d-4b61-9fee-1a299b67cc5c" />


**Question 9**
---
Write the SQL query that achieves the grouping of data by city, calculates the average income for each city, and includes only those cities where the average income is greater than 500,000.

```sql
SELECT city,AVG(income)
FROM employee
GROUP BY city
HAVING AVG(income)>500000;
```

**Output:**

<img width="573" height="406" alt="image" src="https://github.com/user-attachments/assets/9f69b52b-bbf5-4937-af5f-e9cae88e2af2" />


**Question 10**
---
Write the SQL query that achieves the grouping of data by occupation, calculates the average work hours for each occupation, and includes only those occupations where the average work hour falls between 10 and 12.

```sql
SELECT occupation,AVG(workhour)
FROM employee1
GROUP BY occupation
HAVING AVG(workhour) BETWEEN 10 AND 12;
```

**Output:**
<img width="621" height="367" alt="image" src="https://github.com/user-attachments/assets/50ba206b-17df-409d-af8c-2cf134f541c7" />





## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
