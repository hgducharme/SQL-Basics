# Aggregation Clauses

<br>
<br>

### The `GROUP BY` Clause

The `GROUP BY` clause is only used in conjuction with aggregation. Our first query is going to find the number of movies that were produced in each year, and it's going to do so by using grouping. Essentially what grouping does is it takes a relation and it partitions it by values of a given attribute or set of attributes.

```sql
SELECT Year, COUNT(*)
FROM Movie
GROUP BY Year;
```

Specifically in this query we're taking the `Movie` relation and we're breaking it into multiple groups. Each group is represented by each individual year. Then for each group we return one tuple in the result containing the `Year` for the group and the number of tuples in the group.

We would then get the number 1 for each year, because there is no two movies in our database that were produced in the same year.

<br>

### The `HAVING` Clause

The `HAVING` clause is another clause that is only used with aggregation. The `HAVING` clause allows us to apply conditions to the results of the aggregate functions. The `HAVING` clause is placed after the `GROUP BY` clause and it allows us to check conditions that involve the entire group. In contrast, the `WHERE` clause applies only to one tuple at a time.

Let's create a query that finds directors which have produced more than one movie.

Our query will look like this:

```sql
SELECT Director
FROM Movie
GROUP BY Director
HAVING COUNT(DISTINCT mID) > 1;
```

The above query is going to get each `Director` from the `Movie` table, and then put them into their own groups. For each group, or `Director`, it is going to check to see if there are more than one `mID`'s that are associated with that `Director`. If there is, the `Director` will be returned in the results. If not, then the `Director` will not be returned.

For this particular query, we would either get an error or a `null` value, because there is no director in our database that has produced more than one movie.
