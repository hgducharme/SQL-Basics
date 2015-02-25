# Subqueries in the From & Select Clause

<br>
<br>

### What They Are Used For

* When using subqueries in the `WHERE` clause, the subquery generates a set of elements that will be used in a comparison.
* When using subqueries in the `FROM` clause, the subquery generates a new table that will be used for the rest of the query.
* When using subqueries in the `SELECT` clause, the subquery produces a sub-select expression that returns a single value.

<br>

### Using a Subquery in the From Clause

Lets create a query that scales all the movie ratings depending on which year they were produced. For example, a movie produced in 2012 with a rating of 4 would be higher ranked than a movie produced in 1990 with a rating of 4. Lets assume that throughout the years in the movie industry, the CGI and film effects get better, thus producing a better movie. This technology wouldn't be available in the previous years, so older movie's ratings won't hold the same value as present day movies.

Our query would look like this:

```sql
SELECT *
FROM (SELECT M.mID, Title, Year, Rating, Rating*(Year/1000.0) as scaledRating
    FROM Movie M, Review
    WHERE M.mID = Review.mID) sR
WHERE abs(sR.scaledRating) > 8;
```
This query is saying that it will `SELECT` all attributes `FROM` a table produced by a subquery. This subquery is going to produced a table that has the `mID`, `Title`, `Year`, `Rating`, then a scaled rating with a column name as `scaledRating`. The results of this subquery has the table variable name of `sR`. Then in the main query, we only want to output the movies where the absolute value (`abs()`) of the `scaledRating` is greater than 8.

We would then get output the following movies: *Titanic*, and *Gravity*.

<br>

### Using Subqueries in the Select Clause

Lets create a query that lists all the users and pairs them with the highest rating that they have given.

The query would look like so:

```sql
SELECT uID, Name
(SELECT DISTINCT Rating
 FROM User, Review
 WHERE User.uID = Review.uID
    and Rating >= ALL
        (SELECT Rating
         FROM User, Review
         WHERE User.uID = Review.uID)) as hRating
FROM User;
```

This query says that we are going to find `uID` and that user's `Name` from the `User` table, and then it runs a subquery. The subquery looks for a `Rating` inside the table `Review`. We make sure to join the `uID` from the `User` table to the `uID` in the `Review` table. We then choose the largest `Rating` among `ALL` the `Rating`s that are associated with that user. Lastly, we give the subquery result's a variable name, which takes the name of `hRating`, which stands for 'highest rating'.

In other words, we would get a resutling table with each user's `uID` and `Name`. Then, it would have a column labeled `hRating` which has the highest rating that each individual user has ever done.

Our query would ouput these results:

| uID | Name             | hRating |
| --- | ---------------- | ------- |
| 201 | James Dean       | 4       |
| 202 | Chris Anderson   | 4       |
| 203 | Ashley Burley    | 2       |
| 204 | Ralph Truman     | 4       |
| 205 | Gordon Maximus   | 3       |
| 206 | Sarah Rodgriguez | 3       |
| 207 | Darrel Sherman   | 5       |
| 208 | Lisa Jackson     | 2       |

<br>

### Important Note on Subqueries in the Select Clause

When implenting a subquery in the `SELECT` clause, it is crucial that the subquery only returns exactly one value, because the result of that subquery is being used to only fill in one cell of the parent query.
