# Data Modification Statements

<br>
<br>

### Inserting Data

In SQL, there are statements for inserting data, deleting data, and modifying data in a database. For inserting data, there are two methods:

```sql
INSERT INTO <table>
    VALUES(A1, A2, . . ., An);
```

The first method allows us to insert one tuple into the database by specifying its actual value. The command above is saying to `INSERT INTO` a table, then specify the value of that tuple, and the result of the command will be to insert one new tuple into that table with the specified value.

The other method of inserting data into a table is to run a query over the database as a select statement. That select statement will produce a set of tuples, and as long as that set of tuples has the same schema as the table, we could insert all of the tuples into the table.

Like this:

```sql
INSERT INTO <table>
    SELECT STATEMENT;
```

<br>

### Deleting Data

Deleting data is fairly simple, and looks like this:

```sql
DELETE FROM <table>
WHERE <condition>;
```

The above query is saying to `DELETE FROM` a table, `WHERE` a certain condition is true, so this condition is similar to the conditions that we see in the select statement. Then every tuple in the table that satisfies the given condition will be deleted.

This condition can sometimes get fairly complicated, because it can include subqueries, and aggregation over other tables.

<br>

### Updating Data

Updating data is done through a command similar to the `DELETE FROM` command. It similarily operates on a single table, it then evaluates a condition over each tuple of the table, and when the condition is true, it will modify the tuple.

It looks like this:

```sql
UPDATE <table>
SET <attr> = <expression>
WHERE <condition>;
```

The above query takes an attribute that is specified and reassigns it to have the value that is the result of the expression. The condition can also get fairly complicated, for it can have subqueries and so on. As well as the expression, for it can involve queries over other tables or the same table in the database.

You can also update multiple attributes in a tuple. You can update any number of attributes simultaneously by evaluating an expression for each, and assigning the result of that expression to the attribute.

Like so:

```sql
UPDATE <table>
SET A1=<expr>, A2=<expr>, ...,  An=<expr>
WHERE <condition>;
```
