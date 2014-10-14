# Subqueries in the Where Clause

// TODO -- Seperate the subquery portions, and the operator/keyword portions.

<br>
<br>

### Basic Syntax

Just like before, our queries will contain a `SELECT` clause, a `FROM` clause, and a `WHERE` clause specifying a condition. However, we now are add in the ability to nest a `SELECT` clause inside the `WHERE` clause, thus creating a subquery.

Subqueries can be very powerful when trying to eliminate duplicates, and where joining relations has loopholes in its efficiency.

<br>

### Creating a Basic Subquery

Lets create a query that looks for a movie's ID, title, and director, but only if it has a rating above 4.

We can create a sub-query like so:

```sql
SELECT mID, Title, Director
FROM Movie, Review
WHERE mID in (SELECT mID FROM Review WHERE Rating > 4);
```

We could easily do this query without implementing a subquery by joining the Movie relation with the Review relation. However, this is just to show how a subquery would be performed.

<br>

### Slightly More Complex Subqueries

Lets create a query that retrieves the `Title` of all movies who have a `Rating` less than 3, and have a `mID` greater than 103.

Our query would look like this:

```sql
SELECT Title
FROM Movie
WHERE mID in (SELECT mID FROM Review WHERE Rating < 3)
  AND mID NOT IN (SELECT mID FROM Review WHERE mID < 103);
```

Our outside query returns the `Title` of all movies whose `mID` is in the first subquery, but not in the second subquery. Our first subquery looks for all `mID`'s whose `Rating` is greater than 3, and the second subquery looks for all `mID`s that are greater than 103.

<br>

### The Exist Operator & Correlated References

The `EXIST` operator checks whether a subquery is empty or not, instead of checking whether values are in the subquery.

A correlated reference is where you use a value inside a subquery, that comes from *outside* that subquery.

Lets look at an example:

```sql
SELECT mID, Rating
FROM Review R1
WHERE EXISTS (SELECT * FROM Review R2
              WHERE R1.Rating = R2.Rating and R1.mID <> R2.mID);
```

This query says that we're going to take the `mID`'s, and for each movie we're going to check to see if there is another `mID` and where going to call that one `R2`, where the `Rating` of `R2` is the same as the `Rating` of `R1`. We then say that each `mID` should be different, and not equal itself.

We use a correlated reference to use an outside variable *inside* a subquery.

<br>

### Looking for a Largest Value

Assume that you wanted to look for the largest value of some element. In this case, we want to find the movie that was most recently created. Thus, the movie's `Year` would be the largest.

We could write a query that looks like this:

```sql
SELECT Title, Year
FROM Movie M1
WHERE NOT EXISTS (SELECT * FROM Movie M2
                  WHERE M1.Year > M2.Year);
```

This query says that we are going to find all movies where there does **not exist** another movie whose `Year` is greater than the first movie.

This would be a form of query that we could write whenever looking for the greatest value of some-sort.

<br>

### The All Keyword

The `ALL` keyword tells us that instead of checking whether a value is in or not in the result of a subquery, we're going to check if the value has a certain relationship with `ALL` the results of a subquery.

Lets create a query that checks to see if the `Rating` of a movie is greater than or equal to `ALL` elements of the subquery which returns all the `Ratings` of each movie.

It would look like this:

```sql
SELECT mID, Rating
FROM Review
WHERE Rating >= all (SELECT Rating FROM Review);
```

We would then get an output table of all the movie's with a `Rating` of 5, since there is no single movie with a greater `Rating` than every other movie.

<br>

### The Any Keyword

The `ANY` keyword performs very similar to the `ALL` keyword, except instead of having to satisfy a condition with `ALL` of the elements of a set, it only has to satisfy a condition with at least one element of a set.

Lets create a query that finds all movies that have a `Year` that is not the smallest `Year` value. In other words, we are looking for movies whose `Year` is greater than `ANY` other movie `Year`.

Our query would look like this:

```sql
SELECT Title, Year
FROM Movie
WHERE Year > ANY (SELECT Year FROM Movie);
```

In the above example query, a movie will be returned if there is some other movie whose `Year` is less than this movie. We then get a resulting table with all the movies that do not have the least `Year` value. Thus, we would get every movie except for *The Godfather*, because it has the smallest `Year` value.

<br>

### Conclusion on Operators

The `ANY` and `ALL` operators are very convienient when creating queries, however, they are not vital to creating a query. We can always write a query that would normally use the `ANY` or `ALL` keywords, by using the `EXISTS` or `NOT EXISTS` operators.
