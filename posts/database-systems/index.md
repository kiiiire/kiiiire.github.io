# Database Systems


# What is an I/O and why should I care?

&lt;!--more--&gt;If you’re taking 186, you’ve probably heard or experienced some variation of the following:

- This class has a lot of I/O counting on exams
- I/Os are weird, sometimes you can read like 5 things at the same time but it’s still one I/O for some reason
- I/O counting is tedious and boring and I will never do it after this class, so why do I need to do it??????

While I can’t guarantee that you’ll ever count I/Os after this class, my hope for this article is to justify why it’s necessary for understanding key database design concepts, and how the principles can be applied to real-world issues in query optimization. It’s like learning how to multiply numbers by hand when we have calculators: although functionally obsolete, we still need to understand the mechanics before we can start taking the shortcut.

## The Problem [#](#the-problem)

The biggest issue that most database solutions solve is that **disks are slow and memory is fast, but we don’t have enough memory to store everything we need.**

Think about trying to process hundreds of gigabytes of data on your computer (very common for applications like machine learning), even though you only have something like 16GB of RAM.

If we want our operations to complete in a reasonable amount of time, we’ll want to do as much as possible within RAM. Most of the algorithms you’ve likely encountered in 61B assume that we have an *infinite* amount of fast memory, and that all operations took the same amount of time, since we only cared about asymptotic runtimes.

However, in the real world, reading something from memory could be hundreds of thousands of times faster than reading something from disk. As such, when evaluating the runtime of an algorithm we only care about how long it takes to transfer something from disk into memory so that we can access it. This basic unit of time is what we call the Input-Output cost, or “an I/O” for short.

## Definition of an I/O [#](#definition-of-an-io)

&gt; Summary
&gt;
&gt; **An I/O is a single read or write event where one page of data is transferred between memory and disk.**

A **page** is the basic (“atomic”) unit of information on disk: since it’s more efficient than reading one byte at a time, nearly all modern hard drives and SSDs have some hard-coded block size in their firmware, such that any data accessed from them will always be delivered one block at a time. These blocks are then converted into pages, which also have a fixed size, in the operating system.

