
# Join Statements - Lab

## Introduction

In this lab, you'll practice your knowledge of join statements, using various types of joins and various methods for specifying the links between them.

## Objectives

You will be able to:
- Write queries that make use of various types of Joins
- Join tables using foreign keys

# CRM Schema

In almost all cases, rather then just working with a single table we will typically need data from multiple tables. Doing this requires the use of **joins ** using shared columns from the two tables. For example, here's a diagram of a mock customer relation management database.
<img src='Database-Schema.png' width=550>

# Connecting to the Database
Import the necessary packages and connect to the database **data.sqlite**.


```python
import sqlite3
import pandas as pd
```


```python
conn = sqlite3.connect('data.sqlite', detect_types=sqlite3.PARSE_COLNAMES)
cur = conn.cursor()
```

# Display the names of all the employees in Boston.

Hint: join the employees and customers tables.


```python
#Your code here
```


```python
cur.execute("""select firstName, lastName from employees join offices using(officeCode) where city = 'Boston';""")
cur.fetchall()
```




    [('Julie', 'Firrelli'), ('Steve', 'Patterson')]



# Do any offices have no employees?
Hint: Combine the employees and offices tables and use a group by.


```python
cur.execute("""select city,
                    count(*)
                    from offices
                    left join employees
                    using(officeCode)
                    group by 1;""")
df = pd.DataFrame(cur.fetchall())
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Boston</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>London</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NYC</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Paris</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>San Francisco</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
# No, but be sure to use a left join!!

# Note: this question could also be answered using a having clause which you'll learn about later 
#(HAVING clauses are filters similar to the WHERE clause but are conditions applied after a group by.)
```

# Write 3 Questions of your own and answer them


```python
# Answers will vary
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

# Level Up: Display the names of each product each employee has sold.


```python
cur.execute("""select firstName, lastName,
                      productName
                      from employees e
                      join
                      customers c
                      on e.employeeNumber = c.salesRepEmployeeNumber
                      join orders o
                      using(customerNumber)
                      join orderdetails od
                      using(orderNumber)
                      join products p
                      using(productCode)""")
df = pd.DataFrame(cur.fetchall())
print(len(df))
df.head()
```

    2996





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
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



# Level Up: Display the Number of Products each Employee Has sold


```python
df.groupby([0,1]).count()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>2</th>
    </tr>
    <tr>
      <th>0</th>
      <th>1</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Andy</th>
      <th>Fixter</th>
      <td>185</td>
    </tr>
    <tr>
      <th>Barry</th>
      <th>Jones</th>
      <td>220</td>
    </tr>
    <tr>
      <th>Foon Yue</th>
      <th>Tseng</th>
      <td>142</td>
    </tr>
    <tr>
      <th>George</th>
      <th>Vanauf</th>
      <td>211</td>
    </tr>
    <tr>
      <th>Gerard</th>
      <th>Hernandez</th>
      <td>396</td>
    </tr>
    <tr>
      <th>Julie</th>
      <th>Firrelli</th>
      <td>124</td>
    </tr>
    <tr>
      <th>Larry</th>
      <th>Bott</th>
      <td>236</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Leslie</th>
      <th>Jennings</th>
      <td>331</td>
    </tr>
    <tr>
      <th>Thompson</th>
      <td>114</td>
    </tr>
    <tr>
      <th>Loui</th>
      <th>Bondur</th>
      <td>177</td>
    </tr>
    <tr>
      <th>Mami</th>
      <th>Nishi</th>
      <td>137</td>
    </tr>
    <tr>
      <th>Martin</th>
      <th>Gerard</th>
      <td>114</td>
    </tr>
    <tr>
      <th>Pamela</th>
      <th>Castillo</th>
      <td>272</td>
    </tr>
    <tr>
      <th>Peter</th>
      <th>Marsh</th>
      <td>185</td>
    </tr>
    <tr>
      <th>Steve</th>
      <th>Patterson</th>
      <td>152</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congrats! You now know how to use join statements, along with leveraging your foreign keys knowledge!
