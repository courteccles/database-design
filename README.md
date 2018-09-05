# Database Design Mini Lecture
This is a mini lecture designed to help students as they begin personal projects. For most students this is the first time they have actually tried to build thier own schema. The main goals of this mini lecture are as follows:

1. To help students identify and avoid complicating pitfalls. 
2. To help students distinguish between "like" data and related data.
3. To demonstrate best practices.
4. To help students build more robust databases.

## Resources
* [lucidchart.com](https://lucidchart.com/) - You can create a free account that lets you build all kinds of charts including database diagrams.

## Instructions
Below is an example of a poorly designed database. Walk through this poor table design and show students how we would need to query it. Point out the difficulties that come from the poor design.  During each scenario we will also demonstrate some queries so students can really grasp how the design of their databases will affect the dataflow of their applications.


### Bad Example 
  #### students_classes_teachers
  | student_id  | student_first | student_last | student_gender | class1 | class2 | class3 | class4 | class5 | teacher1 | teacher2 | teacher3 | teacher4 | teacher5 |
  | ----------- | ------------- | ------------ | -------------- | ------ | ------ | ------ | ------ | ------ | -------- | -------- | -------- | -------- | -------- |   
  |             |               |              |                |        |        |        |        |        |          |          |          |          |          |
  1. What do you name your table when it holds everything?
  2. Creates duplicate data that is hard to differentiate when querying.
  3. What if you needed to track other faculty?
  4. Other reasons/examples of how this is difficult to maintain?


  Queries

  If we wanted to search for a specific teacher or class the code gets quite wet, and the results have duplicates which are difficult to make sense of. 
  ```
  select * 
  from students_classes_teachers
  where 
    teacher1 = 'Dorey Burn' or 
    teacher2 = 'Dorey Burn' or
    teacher3 = 'Dorey Burn' or
    teacher4 = 'Dorey Burn' or
    teacher5 = 'Dorey Burn' or
    teacher6 = 'Dorey Burn' or
    teacher7 = 'Dorey Burn' or
    teacher8 = 'Dorey Burn';
  ```
  ```
  select * 
  from students_classes_teachers
  where 
    class1 = 'Orchestra' or
    class2 = 'Orchestra' or
    class3 = 'Orchestra' or
    class4 = 'Orchestra' or
    class5 = 'Orchestra' or
    class6 = 'Orchestra' or
    class7 = 'Orchestra' or
    class8 = 'Orchestra';
  ```
  This table does not adequately represent the relationships between classes and teachers, merely that they are related to a particular student. It could be assumed that teacher1 corresoponds with class1, but this is still very difficult to search for. 

### Better Design
> Future employers may use a different naming convention, but for consistency use _snake\_case_.

> *Schema* is the blueprint of our database; it includes not only the data types of our fields/columns, but also the structure of data relationships.

  #### students
  | id  | first_name | last_name | gender | class_id |
  | --- | ---------- | --------- | ------ | -------- |

  #### classes
  | id | grade_level | teacher_id | room |
  | -- | ----------- | ---------- | ---- |
### Strategies
1. "All classes in separate tables" >> this means separating "like" data in their own tables. It's better to have many smaller tables that can be queried through their relationships rather than have all "unlike", but related, data in the same table.
2. Try not to alter tables in production. This may seem relatively harmless, but it is dangerous. You could inadvertantly delete data trying to refactor your database.
3. Don't dynamically create/drop tablesl; for example we shouldn't be creating a new 'cart' table for each user of our app.

### One to Many
  One student enrolled in many classes.
  > Students should have a grasp on one to many relationships. We are starting here to help set up our many to many relation later on. This also helps demonstrate how a database can grow as new features are added.

  1. All classes will be kept in the same table referenced by a foreign key.
  * Foreign Key
    - 1. What is it?
    - 2. Does it need to be constrained? (It is good to know that you can, but for the scope of this mini-lecture as well as the scope of most personal projects this is not necessary.)
  2. When labeling, create table names that are unique and descriptive; create column names that make sense, they don't have to be unique from other tables. You can also use the nouns/adjectives rule; make table names a noun and column names adjectives to the table name.   
  3. I like to label my primary key "table_name_id", and ALWAYS label corresponding foreign keys the same! (The ultimate rule is consistency and to follow the convention set by your company.)
> Always think about how you will access the data!

  #### students
  | student_id  | name | clss_id |
  | ----------- | ---- | ------- |
  |             |      |         |
  #### class
  | class_id  | class | grade_level | teacher_id | room_num |
  | --------- | ----- | ----------- | ---------- | -------- |
  |           |       |             |            |          |
We can search for students by name, or find reviews by title.
```
select books.book_id, books.title, reviews.review, reviews.rating
from reviews
join books on books.book_id = reviews.book_id
where reviews.rating <= 5;
```
### Strategies
> "All classes in separate tables" >> this means separating "like" data in their own tables. It's better to have many smaller tables that can be queried through their relationships rather than have all "unlike", but related, data in the same table.
> junction tables to describe relationships

## Expanding Out Database

## Contributions
  If you see any problems or have great ideas that can be added please let us know!