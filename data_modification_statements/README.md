# Data Modification Statements

<br>
<br>

### Inserting Data

In SQL, there are statements for inserting data, deleting data, and modifying data in a databse. For inserting data, there are two methods:

```sql
INSERT INTO <table>
    Values(A1, A2, . . ., An);
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
WHERE <condition>
```
