# The 'Join' Family Operators

<br>
<br>

### The Basics

In our select statements, we seperate tables in the `FROM` clause by commas, and that is implicitly a cross product of those tables.

Like so:

```sql
SELECT A1, A2, . . . , A(n)
FROM R1, R2, . . . , R(m)
WHERE <condition>;
```

However, there is a way to explicitly join two or more tables using one of the `JOIN` operators. There are a few different types, and they are listed below:

1. The `INNER JOIN` on *condition*
    * This kind of join operator takes the cross product of the tables and then applies a condition, and only taking the cross product elements that satisfy the condition given. It then eliminates all duplicate columns that are created.
2. The `NATURAL JOIN`
    * The `NATURAL JOIN` operator equates all columns with the same name in the tables that are being joined. It requires the values in the columns to be the same in order to keep the elements in the cross product. This type of join also eliminates any duplicate columns that are created.
3. The `INNER JOIN USING(attrs)`
    * This again is an `INNER JOIN`, however this type of join takes a special clause called `USING` and listing attributes. This is sort of like the `NATURAL JOIN`, except you specifically state the attributes that you want to be equated.
4. The `OUTER JOIN`
    * There are multiple forms of this kind of join operator. There is the `LEFT OUTER JOIN`, the `RIGHT OUTER JOIN`, and the `FULL OUTER JOIN`. These joins combine elements similar to the `INNER JOIN`, except when elements don't match the `INNER JOIN` condition, they're still added to the result and padded with `<null>` values.


All of these join operators don't add any specific power to SQL, they can all be described using different constructs, however they can be very helpful when creating queries. Especially the `OUTER JOIN`, for it is very difficult to express without the `OUTER JOIN` itself.
