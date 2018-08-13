# Database Design Mini Lecture
This is a mini lecture designed to help students as they begin personal projects. For most students this is the first time they have actually thought about how they would architect a database. The goal is to help them avoid complicating pitfalls and incorporate some best practices.

We are going to start by building a database with many common mistakes, and then redesign it demonstrating the benefits of better practices. 

## Schema
Schema is the blueprint of our database; yes it does include the data types of our fields / columns, but that is only one piece. It also includes how we are going to set up our tables and how we will keep track of our data relationships.
1. Structure
  * One to one
  * One to many
  * Many to many
2. Datatypes
  * It is important to make sure data is the right type when coming from the client
3. M-M, 1-1, 1-M

## Naming Convention
1. Naming convention
    * Lower Case Snake Case ex: _snake\_case_ or _my\_awesome\_table_

## Strategies
1. "all classes in separate tables"
2. junction tables to describe relationships

## Discuss relationship
1. 1-1
2. 1-M 
3. M-M

## Progression of nastiness
1. one row with tons of columns
2. many rows with repeat data
3. Properly formatted db with multiple rows

class   classid student teacher
g	    1	    1	    4
g	    1	    2	    4
g	    3	    3	    3
a	    2	    3	    2

1. Student/Class/Teachers - 
    - Progressing from grade to high school
    - What about lunch lady?
1. 1-M: k-6 class/students,
1. 1-1: Teachers to classes/ could include in class table but what about lunch lady?
1. Scale it up bad examples - Classes and students in high school
1. Scale it up again: M-M: Teacher_classes_role - college example
1. students-grades-/-assignments-classes

# One to Many
  One student enrolled in many classes.
  * Students should have a grasp on one to many relationships. We are starting here to help set up our many to many relation later on. This also helps demonstrate how a database can grow as new features are added.

## Bad Example
  1. What do you name your table when it holds everything?
  #### info
  | student_id  | student_first | student_gender | class1 | class2 | class3 | class4 | class5 | teacher1 | teacher2 | teacher3 | teacher1 | teacher1 |
  | ----------- | ------------- | -------------- | ------ | ------ | ------ | ------ | ------ | -------- | -------- | -------- | -------- | -------- |   
  |             |               |                |        |        |        |        |        |          |          |          |          |          |

## Better Design
  1. All classes will be kept in the same table referenced by a foreign key.
  * Foreign Key
    - 1. What is it?
    - 2. Does it need to be constrained? (It is good to know that you can, but for the scope of this mini-lecture as well as the scope of most personal projects this is not necessary.)
  2. When labeling, create table names that are unique and descriptive; create column names that make sense, they don't have to be unique from other tables. You can also use the nouns/adjectives rule; make table names a noun and column names adjectives to the table name.   
  3. I like to label my primary key "table_name_id", and ALWAYS label corresponding foreign keys the same! (The ultimate rule is consistency and to follow the convention set by your company.)
> Always think about how you will access the data!

  #### students
  | student_id  | name |
  | ----------- | ---- |
  |             |      |
  #### class
  | class_id  | class | student_id |
  | --------- | ----- | ---------- |
  |           |       |            |
We can search for students by name, or find reviews by title.
```
select books.book_id, books.title, reviews.review, reviews.rating
from reviews
join books on books.book_id = reviews.book_id
where reviews.rating <= 5;

```