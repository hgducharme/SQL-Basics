# Update Commands

<br>
<br>

Let's find all users who do not currently have a `ratingDate` assigned to one of their rating's, and let's assign it a value.

Our query will look like this:

```sql
SELECT *
FROM User
WHERE uID in (SELECT uID FROM Review WHERE ISNULL ratingDate);
```

The query is going to look in the `User` relation and for each individual user, it will check for their `uID` in the `Review` table, and if the `ratingDate` returns true for `ISNULL`, then it will retrn that user in the end result.

If we ran the query, our results would include the users "Darrel Sherman", and "Chris Anderson", with a `uID` of 207 and 202 respectively.

Well no we want to update the `ratingDate` for these two individuals in the `Review` table. We would have to edit our query a little bit and have it look like this:

```sql
UPDATE Review
SET ratingDate = '2014'
WHERE uID in (SELECT uID FROM Review WHERE ISNULL ratingDate);
```

We added the `UPDATE` statement, and we're telling SQL to `UPDATE` the `Review` table. Find all the users with a `uID` that satisfy the subquery in the `WHERE` clause, and then take the `ratingDate` for those users and give it a value of `'2014'`. We'll assume we don't know the exact date that they rated the movie, but we know it was in 2014.
