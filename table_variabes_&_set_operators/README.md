# Table Variabes & Set Operators

<br>

### What are they?

Take a look at the code block below:

```sql
SELECT A1, A2, . . . , A(n)
FROM R1, R2, . . . , R(m)
WHERE condition;
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

In this next query, we want to find all movies that have the exact same rating. In order to do that, we need to have two instances of `Movie`. We will call the first instance `M1` and the second instance `M2`.

Our query would look like this:

```sql
SELECT M1.mID, M1.Title, M1.Rating, M2.mID, M2.Title, M2.Rating
FROM Movie M1, Movie M2
Where M1.Rating = M2.Rating and M1.mID < M2.mID;
```

We first added the two table variables: `M1` and `M2` to seperate each instance. We are looking for the `mID`'s, `Title`'s, and `Rating`'s of each movie. We then place a condition in the `WHERE` clause to specify that we want to output movies with the same rating. Lastly, we specify a second clause: `and M1.mID < M2.mID;`. This is telling the computer that the `M1.mID` is different from `M2.mID`, or that movie 1 needs to be different from movie 2. If we didn't specify this clause, we would get an output of movies that equal themselves, because movie 1 would have the same `Rating` as itself, thus satisfying the condition.

<br>

### Union Set Operator
