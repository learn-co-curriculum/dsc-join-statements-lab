# Join Statements - Lab

## Introduction

In this lab, you'll practice your knowledge of `JOIN` statements, using various types of joins and various methods for specifying the links between them.

## Objectives

You will be able to:

* Write SQL queries that make use of various types of joins
* Compare and contrast the various types of joins
* Discuss how primary and foreign keys are used in SQL
* Decide and perform whichever type of join is best for retrieving desired data

## CRM ERD

In this lab, you'll use the same customer relationship management (CRM) database that you saw from the previous lesson.
<img src='images/Database-Schema.png' width="600">

## Connecting to the Database
Import the necessary packages and connect to the database `'data.sqlite'`.


```python
import sqlite3
import pandas as pd
```


```python
conn = sqlite3.connect('data.sqlite')
```

## Select the names of all employees in Boston 

Hint: join the employees and offices tables. Select the first and last name.


```python
q = """
SELECT firstName, lastName
FROM employees
JOIN offices
    USING(officeCode)
WHERE city = 'Boston'
;
"""
pd.read_sql(q, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Julie</td>
      <td>Firrelli</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Steve</td>
      <td>Patterson</td>
    </tr>
  </tbody>
</table>
</div>



## Are there any offices that have zero employees?
Hint: Combine the employees and offices tables and use a group by. Select the office code, city, and number of employees.


```python

# Note that COUNT(*) is not appropriate here because
# we are trying to count the _employees_ in each group.
# So instead we count by some attribute of an employee
# record. The primary key (employeeNumber) is a 
# conventional way to do this
q = """
SELECT
    o.officeCode,
    o.city,
    COUNT(e.employeeNumber) AS n_employees
FROM offices AS o
LEFT JOIN employees AS e
    USING(officeCode)
GROUP BY officeCode
HAVING n_employees = 0
;
"""
pd.read_sql(q, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>officeCode</th>
      <th>city</th>
      <th>n_employees</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>27</td>
      <td>Boston</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Write 3 questions of your own and answer them


```python

# Example question: 
"""
How many customers are there per office?
"""

# Example question answer:
q = """
SELECT
    o.officeCode,
    o.city,
    COUNT(c.customerNumber) AS n_customers
FROM offices AS o
JOIN employees AS e
    USING(officeCode)
JOIN customers AS c
    ON e.employeeNumber = c.salesRepEmployeeNumber
GROUP BY officeCode
;
"""
pd.read_sql(q, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>officeCode</th>
      <th>city</th>
      <th>n_customers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>San Francisco</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Boston</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>NYC</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Paris</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Tokyo</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Sydney</td>
      <td>10</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>London</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



## Level Up 1: Display the names of every individual product that each employee has sold

Hint: You will need to use multiple `JOIN` clauses to connect all the way from employee names to product names.


```python

# We don't need to use aliases for the columns since they
# are conveniently already labeled as different kinds of
# names (firstName, lastName, productName)
q = """
SELECT firstName, lastName, productName
FROM employees AS e
JOIN customers AS c
    ON e.employeeNumber = c.salesRepEmployeeNumber
JOIN orders
    USING(customerNumber)
JOIN orderdetails
    USING(orderNumber)
JOIN products
    USING(productCode)
;
"""
df = pd.read_sql(q, conn)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>productName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1958 Setra Bus</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1940 Ford Pickup Truck</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1939 Cadillac Limousine</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1996 Peterbilt 379 Stake Bed with Outrigger</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1968 Ford Mustang</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2991</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1954 Greyhound Scenicruiser</td>
    </tr>
    <tr>
      <th>2992</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1950's Chicago Surface Lines Streetcar</td>
    </tr>
    <tr>
      <th>2993</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>Diamond T620 Semi-Skirted Tanker</td>
    </tr>
    <tr>
      <th>2994</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1911 Ford Town Car</td>
    </tr>
    <tr>
      <th>2995</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1936 Mercedes Benz 500k Roadster</td>
    </tr>
  </tbody>
</table>
<p>2996 rows × 3 columns</p>
</div>



## Level Up 2: Display the number of products each employee has sold

Alphabetize the results by employee last name.

Hint: Use the `quantityOrdered` column from `orderDetails`. Also, think about how to group the data when some employees might have the same first or last name.


```python
q = """
SELECT firstName, lastName, SUM(quantityOrdered) as total_products_sold
FROM employees AS e
JOIN customers AS c
    ON e.employeeNumber = c.salesRepEmployeeNumber
JOIN orders
    USING(customerNumber)
JOIN orderdetails
    USING(orderNumber)
GROUP BY firstName, lastName
ORDER BY lastName
;
"""
pd.read_sql(q, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>total_products_sold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Loui</td>
      <td>Bondur</td>
      <td>6186</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Larry</td>
      <td>Bott</td>
      <td>8205</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>9290</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>4227</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andy</td>
      <td>Fixter</td>
      <td>6246</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>4180</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>14231</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>11854</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Barry</td>
      <td>Jones</td>
      <td>7486</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Peter</td>
      <td>Marsh</td>
      <td>6632</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Mami</td>
      <td>Nishi</td>
      <td>4923</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Steve</td>
      <td>Patterson</td>
      <td>5561</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>4056</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Foon Yue</td>
      <td>Tseng</td>
      <td>5016</td>
    </tr>
    <tr>
      <th>14</th>
      <td>George</td>
      <td>Vanauf</td>
      <td>7423</td>
    </tr>
  </tbody>
</table>
</div>



## Level Up 3: Display the names employees who have sold more than 200 different products

Hint: this is different from the previous question because the quantity sold doesn't matter, only the number of different products


```python
# Recall that HAVING is used for filtering after an aggregation
q = """
SELECT firstName, lastName, COUNT(productCode) as different_products_sold
FROM employees AS e
JOIN customers AS c
    ON e.employeeNumber = c.salesRepEmployeeNumber
JOIN orders
    USING(customerNumber)
JOIN orderdetails
    USING(orderNumber)
GROUP BY firstName, lastName
HAVING different_products_sold > 200
ORDER BY lastName
;
"""
pd.read_sql(q, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>different_products_sold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Larry</td>
      <td>Bott</td>
      <td>236</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>272</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>396</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>331</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Barry</td>
      <td>Jones</td>
      <td>220</td>
    </tr>
    <tr>
      <th>5</th>
      <td>George</td>
      <td>Vanauf</td>
      <td>211</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congrats! You practiced using join statements and leveraged your foreign keys knowledge!
