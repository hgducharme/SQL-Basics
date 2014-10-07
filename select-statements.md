# Basic Select Statements

<br>
<br>

### The Three Clauses:

The select statement has three clauses: the `FROM` clause, the `WHERE` clause, and the `SELECT` clause. The basic concept is that the `FROM` clause identifies the relation that you want to query over, the condition is used to combine the relations and filter the relations, and the `SELECT` tells you what to return.

The syntax looks like this:

```sql
SELECT A1, A2, . . . , A(n)
FROM R1, R2, . . . , R(m)
WHERE condition
```

Since relational query languages are compositional, when you run a query over relations, you get a relation as a result. Thus, the result of the above select statement is a relation, but it doesn’t have a name. The schema of that relation is the set of attributes that are returned.

<br>

### A Basic Query:

Assume we have a database with three tables:
1. A `MOVIE` table that has four columns labeled `mID` (movie ID), `Title`, `Year`, and `Director`.

2. A `Reviewer` table that has two columns labeled `rID` (reviewer ID), and `Name`.

3. Lastly, a `Review` table that has four columns labeled `rID`, `mID`, `Star`, and `ratingDate`.

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

<br>

### Combing Two Relations:

Now let’s create a query that combines two relations, such as finding movie titles, mID's and the rating that the movie recieved. We’re now involving the `Movie` table, and the `Review` table.

Combining relations looks like this:

```sql
SELECT mID, Title, Rating
FROM Movie, Review
WHERE  Movie.mID = Review.mID;
```

The condition above is called a **join** condition and is saying that we want to combine movies with review statistics that have the same `mID`. We would then get a table as a result with three columns labeled `mID`, `Title`, and `Rating`. It would then display all the movies with their `mID` and their `Rating`.

<br>

### Combing Two Relations w/ a Condition:

The next query is going to find the `Title`, `mID` and `Rating` of movies that were created before the year 2000, and `Rating` is greater than 2.

It would look like this:

```sql
SELECT mID, Title, Rating
FROM Movie, Review
WHERE Movie.mID = Review.mID
    and Rating > 2 and Year < 2000;
```

So in this case, we are looking for `mID`, `Title`, and `Rating`. We are looking inside the `Movie` and `Review` tables, and we have a join condition making sure that the query knows the `mID` in the `Movie` table is the same `mID` in the `Review` table. We are filtering the results based on the year the movie was created, and the rating it recieved. We would then get a table with the results of the query. The results would include all movies that were created before the year 2000, with a rating greater than 2.

<br>

### Combining Three Relations:

This time we are going to combine all three relations, and we’re going to get a table with the results of all the student’s student IDs, names, college, major, GPA, and decision.

     It would look like this:

     SELECT Student.sID, sName, gpa, Application.cName, decision
     FROM Student, Application, College
     WHERE Student.sID = Application.sID and Application.cName = College.cName;

Notice how in the SELECT and WHERE statements, we specify which table we want to pull some of the attributes out of. Since there is more than one table with sID and cName, we need to specify which table we want to pull it out of. It doesn’t really matter, but if we don’t we will get an error because it confuses the computer.


### Sorting Table Results:

SQL by default does not order table results in any particular order. However, if we specify a specific order that we want, we can get results sorted by a specific attribute, or set of attributes. Say we want to sort all of our students by descending GPA. In order to do this, we need to add an additional clause called the ORDER BY clause. If we want to get a descending order, we write what we want to search for and then use the keyword DESC.

     It would look like this:

     SELECT Student.sID, sName, gpa, Application.cName, decision
     FROM Student, Application, College
     WHERE Student.sID = Application.sID and Application.cName = College.cName
     ORDER BY gpa DESC;

If we wanted to have it sort by additional attributes, we would just put a comma after DESC, and add another attribute. However, SQL defaults to ascending order, so you need to specify what you prefer for any additional attributes that you add.


### Doing Arithmetic within Select Statements:

While doing a SELECT statement, SQL allows for doing arithmetic operations. Say we want to find all the student’s attributes, but add it to a scaled GPA. Where we are going to boost their GPA if they are from a big high school, and reduce it if they are from a small one.

     The query would look like this:

     SELECT Student.sID, sName, gpa, sizeHS, gpa*(sizeHS/1000)
     FROM Student;

We would then get a table with all of the above attributes, and then an additional column that shows the student’s GPA’s after being calculated and scaled according to the size of their high school. However, we will get a column labeled “ gpa*(sizeHS/1000) ”, and say that we wan’t to change it to a particular label.

     We would just use the AS clause like so:

     SELECT Student.sID, sName, gpa, sizeHS, gpa*(sizeHS/1000) AS scaledGPA
     FROM Student;

     # This would then give us a column labeled — scaledGPA
