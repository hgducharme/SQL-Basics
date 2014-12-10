# Subqueries in the Where Clause

<br>
<br>

### Basic Syntax

Just like before, our queries will contain a `SELECT` clause, a `FROM` clause, and a `WHERE` clause specifying a condition. However, we now are adding in the ability to nest a `SELECT` clause inside the `WHERE` clause, thus creating a subquery.

Subqueries can be very powerful when trying to eliminate duplicates, and is often more efficient than using joining relations.

<br>

### Creating a Basic Subquery

Let's create a query that looks for a movie's ID, title, and director, but only if it has a rating above 4.

We can create a sub-query like so:

```sql
SELECT DISTINCT Movie.mID, Title, Director
FROM Movie, Review
WHERE Movie.mID in (SELECT mID FROM Review WHERE Rating > 4);
```

We could easily do this query without implementing a subquery by joining the Movie relation with the Review relation. However, this is just to show how a subquery would be performed.

We would then get the movies: *Titanic*, and *Gravity*.

<br>

### Slightly More Complex Subqueries

Lets create a query that retrieves the `Title` of all movies which have a `Rating` less than 3, and have a `mID` greater than 103.

Our query would look like this:

```sql
SELECT Title
FROM Movie
WHERE mID in (SELECT mID FROM Review WHERE Rating < 3)
  AND mID NOT IN (SELECT mID FROM Review WHERE mID < 103);
```

Our outside query returns the `Title` of all movies whose `mID` is in the first subquery, but not in the second subquery. Our first subquery looks for all `mID`'s whose `Rating` is less than 3, and the second subquery looks for all `mID`s that are greater than 103.

The output movies would be: *Spiderman*, *Gravity*, and *Harry Potter*.
