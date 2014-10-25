# Delete Commands

<br>
<br>

Let's create a query that finds all users who rated more than 2 movies. Let's assume that users are only allowed to rate 2 movies, and no more.

Our query would look like this:

```sql
SELECT uID, COUNT(DISTINCT Rating)
FROM Review
GROUP BY uID
HAVING COUNT(DISTINCT Rating) > 2;
```

This query says to go into the `Review` relation, then form groups or paritions by each user ID. This allows us to evaluate the set of ratings for each individual `User`. We're going to count how many `DISTINCT` `Rating`s there are in each group (or for each user). Then it's going to check to see if that number is greater than two, and if it is, it's going to return the `uID` of that user, and also the number of `Rating`s that user has given.

If we were to run the query we would return the user: "Darrel Sherman", who has a `uID` of 207. He is the only person to have gone over his rating limit, and has rated 3 movies. Since he has broken the hypothetical contract, we are going to have to delete him from the database.

Our query would look like this:

```sql
DELETE FROM Review
WHERE uID in
(SELECT uID, COUNT(DISTINCT Rating)
FROM Review
GROUP BY uID
HAVING COUNT(DISTINCT Rating) > 2);
```

So all we did was add the `DELETE FROM <table> WHERE <condition>` statement, and turned our previous query into a subquery in the `WHERE` clause. The query is saying to return all the users who have rated more than 2 movies (just like before), then `DELETE` those users from the `Review` relation.

If we were to run the query, we would delete all instances of the `uID` 207, or Darrel Sherman, from the `Review` relation. However, we would NOT delete him from the `User` relation. In order to do that we would just have to change the relation to `DELETE FROM`.

Like so:

```sql
DELETE FROM User
WHERE uID in
(SELECT uID, COUNT(DISTINCT Rating)
FROM Review
GROUP BY uID
HAVING COUNT(DISTINCT Rating) > 2);
```

We did the exact same query before, except change the table from `Review` to `User`. Now some SQL database systems don't allow you to delete data if the subquery includes the same relation that you are deleting from, so it can get a little tricky depending on the database you are using.
