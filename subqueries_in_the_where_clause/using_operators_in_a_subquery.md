# Using Operators in a Subquery

<br>
<br>

### The Exist Operator & Correlated References

The `EXISTS` operator checks whether a subquery is empty or not, instead of checking whether values are in the subquery.

A correlated reference is where you use a value inside a subquery, that comes from *outside* that subquery.

Lets look at an example:

```sql
SELECT mID, Rating
FROM Review R1
WHERE EXISTS (SELECT * FROM Review R2
              WHERE R1.Rating = R2.Rating and R1.mID <> R2.mID);
```
This query will return all movies that have the same rating. First we're going to take the `mID`'s from `R1`. Then we're creating a new relation called `R2`. For each movie we're going to check if there is another `mID`, where the `Rating` in `R2` is the same as the `Rating` in `R1`. We then say that each `mID` should be different, and not equal to itself.

We use a correlated reference to use an outside variable *inside* a subquery.

<br>

### Looking for a Largest Value

Assume that you wanted to look for the largest value of some element. In this case, we want to find the movie that was most recently created. Thus, the movie's `Year` would be the largest.

We could write a query that looks like this:

```sql
SELECT Title, Year
FROM Movie M1
WHERE NOT EXISTS (SELECT * FROM Movie M2
                  WHERE M1.Year < M2.Year);
```

This query says that we are going to find all movies where there does **not exist** another movie whose `Year` is greater than the first movie.
This would be a form of query that we could write whenever looking for the greatest value of some-sort.

The resulting movie would be: *Gravity*.

<br>

### The All Operator

The `ALL` keyword tells us that instead of checking whether a value is in or not in the result of a subquery, we're going to check if the value has a certain relationship with `ALL` the results of a subquery.

Lets create a query that checks to see if the `Rating` of a movie is greater than or equal to `ALL` elements of the subquery which returns all the `Ratings` of each movie.

It would look like this:

```sql
SELECT mID, Rating
FROM Review
WHERE Rating >= all (SELECT Rating FROM Review);
```

We would then get an output table of all the movie's with a `Rating` of 5, since there is no single movie with a greater `Rating` than every other movie.

The output table would include the movies: *Gravity*, and *Titanic*.

<br>

### The Any Operator

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
