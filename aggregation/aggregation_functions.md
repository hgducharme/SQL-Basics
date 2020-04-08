# Aggregation Functions

<br>
<br>

### Basic Aggregation Query

Our first aggregation query is going to compute the average movie `Rating` of the movies in the database.

Like so:

```sql
SELECT AVG(Rating)
FROM Review;
```

We would then get a single cell back with the result of: `3.333333333`. All the ratings add up to 40, and there are twelve ratings, so 40/12 = 3.33 repeating.

<br>

### Aggregation and Joins

Our second query is going to be a little more complicated. It finds the minimum `Rating` of movies that were produced before the year `2000`.

Our query would look like this:

```sql
SELECT MIN(Rating)
FROM Movie, Review
WHERE Movie.mID = Review.mID and Year < 2000
```

The above query is saying that the aggregation is going to look at the `Rating` column and it's going to find the lowest value. It is going to look inside the `Movie` and `Review` tables, it will join the `mID` across both relations, and will filter for movies that were produced before the year 2000.

Our resulting movie would be *Top Gun*.

Now lets go back to our `AVG` aggregation query again. We will again compute the average `Rating` of all the movies that were produced after the year 1995. However, we have to change up the query because some movies were rated more than once, and we don't want to include the duplicate ratings in our average rating computation. We only want to count the `Rating` one time for each movie. In order to do that, we need to use a subquery from where we select from `Review`, and then we just want to check for each movie whether their ID is among those whose year is greater than 1995.

Our query would look like this:

```sql
SELECT AVG(Rating)
FROM Review
WHERE mID IN (SELECT mID FROM Movie WHERE Year > 1995);
```

So now we would get a resulting table where the `Rating` for each movie is only counted once in the computation. Our average `Rating` for all movies produced after the year 1995 would be: 3.

<br>

### The `COUNT` Function

The `COUNT` function, not suprisingly, counts the number of tuples in the result that meet the `WHERE` condition.

Like so:

```sql
SELECT COUNT(*)
FROM Movie
WHERE Year > 1990;
```

We are `SELECT`ing all attributes inside the `Movie` table, and we are going to `COUNT` the number of movies whose year is greater than 1990.

Our result would be the number 6, because the following movies all were produced after the year 1990:  *The Lion King*, *Titanic*, *Gravity*, *Harry Potter*, *Cast Away*, and *Spiderman*.

Now lets create another query that counts the number of movies with a `Rating` greater than 3.

It would look like this:

```sql
SELECT COUNT(*)
FROM Review
WHERE Rating > 3;
```

We would then get a result of 6. However, there are some movies that were rated more than once and who have multiple ratings greater than 3, so we want to eliminate the duplicates and only count each movie once.

Our new query would look like this:

```sql
SELECT COUNT(DISTINCT mID)
FROM Review
WHERE Rating > 3;
```

SQL includes a nice keyword for us ot use in this particular query. In the `COUNT` function we put the `DISTINCT` keyword and then the name of the attribute that `COUNT` will look for. In this case, `COUNT` will look at the result, and then it will count the distinct values for the particular attribute.

Our result would be the number 5, because the movie *Gravity* was rated twice with a `Rating` greater than 3. So now we eliminate the duplicate and get our correct result by only counting each movie once in the computation.

<br>

### Aggregation in Subqueries

We're going to write up a fairly complicated query this time. This query computes the difference of the average `Rating` between movies produced after the year 2000, and movies produced before the year 2000.

Our query will look like this:

```sql
SELECT Post.avgRating - Pre.avgRating
FROM
    (SELECT AVG(Rating) as avgRating
    FROM Review
    WHERE mID IN
        (SELECT mID FROM Movie WHERE Year >= 2000)) as Post
    (SELECT AVG(Rating) as avgRating
    FROM Review
    WHERE mID NOT IN
        (SELECT mID FROM Movie WHERE Year >= 2000)) as Pre;
```

In the above query we are using subqueries in the `FROM` clause. Recall from earlier chapters that a subquery in the `FROM` clause allows you to write a select statement, and then use the result as if it were an actual relation in the database. So we are going to compute two subqueries in the `FROM` clause, one of them computing the average `Rating` of movies that were produced on or after the year 2000, and the second one computing the average `Rating` of movies that were NOT produced on or after the year 2000.

So lets walk through the query:
* The first subquery says, let's find the movies whose `Year` is greater than or equal to 2000, let's compute their average `Rating`, and we will call it `avgRating`. We will take the whole result of this query and then name it `Post`, as in post-2000.
* Similarly the second relation that we are computing in the `FROM` clause computes the average `Rating` of movies whose `Year` is not greater than or equal to 2000, so their `mID` is `NOT IN` the set of movies whose `Year` is greater than 2000. We then name the result of this query `Pre`, as in pre-2000.
* To conclude, in the `FROM` clause we now have a relation called `Post` with an attribute called `avgRating`, and a second relation called `Pre` with an attribute called `avgRating`. Then, in the `SELECT` clause of the main query, we subtract the `avgRating` of movies from `Pre` from the `avgRating` of movies from `Post`.

If we were to run the query, we would get the result of: 0. Thus, that means the average `Rating` of movies produced before the year 2000 is exactly the same as the average `Rating` of movies produced on or after the year 2000.
