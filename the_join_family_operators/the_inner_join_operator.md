# The Inner Join Operator

<br>
<br>

### Basic `INNER JOIN` Query

Let's create a query that you should be familiar with, which outputs the `Title` and `Rating` of all the movies.

Like so:

```sql
SELECT Title, Rating
FROM Movie, Review
WHERE Movie.mID = Review.mID;
```

In the above query, we make a join relation making sure that the movie ID is the same across the `Movie` and `Review` table.

Now let's rewrite it using an `INNER JOIN`:

```sql
SELECT Title, RATING
FROM Movie INNER JOIN Review
ON Movie.mID = Review.mID;
```

This query does the `INNER JOIN`, or the combination of `Movie` and `Review`, **ON** a specific condition. So it does the cross product of the two tables, then after doing the cross product, it checks the condition and only returns the elements that satisfy the condition.

We would then get a table in return with every movie in the database and its `Rating`.

The `INNER JOIN` operator is the default operator in SQL, and even if you were to take out the `INNER` and just write: `<table> JOIN <table>`, it would default to an `INNER JOIN`.

<br>

### Inner Join with Multiple Conditions

Let's create another query that gets the `Title` and `Rating` of all movies whose `Rating` is greater than 3, and was produced after the year 1990.

Like so:

```sql
SELECT Title, Rating
FROM Movie, Review
WHERE Movie.mID = Review.mID
    and Rating > 3 and Year > 1990;
```

Let's now rewrite this query to use the `INNER JOIN` operator. Our query would look like this:

```sql
SELECT Title, Rating
FROM Movie JOIN Rating
ON Movie.mID = Review.mID
    and Rating > 3 and Year > 1990;
```

Our query selects all movies whose `Rating` is greater than 3, and whose `Year` is greater than 1990. It joins the `Movie` and `Review` tables, and the join relation is again combining the `Movie` and `Review` records where the `mID` matches. It then checks the condition and returns the tuples that satisfy the condition.

We would then get the following movies in return: *Gravity*, *The Lion King*, *Titanic*, and *Cast Away*.

The `ON` condition can also be ran using the `WHERE` clause, but it's more efficient to use the `ON` operator for reasons I will not get into here.

<br>

### Running a Query with Three Relations

We will create a query that just gets all the general information on each individual movie, and also return all the user's names.

Our query will look like this:

```sql
SELECT Movie.mID, Title, Year, Director, Rating, User.uID Name
FROM Movie, User, Review
WHERE Movie.mID = Review.mID
    and User.uID = Review.uID;
```

Now lets rewrite it using a join operator:

```sql
SELECT Movie.mID, Title, Year, Director, Rating, User.uID, Name
FROM Movie JOIN User JOIN Review
ON Movie.mID = Review.mID
    and User.uID = Review.uID;
```

In this particular query, we would **possibly** get an error depending on the type of system that you are using. A few SQL sytems are: SQLite, MySQL, and Postrisk.

If working in the Postrisk system, we would get an error when running the above query because Postrisk does not support multiple join operators. It requires all join operations to be binary, meaning it can only join two relations. If running on the Postrisk system, you could rewrite the query to look like this:

```sql
SELECT Movie.mID, Title, Year, Director, Rating, User.uID, Name
FROM (Movie JOIN Review ON Movie.mID = Review.mID) JOIN User
ON User.uID = Review.uID;
```

Notice in the above query we joined the two relations `Movie` and `Review` and then wrapped them in parenthesis. This allows for it to satisfy the Postrisk's requirement for all join relations to be binary. We are saying "First join the `Movie` and `Review` table, then join that result with the `User` table. Lastly, we moved the `ON` condition inside the parenthesis for that particular join operator.
