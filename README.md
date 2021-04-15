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
# Your code here
```

## Select the names of all employees in Boston 

Hint: join the employees and offices tables. Select the first and last name.


```python
# Your code here
```

## Are there any offices that have zero employees?
Hint: Combine the employees and offices tables and use a group by. Select the office code, city, and number of employees.


```python
# Your code here
```

## Write 3 questions of your own and answer them


```python
# Answers will vary

# Example question: 
"""
How many customers are there per office?
"""
```


```python
"""
Question 1
"""

# Your code here
```


```python
"""
Question 2
"""

# Your code here
```


```python
"""
Question 3
"""

# Your code here
```

## Level Up 1: Display the names of every individual product that each employee has sold

Hint: You will need to use multiple `JOIN` clauses to connect all the way from employee names to product names.


```python
# Your code here
```

## Level Up 2: Display the number of products each employee has sold

Alphabetize the results by employee last name.

Hint: Use the `quantityOrdered` column from `orderDetails`. Also, think about how to group the data when some employees might have the same first or last name.


```python
# Your code here
```

## Level Up 3: Display the names employees who have sold more than 200 different products

Hint: this is different from the previous question because the quantity sold doesn't matter, only the number of different products


```python
# Your code here
```

## Summary

Congrats! You practiced using join statements and leveraged your foreign keys knowledge!
