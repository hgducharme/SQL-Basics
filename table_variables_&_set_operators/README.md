# Table Variables & Set Operators

<br>
<br>

### What are they?

Take a look at the code block below:

```sql
SELECT A1, A2, . . . , A(n)
FROM R1, R2, . . . , R(m)
WHERE <condition>;
```

All of the variables inside the `FROM` clause are called table variables. They help with making the query more readable, and they rename relations within the `FROM` clause when we have more than one instance of a relation.

The second construct is called a set operator. A few examples of them are: the `UNION` operator, the `INTERSECT` operator, and the `EXCEPT` operator.

<br>

### Adding Table Variables for Readability

Let's assume we want to make a query that outputs all the movies along with their movie ID, title, User ID, User name, and rating.

Our query would look like this:

```sql
SELECT Movie.mID, Title, User.uID, Name, Rating
FROM Movie, User, Review
WHERE Movie.mID = Review.mID and User.uID = Review.uID;
```

We would then get our expected table output. However, to make the query a little more readable, we can place variables inside the `FROM` clause, and replace all the table names with just the table variable.

Our query would look like this:

```sql
SELECT M.mID, Title, U.uID, Name, Rating
FROM Movie M, User U, Review R
WHERE M.mID = R.mID and U.uID = R.uID;
```

Notice how we added `M` after `Movie` inside the `FROM` clause. That is called adding a table variable, then where ever we used `Movie` in our entire select statement, we can just replace with `M`. We also did the same thing for `User U` and `Review R`.

<br>

### Adding Table Variables for Multiple Instances

In this next query, we want to find all movies that have the exact same rating. In order to do that, we need to have two instances of `Review`. We will call the first instance `R1` and the second instance `R2`. We also need to include `Movie` in the `FROM` clause to get the movie `Title`.

Our query would look like this:

```sql
SELECT DISTINCT Movie.mID, Title, R1.Rating
FROM Movie M, Review R1, Review R2 
WHERE R1.Rating = R2.Rating and R1.mID = M.mID and R1.mID <> R2.mID and R1.uID <> R2.uID;

```

We first added the two table variables: `R1` and `R2` to separate each instance. We are looking for the `mID`'s, `Title`'s, and `Rating`'s of each movie. We specify `DISTINCT` in the `WHERE` clause to remove duplicates. We then place a condition in the `WHERE` clause to specify that we want to output movies with the same rating. We include the join condition `R1.mID = M.mID` to make sure the movies are the same in each relation.
We then specify two final clauses: `and R1.mID <> R2.mID and R1.uID <> R2.uID;`. `R1.mID <> R2.mID` tells the computer that the `R1.mID` is different from `R2.mID`, or that movie 1 needs to be different from movie 2. If we didn't specify this clause, we would get an output of movies that equal themselves, because movie 1 would have the same `Rating` as itself, thus satisfying the condition. Likewise, `R1.uID <> R2.uID` makes sure we don't get back two instances of a review by the same reviewer.
<br>

### The Union Operator

The `UNION` operator allows us to create queries that will output a list of elements that come from multiple tables. Previously we could only separate these elements into different columns. However, by using the `UNION` operator, we can get elements from different tables listed into a single column together.

For example, if we wanted to get a single list of all the movie titles and reviewer names, we would create a query that looks like this:

```sql
SELECT Title FROM Movie
UNION
SELECT Name FROM User;
```

We would then get a table with only **one** column, and it would list each movie title and each reviewer name.

<br>

### Specifying a Column Name

By default, SQL would pick label the column either `Title` or `Name`. If you wanted to specify a label for the column, you would use the `AS` operator.

It would look like this:

```sql
SELECT Title AS list FROM Movie
UNION
SELECT Name AS list FROM User;
```

Notice how we placed `AS list` in each select clause, and this tells SQL to name the column `list`.

<br>

### The Intersect Operator

The `INTERSECT` operator takes away the necessity to specify a joint relation. It automatically knows that each select statement in the query is for the same movie.

If we wanted to create a query that searched for movies that were created before the year 2000, and had a `mID` of less than 105, we could use the `INTERSECT` operator.

Our query would look like this:

```sql
SELECT Title FROM Movie WHERE Year < 2000
INTERSECT
SELECT Title FROM Movie WHERE mID < 105;
```

We would then get a table in return with all the movies that were created before the year 2000, and had a `mID` less than 105.

The resulting movies would be: *Top Gun*, *Titanic*, and *The Lion King*.

<br>

### The Except Operator

The `EXCEPT` operator does exactly the opposite of the `INTERSECT` operator.

Lets create a query that looks for movies that were created before the year 2000, but **do not** have a `mID` less than 105.

Our query would look like this:

```sql
SELECT Title FROM Movie WHERE Year < 2000
EXCEPT
SELECT Title FROM Movie WHERE mID < 105;
```

In this case, the `EXCEPT` operator tells SQL to look for movies that were made before the year 2000, and then take away the movies with a `mID` less than 105. This then leaves us with a result of movies which were created before 2000, but have a `mID` greater than or equal to 105.

The resulting movie would be: *The Godfather*.
