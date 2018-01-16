# Aggregation

<br>
<br>

### Aggregation Functions

The aggregate, or aggregation functions, initially appear inside the `SELECT` clause of a query, and they perform computations over sets of values in multiple rows of our relations. The basic aggregation functions supported by every SQL systems are: `MIN`, `MAX`, `SUM`, `AVG`, and  `COUNT`.

<br>

### Two New Clauses

Now that the aggregation functions have been introduced, two new clauses can be added to the SQL select statements. These are the: `GROUP BY`, and the `HAVING` clauses.
* The `GROUP BY` allows us to partition our relations into groups, and then compute aggregated aggregate functions over each group independently.
* The `HAVING` condition allows us to test filters on the results of aggregate values.
* The difference between the `WHERE` and `HAVING` conditions is the `HAVING` applies to all the groups generated from the `GROUP BY` clause. While the `WHERE` condition applies to single rows at a time.

The syntax looks like this:

```sql
SELECT A1, A2, . . . , A(n)
FROM R1, R2, . . . , R(m)
WHERE <condition>
GROUP BY <columns>
HAVING <condition>;
```