For the purposes of this class, **“block” and “page” can be used interchangably.** In most contexts we will refer to it as a page. However, in general, [there is a difference.](https://stackoverflow.com/questions/22137555/whats-the-difference-between-page-and-block-in-operating-system)

A very important thing to note down is that **there is no such thing as a fractional I/O.** If we need to read 4.1 pages’ worth of data, it will really take 5 I/Os since the last page needs to be read in its entirety.

## Applications [#](#applications)

The primary application for evaluating I/O cost is for [query optimization]()

### Query Optimization

**Introduction** Query optimization is the bridge between a declarative language (like SQL, where you describe “what” you want) and an imperative...

2023/11/20

, where we need to decide which operation is the most efficient out of several possibilities.



**Why can’t we just use asymptotic runtime?**

- We’re dealing with known, finite input sizes: We may decide to use a different algorithm for a 1000-row table versus a 10000000-row table, even if the algorithm for the former has poorer asymptotic properties.
- The difference between a O(n)O(n)O(n) algorithm and an O(2n)O(2n)O(2n) algorithm can get extremely noticable when we have millions (or billions) of rows in a table.
- The runtime depends on the data that we put in. For instance, certain types of joins (like Sort-Merge Join) perform poorly when we have large amounts of duplicate data. Since we already know (or can approximate) what data we have, we can use this knowledge to estimate how much of the data will need to be accessed to complete the operation, which may be dramatically better or worse than its average runtime.

# SQL Part 1 - Basic Queries

## Database Lingo

Relational databases are made up of **tables** (aka relations). A table has a name (we’ll call this one `Person`) and looks like this:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Ben  | 7    | 2        |
| Cho  | 27   | 3        |

Tables have rows (aka tuples) and columns (aka attributes). In this example, the columns are `name`, `age`, `num_dogs`.

## Querying a Table

The most fundamental SQL query looks like this:

```
SELECT &lt;columns&gt;
FROM &lt;tb1&gt;;
```



The `FROM` clause tells SQL which table you’re interested in, and the `SELECT` clause tells SQL which columns of that table you want to see. For example, consider a table Person(`name`, `age`, `num_dogs`) containing the data below:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Ben  | 7    | 2        |
| Cho  | 27   | 3        |

If we executed this SQL query:

```
SELECT name , num_dogs
FROM Person ;
```



then we could get the following output.

| name | num_dogs |
| :--- | :------- |
| Ace  | 4        |
| Ada  | 3        |
| Ben  | 2        |
| Cho  | 3        |

Optionally, we can also add a `DISTINCT` keyword to the `SELECT` statement which removes duplicate rows before output. If we execute the following query to the previous example, the output does not change because all rows are already unique:

```
SELECT DISTINCT name , num_dogs
FROM Person ;
```



| name | num_dogs |
| :--- | :------- |
| Ace  | 4        |
| Ada  | 3        |
| Ben  | 2        |
| Cho  | 3        |

In SQL, however, the order of the rows is nondeterministic unless the query contains an `ORDER BY` (we’ll get to this later). So the following output is equally valid:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ben  | 7    | 2        |
| Cho  | 27   | 3        |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |

In fact, any ordering of those 4 rows is correct – so unless your query contains an `ORDER BY` clause, don’t make any assumptions about the order of your results.

## Filtering out uninteresting rows

Frequently we are interested in only a subset of the data available to us. That is, even though we might have data about many people or things, we often only want to see the data that we have about very specific people or things. This is where the `WHERE` clause comes in handy; it lets us specify which specific rows of our table we’re interested in. Here’s the syntax:

```
SELECT &lt;columns&gt;
FROM &lt;tbl&gt;
WHERE &lt;predicate&gt; ;
```



Once again, let’s consider our table Person(`name`, `age`, `num_dogs`). Suppose we want to see how many dogs each person owns — same as before — but this time we only care about the dog-owners who are adults. Let’s walk through this SQL query:

```
SELECT name, num_dogs
FROM Person
WHERE age &gt;= 18 ;
```



When reasoning about query execution you can use the following rule: each clause in a SQL query happens in the order it’s written, except for `SELECT` which happens last. This is not necessarily the order the database actually does these operations (more on this later in the semester), but it is easy to think about and will always give us the correct answer. The `FROM` clause tells us we’re interested in the Person table, so this is the table we start with:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Ben  | 7    | 2        |
| Cho  | 27   | 3        |

Next we move on to the `WHERE` clause. It tells us that we only want to keep the rows satisfying the predicate age &gt;= 18, so we remove the row with Ben, and are left with the following table:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Cho  | 27   | 3        |

And finally, we `SELECT` the columns `name` and `num_dogs` to obtain our final result. (Again, any permutation of this result is equally valid so you shouldn’t make any assumptions about the order of the rows.) This gives us our final table:

| name | num_dogs |
| :--- | :------- |
| Ace  | 4        |
| Ada  | 3        |
| Cho  | 3        |

## Boolean operators

If you want to filter on more complicated predicates, you can use the boolean operators `NOT`, `AND`, and `OR`. For instance, if we only cared about dog-owners who are not only adults, but also own more than 3 dogs, then we would write the following query:

```
SELECT name, num_dogs
FROM Person
WHERE age &gt;= 18
AND num_dogs &gt; 3;
```



As in Python, this is the order of evaluation for boolean operators:

1. `NOT`
2. `AND`
3. `OR`

That said, it is good practice to avoid ambiguity by adding parentheses even when they are not strictly necessary.

## Filtering Null Values

In SQL, there is a special value called NULL, which can be used as a value for any data type, and represents an “unknown” or “missing” value.

Bear in mind that some values in your database may be `NULL` whether you like it or not, so it’s good to know how SQL handles them. It pretty much boils down to the following rules:

- If you do anything with `NULL`, you’ll just get `NULL`. For instance if x is `NULL`, then x &gt; 3, 1 = x, and x &#43; 4 all evaluate to `NULL`. Even x = `NULL` would evaluate to `NULL`; if you want to check whether x is `NULL`, then write x IS `NULL` or x IS NOT `NULL` instead.

- `NULL` is falsey, meaning that `WHERE NULL` is just like `WHERE FALSE`. The row in question does not get included.

- ```plaintext
  NULL
  ```

   

  short-circuits with boolean operators. That means a boolean expression involving

   

  ```plaintext
  NULL
  ```

   

  will evaluate to:

  - `TRUE`, if it’d evaluate to `TRUE` regardless of whether the NULL value is really `TRUE` or `FALSE`.
  - `FALSE`, if it’d evaluate to `FALSE` regardless of whether the `NULL` value is really `TRUE` or `FALSE`.
  - Or `NULL`, if it depends on the `NULL` value.

Let’s walk through this query as an example:

```
SELECT name, num_dogs
FROM Person
WHERE age &lt;= 20
OR num_dogs = 3;
```



Let’s assume we change some values to NULL, so after evaluating the FROM clause we are left with:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | NULL | 3        |
| Ben  | NULL | NULL     |
| Cho  | 27   | NULL     |

Next we move on to the `WHERE` clause. It tells us that we only want to keep the rows satisfying the predicate `age` &lt;= 20 OR `num_dogs` = 3. Let’s consider each row one at a time:

- For Ace, `age` &lt;= 20 evaluates to `TRUE` so the claim is satisfied.
- For Ada, `age` &lt;= 20 evaluates to `NULL` but `num_dogs` = 3 evaluates to `TRUE` so the claim is satisfied.
- For Ben, `age` &lt;= 20 evaluates to `NULL` and `num_dogs` = 3 evaluates to `NULL` so the overall expression is `NULL` which has a falsey value.
- For Cho, `age` &lt;= 20 evaluates to `FALSE` and `num_dogs` = 3 evaluates to `NULL` so the overall expression evaluates to `NULL` (because it depends on the value of the `NULL`). Because `NULL` is falsey, this row will be excluded.

Thus we keep only Ace and Ada.

## Grouping and Aggregation

When you’re working with a very large database, it is useful to be able to summarize your data so that you can better understand the general trends at play. Let’s see how.

### Summarizing columns of data

With SQL you are able to summarize entire columns of data using built-in aggregate functions. The most common ones are `SUM`, `AVG`, `MAX`, `MIN`, and `COUNT`. Here are some important characteristics of aggregate functions:

- The input to an aggregate function is the name of a column, and the output is a single value that summarizes all the data within that column.
- Every aggregate ignores NULL values except for `COUNT(*)`. (So `COUNT(&lt;column&gt;)` returns the number of non-NULL values in the specified column, whereas `COUNT(*)` returns the number of rows in the table overall.)

For example, consider this variant of our table `People(name, age, num_dogs)` from earlier, where we are now unsure how many dogs Ben owns:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Ben  | 7    | NULL     |
| Cho  | 27   | 3        |

With this table in mind …

- `SUM(age)` is 72.0 and `SUM(num_dogs)` is 10.0.
- `AVG(age)` is 18.0 and `AVG(num_dogs)` is 3.3333333333333333.
- `MAX(age)` is 27, and `MAX(num_dogs)` is 4.
- `MIN(age)` is 7, and `MIN(num_dogs)` is 3.
- `COUNT(age)` is 4, `COUNT(num_dogs)` is 3, and `COUNT(*)` is 4.

So, if we desired the range of ages represented in our database, then we could use the query below and it would produce the result 20. (Technically it would produce a one-by-one table containing the number 20, but SQL treats it the same as the number 20 itself.)

```
SELECT MAX(age) - MIN(age)
FROM Person;
```



Or, if we desired the average number of dogs owned by adults, then we could write this:

```
SELECT AVG(num_dogs)
FROM Person
WHERE age &gt;= 18;
```



### Summarizing Groups of Data

Now you know how to summarize an entire column of your database into a single number. More often than not, though, we want a little finer granularity than that. This is possible with the `GROUP BY` clause, which allows us to split our data into groups and then summarize each group separately. Here’s the syntax:

```
SELECT &lt;columns&gt;
FROM &lt;tbl&gt;
WHERE &lt;predicate&gt; -- Filter out rows (before grouping).
GROUP BY &lt;columns&gt;
HAVING &lt;predicate&gt;; -- Filter out groups (after grouping).
```



Notice we also have a brand new `HAVING` clause, which is actually very similar to `WHERE`. The difference?

- `WHERE` occurs *before grouping*. It filters out uninteresting *rows*.
- `HAVING` occurs *after grouping*. It filters out uninteresting *groups*.

To explore all these new mechanics let’s see another step-by-step example. This time our query will find the average number of dogs owned, for each adult age represented in our database. We will exclude any age for which we only have one datum.

```
SELECT age, AVG(num_dogs)
FROM Person
WHERE age &gt;= 18
GROUP BY age
HAVING COUNT(*) &gt; 1;
```



Let us assume the `Person` table is now:

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Ben  | 7    | 2        |
| Cho  | 27   | 3        |
| Ema  | 20   | 2        |
| Ian  | 20   | 3        |
| Jay  | 18   | 5        |
| Mae  | 33   | 8        |
| Rex  | 27   | 1        |

Next we move on to the `WHERE` clause. It tells us that we only want to keep the rows satisfying the predicate `age &gt;= 18`, so we remove the row with Ben.

| name | age  | num_dogs |
| :--- | :--- | :------- |
| Ace  | 20   | 4        |
| Ada  | 18   | 3        |
| Cho  | 27   | 3        |
| Ema  | 20   | 2        |
| Ian  | 20   | 3        |
| Jay  | 18   | 5        |
| Mae  | 33   | 8        |
| Rex  | 27   | 1        |

Now for the interesting part. We arrive at the `GROUP BY` clause, which tells us to categorize the data by age. We end up with a group of all the adults 20 years old, a group of all the adults 18 years old, a group of all the adults 27 years old, and a group of all the adults 33 years old. You can now think of the table like this:

| name        | age      | num_dogs |
| :---------- | :------- | :------- |
| Ace Ema Ian | 20 20 20 | 4 2 3    |
| Ada Jay     | 18 18    | 3 5      |
| Cho Rex     | 27 27    | 3 1      |
| Mae         | 33       | 8        |

The `HAVING` clause tells us we only want to keep the groups satisfying the predicate `COUNT(*) &gt; 1` — that is, the groups that contain more than one row. We discard the group that contains only Mae.

| name        | age      | num_dogs |
| :---------- | :------- | :------- |
| Ace Ema Ian | 20 20 20 | 4 2 3    |
| Ada Jay     | 18 18    | 3 5      |
| Cho Rex     | 27 27    | 3 1      |

Last but not least, every group gets collapsed into a single row. According to our `SELECT` clause, each such row must contain two things:

- The `age` corresponding to the group.
- The `AVG(num_dogs)` for the group.

Our final result looks like this:

| age  | AVG(num_dogs) |
| :--- | :------------ |
| 20   | 3.0           |
| 18   | 4.0           |
| 27   | 2.0           |

So, to recap, here’s how you should go about a query that follows the template above:

- Start with the table specified in the `FROM` clause.
- Filter out uninteresting rows, keeping only the ones that satisfy the `WHERE` clause.
- Put data into groups, according to the `GROUP BY` clause.
- Filter out uninteresting groups, keeping only the ones that satisfy the `HAVING` clause.
- Collapse each group into a single row, containing the fields specified in the `SELECT` clause.

## A Word of Caution

So that’s how grouping and aggregation work, but before we move on we must emphasize one last thing regarding illegal queries. We’ll start by considering these two examples:

1. Though it’s not immediately obvious, this query actually produces an error:

```
SELECT age, AVG(num_dogs)
FROM Person;
```



What’s the issue? `age` is an entire column of numbers, whereas `AVG(num_dogs)` is just a single number. This is problematic because a properly formed table must have the same amount of rows in each column.

1. This query does not work either, for a very similar reason:

```
SELECT age, num_dogs
FROM Person
GROUP BY age;
```



After grouping by age we obtain a table like this:

| name        | age      | num_dogs |
| :---------- | :------- | :------- |
| Ace Ema Ian | 20 20 20 | 4 2 3    |
| Ada Jay     | 18 18    | 3 5      |
| Cho Rex     | 27 27    | 3 1      |
| Mae         | 33       | 8        |

Then the `SELECT` clause’s job is to collapse each group into a single row. Each such row must contain two things:

- The `age` corresponding to the group, which is a single number.
- The `num_dogs` for the group, which is an entire column of numbers.

The takeaway from all this? If you’re going to do any grouping / aggregation at all, then you must only `SELECT` grouped / aggregated columns.

## Order By

Before, we mentioned that the ordering of the output rows was usually nondeterministic in SQL. If you want the rows of your table to appear in a certain order you must use an `ORDER BY` clause.

Here is an example query using an `ORDER BY` clause:

```
SELECT name, num_dogs
FROM Person
ORDER BY num_dogs, name;
```



You can include as many columns as you want in the ORDER BY clause. We first sort on the first column listed, and then break any ties with the second column listed, and then break any remaining ties with the third column listed, and so on. By default, the sort order is ascending, but if we want the order in descending order we add the DESC keyword after the column name. If we wanted to sort by the num_dogs ascending and break ties by name descending, we would use the following query:

```
SELECT name, num_dogs
FROM Person
ORDER BY num_dogs, name DESC;
```



## Limit

Sometimes we only want to see a few rows in our table, even if more rows match all of our other conditions. To do this we can add a `LIMIT` clause to the end of our query to cap the number of rows that will be returned. Note: the same rows may not always be returned by queries using `LIMIT` if an `ORDER BY` is not used or if there are ties in the ordering. Here is a query that only returns one row:

```
SELECT name, num_dogs
FROM Person
LIMIT 1;
```



## Conclusion

Congratulations - we have covered a lot of SQL! To help remember the ordering of the SQL stages, here is the syntax of a query involving the expressions we have learned so far:

```
SELECT &lt;columns&gt;
FROM &lt;tbl&gt;
WHERE &lt;predicate&gt;
GROUP BY &lt;columns&gt;
HAVING &lt;predicate&gt;
ORDER BY &lt;columns&gt;
LIMIT &lt;num&gt;;
```

# SQL Part 2 - Joins &amp; Subqueries

## Cross Join

The simplest join is called **cross join**, which is also known as a cross product or a cartesian product. A cross join is the result of combining every row from the left table with every row from the right table. To do a cross join, simply comma separate the tables you would like to join. Here is an example:

```
SELECT *
FROM courses, enrollment;
```



If the courses table looked like this:

| num   | name |
| :---- | :--- |
| CS186 | DB   |
| CS188 | AI   |
| CS189 | ML   |

And the enrollment table looked like this:

| c_num | students |
| :---- | :------- |
| CS186 | 700      |
| CS188 | 800      |

The result of the query would be:

| num   | name | c_num | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS186 | DB   | CS188 | 800      |
| CS188 | AI   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |
| CS189 | ML   | CS186 | 700      |
| CS189 | ML   | CS188 | 800      |

The cartesian product often contains much more information than we are actually interested in. Let’s say we wanted all of the information about a course (num, name, and num of students enrolled in it). We cannot just blindly join every row from the left table with every row from the right table. There are rows that have two different courses in them! To account for this we will add a **join condition** in the `WHERE` clause to ensure that each row is only about one class.

To get the enrollment information for a course properly we want to make sure that `num` in the courses table is equal to `c_num` in the enrollment table because they are the same thing. The correct query is:

```
SELECT *
FROM courses, enrollment;
WHERE num = c_num
```



which produces:

| num   | name | c_num | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |

Notice that CS189, which was present in the `courses` table but not in the `enrollment` table, is not included. Since it does not appear as a `c_num` value in enrollment, it cannot fulfill the join condition `num = c_num`.

If we really want CS189 to appear anyway, there are ways to do this that we will discuss later.

## Inner Join

The cross join works great, but it seems a little sloppy. We are including join logic in the `WHERE` clause. It can be difficult to find what the join condition is. In contrast, the **inner join** allows you to specify the condition in the `ON` clause. Here is the syntax:

```
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1_column_name = table2_column_name;
```



The `table1_column_name = table2_column_name` is the join condition. Let’s write the query that gets us all of the course information as an inner join:

```
SELECT *
FROM courses INNER JOIN enrollment
ON num = c_num ;
```



This query is logically the exact same query as the query we ran in the previous section. The inner join is essentially syntatic sugar for a cross join with a join condition in the `WHERE` clause like we demonstrated before.

## Outer Joins

Now let’s address the problem we encountered before when we left out CS189 because it did not have any enrollment information. This situation happens frequently. We still want to keep all the data from a relation even if it does not have a “match” in the table we are joining it with. To fix this problem we will use a **left outer join**. The left outer join makes sure that every row from the left table will appear in the output. If a row does not have any matches with the right table, the row is still included and the columns from the right table are filled in with `NULL`. Let’s fix our query:

```
SELECT *
FROM courses LEFT OUTER JOIN enrollment
ON num = c_num;
```



This will produce the following output:

| num   | name | c_num | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |
| CS189 | ML   | NULL  | NULL     |

Notice that CS189 is now included and the columns that should be from the right table `(c_num, students)` are `NULL`.

The **right outer join** is the exact same thing as the left outer join but it keeps all the rows from the right table instead of the left table. The following query is identical to the query above that uses the left outer join:

```
SELECT *
FROM enrollment RIGHT OUTER JOIN courses
ON num = c_num;
```



Notice that I flipped the order of the joins and changed `LEFT` to `RIGHT` because now courses is on the right side.

Let’s say we add a row to our enrollment table now:

| c_num | students |
| :---- | :------- |
| CS186 | 700      |
| CS188 | 800      |
| CS160 | 400      |

But we still want to present all of the information that we have. If we just use a left or a right join we have to pick between using all of the information in the left table or all of the information in the right table. With what we know so far, it is impossible for us to include the information that we have about *both* CS189 and CS160 because they occur in different tables and do not have matches in the other table. To fix this we can use the **full outer join** which guarantees that all rows from each table will appear in the output. If a row from either table does not have a match it will still show up in the output and the columns from the other table in the join will be `NULL`.

To include all of data we have let’s change the query to be:

```
SELECT *
FROM courses FULL OUTER JOIN enrollment
ON num = c_num;
```



which produces the following output:

| num   | name | c_num | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |
| CS189 | ML   | NULL  | NULL     |
| NULL  | NULL | CS160 | 400      |

## Name Conflicts

Up to this point our tables have had columns with different names. But what happens if we change the enrollment table so that it’s `c_num` column is now called `num`?

| num   | students |
| :---- | :------- |
| CS186 | 700      |
| CS188 | 800      |
| CS160 | 400      |

Now there is a `num` column in both tables, so simply using `num` in your query is ambiguous. We now have to specify which table’s column we are referring to. To do this, we put the table name and a period in front of the column name. Here is an example of doing an inner join of the two tables now:

```
SELECT *
FROM courses INNER JOIN enrollment
ON courses.num = enrollment.num
```



The result is:

| num   | name | num   | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |

It can be annoying to type out the entire table name each time we refer to it, so instead we can **alias** the table name. This allows us to rename the table for the rest of the query as something else (usually only a few characters). To do this, after listing the table in the `FROM` we add `AS &lt;alias name&gt;`. Here is an equivalent query that uses aliases:

```
SELECT *
FROM courses AS c INNER JOIN enrollment AS e
ON c.num = e.num;
```



The result is:

| num   | name | num   | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |

Aliases can also be used in the SELECT clause to rename the column names of the output. If we execute the following query:

```
SELECT c.num AS num1, c.name, e.num AS num2, e.students
FROM courses AS c INNER JOIN enrollment AS e
ON c.num = e.num;
```



The output will be:

| num1  | name | num2  | students |
| :---- | :--- | :---- | :------- |
| CS186 | DB   | CS186 | 700      |
| CS188 | AI   | CS188 | 800      |

## Natural Join

Often in relational databases, the columns you want to join on will have the same name. To make it easier to write queries, SQL has the **natural join** which automatically does an equijoin (equijoin `=` checks if columns are equivalent) on columns with the same name in different tables. The following query is the same as explicitly doing an inner join on the `num` columns in each table:

```
SELECT *
FROM courses NATURAL JOIN enrollment;
```



The join condition: `courses.num = enrollment.num` is implicit. While this is convenient, natural joins are not often used in practice because they are confusing to read and because adding columns that are not related to the query can change the output.

## Subqueries

Subqueries allow you to write more powerful queries. Let’s look at an example…

Let’s say you want to find the course num of every course that has a higher than average num of students. You cannot include an aggregation expression (like AVG) in the WHERE clause because aggregation happens after rows have been filtered. This may seem challenging at first, but sub-queries make it easy:

```
SELECT num
FROM enrollment;
WHERE students &gt;= (
    SELECT AVG(students)
    FROM enrollment;
);
```



The output of this query is:

| num   |
| :---- |
| CS186 |
| CS188 |

The inner subquery calculated the average and returned one row. The outer query compared the `students` value for each row to what the subquery returned to determine if the row should be kept. Note that this query would be invalid if the subquery returned more than one row because `&gt;=` is meaningless for more than one number. If it returned more than one row we would have to use a set operator like `ALL`.

## Correlated Subqueries

The subquery can also be correlated with the outer query. Each row essentially gets plugged in to the subquery and then the subquery uses the values of that row. To illustrate this point, let’s write a query that returns all of the classes that appear in both tables.

```
SELECT *
FROM classes
WHERE EXISTS (
    SELECT *
    FROM enrollment
    WHERE classes.num = enrollment.num
);
```



As expected, this query returns:

| num   | name |
| :---- | :--- |
| CS186 | AI   |
| CS188 | DB   |

Let’s start by examining the subquery. It compares the `classes.num` (the num of the class from the current row) to every `enrollment.num` and returns the row if they match. Therefore, the only rows that will ever be returned are rows with classes that occur in each table. The `EXISTS` keyword is a set operator that returns true if any rows are returned by the subquery and false if otherwise. For `CS186` and `CS188` it will return true (because a row is returned by the subquery), but for `CS189` it will return false. There are a lot of other set operators you should know (including `ANY`, `ALL`, `UNION`, `INTERSECT`, `DIFFERENCE`, `IN`) but we will not cover any others in this note (there is plenty of documentation for these operators online).

## Subqueries in the From

You can also use subqueries in the `FROM` clause. This lets you create a temporary table to query from. Here is an example:

```
SELECT *
FROM (
    SELECT num
    FROM classes
) AS a
WHERE num = &#39;CS186&#39;;
```



Returns:

| num   |
| :---- |
| CS186 |

The subquery returns only the `num` column of the original table, so only the `num` column will appear in the output. One thing to note is that subqueries in the `FROM` cannot usually be correlated with other tables listed in the `FROM`. There is a work around for this, but it is out of scope for this course. A cleaner way of doing this is using common table expressions (or views if you want to reuse the temporary table in other queries) but we will not cover this in the note.

## Subquery Factoring

Subquery factoring can simplify queries by giving a subquery a name to be used later. To do this, we use the `WITH` clause:

```
WITH courseEnrollment AS (
    SELECT c.num AS num1, c.name, e.num AS num2, e.students
    FROM courses AS c INNER JOIN enrollment AS e
    ON c.num = e.num;
)
```



We can then treat `courseEnrollment` as if it were its own table in a future query:

```
SELECT num1, name, students
FROM courseEnrollment
WHERE students &gt; 700;
```



Returns:

| num1  | name | students |
| :---- | :--- | :------- |
| CS188 | AI   | 800      |

The subquery returns only the `num` column of the original table, so only the `num` column will appear in the output. One thing to note is that subqueries in the `FROM` cannot usually be correlated with other tables listed in the `FROM`. There is a work around for this, but it is out of scope for this course. A cleaner way of doing this is using common table expressions (or views if you want to reuse the temporary table in other queries) but we will not cover this in the note.

---

> Author: Kire  
> URL: http://localhost:1313/posts/database-systems/  

