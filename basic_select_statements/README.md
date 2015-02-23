# Basic Select Statements

<br>
<br>

### The Three Clauses:

The select statement has three clauses: the `FROM` clause, the `WHERE` clause, and the `SELECT` clause. The basic concept is that the `FROM` clause identifies the relation that you want to query over, the 'WHERE' condition is used to combine the relations and filter the relations, and the `SELECT` tells you what to return.

The syntax looks like this:

```sql
SELECT A1, A2, . . . , A(n)
FROM R1, R2, . . . , R(m)
WHERE <condition>;
```

Since relational query languages are compositional, when you run a query over relations, you get a relation as a result. Thus, the result of the above select statement is a relation, but it doesn’t have a name. The schema of that relation is the set of attributes that are returned.

<br>

### A Basic Query:

Assume we have a database with three tables:
1. A `MOVIE` table that has four columns labeled `mID` (movie ID), `Title`, `Year`, and `Director`.

2. A `User` table that has two columns labeled `uID` (User ID), and `Name`.

3. Lastly, a `Review` table that has four columns labeled `uID`, `mID`, `Star`, and `ratingDate`.

<br>

*(This sample database can be viewed in the Introduction chapter of this book.)*

---

<br>

We’re going to do a basic query that finds the `Title`, and `Year` of movies that were created after the year 2000. The `SELECT` tells us what we want to get out of the query, the `FROM` tells us our table name, and the `WHERE` gives us the filtering condition.

The query would look like this:

```sql
SELECT Title, Year
FROM Movie
WHERE Year > 2000;
```

We would then get a table back that would have two columns labeled `Title`, and `Year`. It would then display all the movies that were created after the year 2000.

The resulting movies would include: *Gravity*, *Harry Potter*, *Cast Away*, and *Spiderman*.

<br>

### Combing Two Relations:

Now let’s create a query that combines two relations, such as finding movie titles, mID's and the rating that the movie recieved. We’re now involving the `Movie` table, and the `Review` table.

Combining relations looks like this:

```sql
SELECT Movie.mID, Title, Rating
FROM Movie, Review
WHERE  Movie.mID = Review.mID;
```

The condition above is called a **join** condition and is saying that we want to combine movies with review statistics that have the same `mID`. We would then get a table as a result with three columns labeled `mID`, `Title`, and `Rating`. It would then display all the movies with their `mID` and their `Rating`.

<br>

### Combing Two Relations w/ a Condition:

The next query is going to find the `Title`, `mID` and `Rating` of movies that were created before the year 2000, and `Rating` is greater than 2.

It would look like this:

```sql
SELECT Movie.mID, Title, Rating
FROM Movie, Review
WHERE Movie.mID = Review.mID
    and Rating > 2 and Year < 2000;
```

So in this case, we are looking for `mID`, `Title`, and `Rating`. We are looking inside the `Movie` and `Review` tables, and we have a join condition making sure that the query knows the `mID` in the `Movie` table is the same `mID` in the `Review` table. We are filtering the results based on the year the movie was created, and the rating it recieved. We would then get a table with the results of the query. The results would include all movies that were created before the year 2000, with a rating greater than 2.

The resulting movies would be: *Top Gun*, *Titanic*, *The Lion King*, and *The Godfather*.

<br>

### Combining Three Relations:

This time we are going to combine all three relations, and we’re going to get a table with the results of every `mID`, `Title`, `Year`, User `Name`, and `Rating`.

It would look like this:

```sql
SELECT Movie.mID, Title, Year, Name, User.uID, Rating
FROM Movie, User, Review
WHERE Movie.mID = Review.mID and User.uID = Review.uID;
```

Notice how in the `SELECT` and `WHERE` statements, we specify which table we want to pull some of the attributes out of. Since there is more than one table with `mID` and `uID`, we need to specify which table we want to pull it out of. It doesn’t matter which table we specify, but if we don’t specify we will get an error because the computer doesn't know which table to pull from.

<br>

### Sorting Table Results:

SQL by default does not order table results in any particular order. However, if we specify a specific order that we want, we can get results sorted by a specific attribute, or set of attributes. Say we want to sort all of our movies by descending `Rating`. In order to do this, we need to add an additional clause called the `ORDER BY` clause. If we want to get a descending order, we write what we want to search for and then use the keyword `DESC`.

It would look like this:

```sql
SELECT  Movie.mID, Title, Year, Name, User.uID, Rating
FROM Movie, User, Review
WHERE Movie.mID = Review.mID and User.uID = Review.uID
    ORDER BY Rating DESC;
```

If we wanted to have it sort by additional attributes, we would just put a comma after `DESC`, and add another attribute. However, SQL defaults to ascending order, so you need to specify which way you prefer for any additional attributes that you add.

<br>

### Doing Arithmetic within Select Statements:

While doing a `SELECT` statement, SQL allows for doing arithmetic operations. Say we want to find all the movie's attributes, but add to it a scaled `Rating`. Where we are going to scale the rating by 10 to get ratings that are in the teens.

The query would look like this:

```sql
SELECT Movie.mID, Title, Rating, Director, Rating + 10
FROM Movie, Review;
```

We would then get a table with all of the above attributes, and then an additional column that shows the movie's `Rating` after being scaled by 10. However, we will get a column labeled `Rating + 10`, but we want to change it to a different particular label.

We would just use the AS clause like so:

```sql
SELECT Movie.mID, Title, Rating, Director, Rating + 10 AS ScaledRating
FROM Movie, Review;
```

We would then have a column labeled `ScaledRating` instead of `Rating + 10`.
