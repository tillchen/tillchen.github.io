# SQL Notes


* [Basics](#basics)
* [SELECT](#select)
* [DELETE](#delete)
* [UPDATE](#update)
* [ALTER](#alter)
* [References](#references)

## Basics

1. Create and use the database (MySQL as an example)

    ```sql
    CREATE DATABASE gregs_list;
    USE gregs_list;
    CREATE TABLE doughnut_list
    (
        doughnut_id INT NOT NULL AUTO_INCREMENT,
        doughnut_name VARCHAR(10) DEFAULT NULL,
        doughnut_type VARCHAR(8) DEFAULT NULL,
        PRIMARY KEY (doughnut_id)
    ); -- Variable Char, up to 10 and 8 chars.
    DESC doughnut_list; -- Describe the table.
    -- DROP TABLE doughnut_list; -- Delete the table.
    INSERT INTO doughnut_list (doughnut_name, doughnut_type) VALUES ('hello', 'world'); -- The order doesn't matter, but they must match
    SELECT * FROM doughnut_list; -- Show the table. * means all columns.
    SELECT doughnut_name FROM doughnut_list; -- Just show the name column
    SELECT * FROM doughnut_list WHERE doughnut_name = 'hello' AND doughnut_type = 'world'; -- Show the table selectively. = instead of ==
    ```

2. For `INSERT`, there are three variations from above:

    * Changing the order of the columns, as long as the values match them in the same order.
    * Omitting column names altogether, as long as we add all the values in the same order.
    * Leaving some columns out, as long as the values we're adding match the columns.

3. `\'` or `''` to insert data with the single quote.

## SELECT

1. `<>` means not equal.

2. Use `IS NULL` to select null ones: `WHERE doughnut_name IS NULL;`.

3. `WHERE location LIKE '%CA';` selects all the values that end with CA. `%` replaces anything while `_` replaces one letter.

4. `WHERE value BETWEEN 1 AND 10;`.

5. `WHERE language IN ('Java', 'Python', 'C', 'C++');` is equivalent to using `OR`. Plus, we have `WHERE language NOT IN ('Java', 'Python', 'C', 'C++');`.

6. `NOT` goes right after `WHERE` when used with `BETWEEN` and `LIKE` : `WHERE NOT location LIKE '%CA';` `WHERE NOT value BETWEEN 1 AND 10;`.

7. `ORDER BY foo DESC, bar;` can be added to the end (DESC means descending.)

## DELETE

1. `DELETE FROM doughnut_list WHERE doughnut_type = 'bar';`.

2. `DELETE FROM doughnut_list;` deletes the entire table.

3. Use `SELECT` first to make sure what we're deleting is correct.

## UPDATE

1. `UPDATE doughnut_list SET doughnut_type = 'barbar' WHERE doughnut_type = 'bar';`.

2. Without `WHERE`, every possible one will be updated.

3. Use comma to seperate different columns after `SET`.

4. It can do basic math: `SET cost = cost + 1`.

## ALTER

1. `ALTER TABLE doughnut_list ADD COLUMN doughnut_id INT NOT NULL AUTO_INCREMENT FIRST, ADD PRIMARY KEY (doughnut_id);`

2. `FIRST`, `SECOND`, `LAST`, `BEFORE foo`, `AFTER foo` can be used.

3. `ALTER TABLE doughnut_list RENAME TO foo;`.

4. `ALTER TABLE doughnut_list CHANGE COlUMN foo bar INT;` changes foo to bar with int.

5. `ALTER TABLE doughnut_list MODIFY COLUMN foo VARCHAR(10);` changes the data type. We can also do `FIRST` or `LAST`.

6. `ALTER TABLE doughnut_list DROP COLUMN foo;` deletes a column.

## References

* [Head First SQL](https://www.amazon.com/Head-First-SQL-Brain-Learners/dp/0596526849/ref=sr_1_1?keywords=head+first+sql&qid=1564318633&s=gateway&sr=8-1)

