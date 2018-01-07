# The Natural Join Operator

<br>
<br>

### Basic `NATURAL JOIN` Query

Let's use one of our previous queries where we used the `INNER JOIN` to combine the `Movie` and `Review` in order to find the `Rating` that the each movie had.

It looked like this:

```sql
SELECT Title, Rating
FROM Movie INNER JOIN Review
ON Movie.mID = Review.mID;
```

As a reminder, the `NATURAL JOIN` operator takes two relations that have column names in common, and then it performs a cross-product that only keeps the tuples where the tuples have the same value in those common attribute names. For example, `Movie` and `Review` have the `mID` column in common. If I were to change the `INNER JOIN` to a `NATURAL JOIN`, they system will automatically apply this equality between the `mID` in the `Movie` relation and the `Review` relation.

It would look like this:

```sql
SELECT Title, Rating
FROM Movie NATURAL JOIN Review;
```

<br>

### `NATRUAL JOIN` with Additional Conditions

Let's go back to our query using the `INNER JOIN` that finds the movies whose `Rating` is greater than 3, and was produced after the year 1990.

```sql
SELECT Title, Rating
FROM Movie JOIN Rating
ON Movie.mID = Review.mID
    and Rating > 3 and Year > 1990;
```

Now if we changed this to using a `NATURAL JOIN`, it would look like this:

```sql
SELECT Title, Rating
FROM Movie NATURAL JOIN Rating
WHERE Rating > 3 and Year > 1990;
```

We changed `JOIN` to `NATURAL JOIN`, then deleted the `ON` condition and changed it to a `WHERE` clause. Lastly, we deleted join relation since `NATURAL JOIN` automatically equates columns with the same name.

We would then get the same movies we got before: *Gravity*, *The Lion King*, *Titanic*, and *Cast Away*.

<br>

### The `USING` Clause

There is a feature in SQL that goes with the `NATURAL JOIN` operator that is often regarded as better practice than just using `NATURAL JOIN`. That feature is the `USING` clause, and it explicitly lists the attributes that should be equated when joining two relations.

Using the previous `INNER JOIN` query, it would look like this:

```sql
SELECT Title, Rating
FROM Movie JOIN Rating
ON Movie.mID = Review.mID
    and Rating > 3 and Year > 1990;
```

Now if we changed this to using a `NATURAL JOIN` with `USING`, it would look like this:

```sql
SELECT Title, Rating
FROM Movie JOIN Rating USING(mID)
WHERE Rating > 3 and Year > 1990;
```

Notice that we deleted `NATURAL` and only kept the word `JOIN`. We then specify that the `mID` is the attribute that should be equated across `Movie` and `Review`.

We can only put in the `USING` clause any attributes that appear in *both* tables. If we tried to put an attribute that was only in one table, the query would not run and would return an error.

The reason this is considered better practice is because the `NATURAL JOIN` implicitly joins all columns that have the same name, when this may not be favored. For instance, it's possible to not realize that two relations have the same column name, and then the system will sort-of, under the covers, equate those values. Compared to specificaly stating which relations to join which will prevent the query from equating values that don't need to be equated. Also, in real applications there can often be upwards of 100 attributes in a relationship. Thus, making it more likely that you have attributes with the same name but aren't meant to get equated.
