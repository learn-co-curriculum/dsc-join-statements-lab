
# Join Statements - Lab

## Introduction

In this lab, you'll practice your knowledge of `JOIN` statements, using various types of joins and various methods for specifying the links between them.

## Objectives

You will be able to:
* Write SQL queries that make use of various types of joins
* Compare and contrast the various types of joins
* Discuss how primary and foreign keys are used in SQL
* Decide and perform whichever type of join is best for retrieving desired data

## CRM Schema

In almost all cases, rather than just working with a single table you will typically need data from multiple tables. 
Doing this requires the use of **joins** using shared columns from the two tables. 

In this lab, you'll use the same customer relationship management (CRM) database that you saw from the previous lesson.
<img src='images/Database-Schema.png' width="600">

## Connecting to the Database
Import the necessary packages and connect to the database `'data.sqlite'`.


```python
# Your code here
```


```python
# __SOLUTION__ 
import sqlite3
import pandas as pd
```


```python
# __SOLUTION__ 
conn = sqlite3.connect('data.sqlite')
cur = conn.cursor()
```

## Display the names of all the employees in Boston 

Hint: join the employees and offices tables.


```python
# Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT firstName, lastName 
               FROM employees 
               JOIN offices 
               USING(officeCode) 
               WHERE city = 'Boston';""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df.head()
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
Hint: Combine the employees and offices tables and use a group by.


```python
# Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT o.officeCode, o.city, COUNT(e.employeeNumber) AS n_employees
               FROM offices AS o 
               LEFT JOIN employees AS e
               USING(officeCode)
               GROUP BY officeCode
               HAVING n_employees = 0;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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



## Write 3 Questions of your own and answer them


```python
# Answers will vary
# Example: Display the htmlDescription and employee's first and last name for each product that each employee has sold
```


```python
# Your code here
```


```python
# Your code here
```


```python
# Your code here
```


```python
# __SOLUTION__ 
# Answers will vary
```

## Level Up: Display the names of every individual product that each employee has sold


```python
# Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT firstName, lastName, productName
               FROM employees e
               JOIN customers c
               ON e.employeeNumber = c.salesRepEmployeeNumber
               JOIN orders o
               USING(customerNumber)
               JOIN orderdetails od
               USING(orderNumber)
               JOIN products p
               USING(productCode)""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(len(df))
df.head()

## NOTE: This question could also be answered using a HAVING clause which you learned about previously 
## Remember, HAVING clauses are filters similar to the WHERE clause but are conditions applied after a GROUP BY.
```

    2996





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
  </tbody>
</table>
</div>



## Level Up: Display the Number of Products each employee has sold


```python
# Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT firstName, lastName, COUNT(productName)
               FROM employees e
               JOIN customers c                   
               ON e.employeeNumber = c.salesRepEmployeeNumber
               JOIN orders o 
               USING(customerNumber)
               JOIN orderdetails od 
               USING(orderNumber)                    
               JOIN products p 
               USING(productCode)
               GROUP BY lastName
               ORDER BY firstName""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(len(df))
df

## NOTE: Another way to access this information would be to use the dataframe from 
## the previous "level up" question and call a pandas .groupby method like so:
# df.groupby(['firstName','lastName']).count()
```

    15





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
      <th>COUNT(productName)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Andy</td>
      <td>Fixter</td>
      <td>185</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barry</td>
      <td>Jones</td>
      <td>220</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Foon Yue</td>
      <td>Tseng</td>
      <td>142</td>
    </tr>
    <tr>
      <th>3</th>
      <td>George</td>
      <td>Vanauf</td>
      <td>211</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>396</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>124</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Larry</td>
      <td>Bott</td>
      <td>236</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>331</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>114</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Loui</td>
      <td>Bondur</td>
      <td>177</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Mami</td>
      <td>Nishi</td>
      <td>137</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>114</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>272</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Peter</td>
      <td>Marsh</td>
      <td>185</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Steve</td>
      <td>Patterson</td>
      <td>152</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congrats! You practiced using join statements and leveraged your foreign keys knowledge!
