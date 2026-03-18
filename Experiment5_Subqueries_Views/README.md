# Experiment 5: Subqueries and Views

## AIM
To study and implement subqueries and views.

## THEORY

### Subqueries
A subquery is a query inside another SQL query and is embedded in:
- WHERE clause
- HAVING clause
- FROM clause

**Types:**
- **Single-row subquery**:
  Sub queries can also return more than one value. Such results should be made use along with the operators in and any.
- **Multiple-row subquery**:
  Here more than one subquery is used. These multiple sub queries are combined by means of ‘and’ & ‘or’ keywords.
- **Correlated subquery**:
  A subquery is evaluated once for the entire parent statement whereas a correlated Sub query is evaluated once per row processed by the parent statement.

**Example:**
```sql
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
### Views
A view is a virtual table based on the result of an SQL SELECT query.
**Create View:**
```sql
CREATE VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;
```
**Drop View:**
```sql
DROP VIEW view_name;
```

**Question 1**
--
<img width="826" height="605" alt="image" src="https://github.com/user-attachments/assets/302ce3c2-76d6-41a9-8eb7-b60a461d9355" />


```
SELECT *
FROM ORDERS
WHERE purch_amt>(
    SELECT AVG(purch_amt)
    FROM ORDERS
    WHERE ord_date="2012-10-10")
```

**Output:**

<img width="826" height="500" alt="image" src="https://github.com/user-attachments/assets/6b886be3-a201-42ec-ba69-32c72e4167a9" />

**Question 2**
---
<img width="826" height="786" alt="image" src="https://github.com/user-attachments/assets/d7a088db-1f1f-48f5-a654-d082d4bbab2b" />

```
SELECT *
FROM GRADES g
WHERE grade=
    (SELECT MAX(grade)
    FROM GRADES
    WHERE subject=g.subject)
```

**Output:**

<img width="826" height="470" alt="image" src="https://github.com/user-attachments/assets/b2447352-4c6d-4744-b0a4-feeaa3dc74e6" />

**Question 3**
---
<img width="826" height="778" alt="image" src="https://github.com/user-attachments/assets/da88e840-fd8a-4d8b-91c0-4af8438a4a65" />


```
SELECT *
FROM Employee
WHERE age<(
    SELECT AVG(age)
    FROM Employee
    WHERE income>1000000)
```

**Output:**

<img width="826" height="461" alt="image" src="https://github.com/user-attachments/assets/98aa6fc5-d8cb-4e66-b174-8e750d537bfd" />

**Question 4**
---
<img width="826" height="813" alt="image" src="https://github.com/user-attachments/assets/252e2cae-28ff-4c7f-923d-0f0454a53091" />


```
SELECT * 
FROM CUSTOMERS 
WHERE ID IN(
    SELECT ID
    FROM CUSTOMERS
    WHERE AGE<30 AND ADDRESS="Delhi")
```

**Output:**

<img width="826" height="389" alt="image" src="https://github.com/user-attachments/assets/eb9127d0-2dbe-4a2e-8821-5485d456e451" />

**Question 5**
---
<img width="1577" height="942" alt="image" src="https://github.com/user-attachments/assets/f97c9f9b-71a7-4f99-88b0-ba5977922efa" />


```
SELECT salesman_id,name
FROM salesman
WHERE salesman_id IN(
    SELECT salesman_id
    FROM customer
    GROUP BY salesman_id
    HAVING COUNT(customer_id)>1)
```

**Output:**

<img width="826" height="540" alt="image" src="https://github.com/user-attachments/assets/80e8670a-bfa7-4792-ad65-40c0c9513404" />

**Question 6**
---
<img width="826" height="898" alt="image" src="https://github.com/user-attachments/assets/fda396ff-6e50-46dc-a0db-c3753196ec09" />


```
SELECT commission
FROM salesman
WHERE salesman_id IN(
    SELECT salesman_id
    FROM customer
    WHERE city="Paris")
```

**Output:**

<img width="826" height="350" alt="image" src="https://github.com/user-attachments/assets/2bc4937e-ec1f-4d68-ae60-1226d0a2d9db" />

**Question 7**
---
<img width="1448" height="708" alt="image" src="https://github.com/user-attachments/assets/0a92afdc-7cfe-4d07-94c4-610ae4f3768f" />


```
SELECT name,city
FROM customer
WHERE city in(
    SELECT city
    FROM customer
    WHERE id IN(3,7))
```

**Output:**

<img width="826" height="513" alt="image" src="https://github.com/user-attachments/assets/db26ae70-f5f5-4640-a91f-5b167ee1fee9" />

**Question 8**
---
<img width="826" height="979" alt="image" src="https://github.com/user-attachments/assets/f96cf1ed-83bb-47e1-9fc6-92cdc6635c88" />


```
SELECT * FROM CUSTOMERS WHERE AGE<30
```

**Output:**

<img width="826" height="678" alt="image" src="https://github.com/user-attachments/assets/00ded20d-bf9a-4e5d-8319-5e8958637d12" />

**Question 9**
---
<img width="826" height="971" alt="image" src="https://github.com/user-attachments/assets/47012d85-2752-4dd8-93c6-3aac5eb1e97e" />

```
SELECT * 
FROM customer
WHERE customer_id=(
    SELECT salesman_id-2001
    FROM salesman
    WHERE name="Mc Lyon"
)
```

**Output:**

<img width="826" height="319" alt="image" src="https://github.com/user-attachments/assets/e70af652-bb40-44f9-803d-91363697dd94" />

**Question 10**
---
<img width="826" height="473" alt="image" src="https://github.com/user-attachments/assets/c83376b2-24fe-4284-9d30-5fce5182587b" />


```
SELECT student_name,grade
FROM GRADES g
WHERE grade=(
            SELECT MIN(grade)
            FROM GRADES
            WHERE subject=g.subject)
```

**Output:**

<img width="826" height="489" alt="image" src="https://github.com/user-attachments/assets/3ffc2656-19b3-44bc-aa20-7dee3ccddf74" />


## RESULT
Thus, the SQL queries to implement subqueries and views have been executed successfully.
