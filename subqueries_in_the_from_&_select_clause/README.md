# Subqueries in the From & Select Clause

<br>
<br>

### What They Are Used For

* When using subqueries in the `WHERE` clause, the subquery generates a set of elements that will be used in a comparison.
* When using subqueries in the `FROM` clause, the subquery generates a new table that will be used for the rest of the query.
* When using subqueries in the `SELECT` clause, the subquery is producing a sub-select expression that gives a value that comes out of the query.

<br>

### Using a Subquery in the From Clause

Lets create a query that scales all the movie ratings depending on which year they were produced. For example, a movie produced in 2012 with a rating of 4 would be higher ranked than a movie produced in 1990 with a rating of 4. Lets assume that throughout the years in the movie industry, the CGI and film effects get better, thus producing a better movie. This technology wouldn't be available in the previous years, so older movie's ratings won't hold the same value as present day movies.

Our query would look like this:

```sql
SELECT *
FROM (SELECT M.mID, Title, Year, Rating, Rating*(Year/1000.0) as scaledRating
    FROM Movie, Review
    WHERE M.mID = Review.mID) sR
WHERE abs(sR.scaledRating) > 8;
```
This query is saying that it will `SELECT` all attributes `FROM` a table produced by a subquery. This subquery is going to produced a table that has the `mID`, `Title`, `Year`, `Rating`, then a scaled rating with a column name as `scaledRating`. The results of this subquery has the table variable name of `sR`. Then in the main query, we only want to output the movies where the absolute value (`abs()`) of the `scaledRating` is greater than 8.

We would then get output the following movies: *Titanic*, *Gravity*, *Harry Potter*, and *Cast Away*.

<br>

### Using Subqueries in the Select Clause

Lets create a query that lists all the users and pairs them with their highest rating, and the title of the movie that was rated.

The query would look like so:

```sql
-- TODO - This needs to revised. Not sure if correct.
SELECT uID, Title
(SELECT Rating
FROM User, Review
WHERE User.uID = Review.uID
    and Movie.mID = Review.mID
    and Rating >= ALL
        (SELECT Rating FROM Review
        WHERE Movie.mID = Review.mID
        and User.uID = Review.uID)) as highestRating

FROM Movie, User;
```

[Insert explanation after this is revised]
