# The Outer Join Operator

<br>
<br>

### Basic Query

Let's start with a query that joins using the `INNER JOIN` operator which will join `Movie` and `Review` on the matching movie IDs, and will return a few attributes.

Like so:

```sql
SELECT Movie.mID, Title, Rating, ratingDate
FROM Movie INNER JOIN Review USING(mID);
```

Now let's assume that we want results with movies that *do not* have a rating date. That is, there is a `<null>` value for that particular element.

Our query would look like this:

```sql
SELECT Movie.mID, Title, Rating, ratingDate
FROM Movie RIGHT OUTER JOIN Review USING(mID);
```

Notice that we changed the `INNER JOIN` to a `RIGHT OUTER JOIN` operator. We would get our exact same results as before, which would be table with all the movies and some of their data, such as `Rating` and `ratingDate`. What the `RIGHT OUTER JOIN` operator does is that it takes the tuples on the right side of the relation, and if they don't have a matching tuple on the left then it's still added to the results and padded with `null` values. When there is a tuple on the right with no matching tuple on the left side of the relation, that is called a dangling tuple, and the `RIGHT OUTER JOIN` includes all dangling tuples in its results.

You can also abbreviate the `RIGHT OUTER JOIN` to just `<LEFT/RIGHT> JOIN` and it will produce the same results. On top of that, the `OUTER` join can also be combined with the `NATURAL JOIN` by joining relations like this: `NATURAL <LEFT/RIGHT> OUTER JOIN`.

In our previous query, we checked to see if a tuple in the `Review` table matched the tuple in the `Movie` table. Now what if we wanted to do it the other way around? For example, check to see if a tuple in the `Movie` table matches a tuple in the `Review` table. You might assume that we could just switch the relations around and put `Movie` on the right, and `Review` on the left. However, SQL actually has a counterpart to the `RIGHT OUTER JOIN` operator, and it is the `LEFT OUTER JOIN`.

It would look like this:

```sql
SELECT Movie.mID, Title, Director, Rating
FROM Movie LEFT OUTER JOIN Review USING(mID);
```

In the above query we are checking to see if the tuples in the `Movie` table match any tuples in the `Review` table. Instead of checking it the other way around like in our previous query. We are also including the `Director` names in this query, and since the `Director` tuples won't match any tuples in the `Review` table, they will be considered dangling tuples. However, recall that the `OUTER JOIN` operator includes dangling tuples in the result, so no need to worry!

<br>

### The `FULL OUTER JOIN` Operator

So we know how to include dangling tuples from one relation into our results, but what if we want to include unmatched results from both the left *and* the right side relation? In this case, we use the `FULL OUTER JOIN` operator.

Let's create another query that includes `null` values from both the `ratingDate` and `Director` columns.

Our query would look like this:

```sql
SELECT Movie.mID, Title, Director, Rating, ratingDate
FROM Movie FULL OUTER JOIN Review USING(mID);
```

We would then get a table of results in return with all the values for each column even the ones that are `null`.
