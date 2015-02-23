# SQL Basics

<br>

**What This Book is About**

This book will cover the basics of SQL, and should help introduce beginners to SQL concepts. It is also a good documentation source when forgetting how the basic SQL syntax works.

**Contributing**

This book is open source, so please feel free to visit the github repository to help contribute! If you notice any typos or errors in this book, you can fork the repo and open a pull request with the new and improved changes, or simply just open a new issue. Any contribution is greatly appreciated and it will help make this book better for those reading it after you!

**License**

This book is currently licensed under a Creative Commons license. For more information, read more on [the official Creative Commons website](http://creativecommons.org/licenses/by-nc-sa/4.0/).

<br>
---
<br>

**Here is the example database that will be used throughout the book:**

<br>

**Movies**

| mID | Title         | Year | Director        |
| --- | ------------- | ---- | --------------- |
| 101 | Top Gun       | 1986 | Tony Scott      |
| 102 | Titanic       | 1997 | James Cameron   |
| 103 | The Lion King | 1994 | Rob Minkoff     |
| 104 | Gravity       | 2013 | Alfonso Cuaron  |
| 105 | Harry Potter  | 2001 | `<null>`        |
| 106 | Cast Away     | 2000 | Robert Zemeckis |
| 107 | Spider Man    | 2002 | Sam Raimi       |
| 108 | The Godfather | 1972 | Francis Coppola |

<br>

**User**

| uID | Name             |
| --- | ---------------- |
| 201 | James Dean       |
| 202 | Chris Anderson   |
| 203 | Ashley Burley    |
| 204 | Ralph Truman     |
| 205 | Gordon Maximus   |
| 206 | Sarah Rodgriguez |
| 207 | Darrel Sherman   |
| 208 | Lisa Jackson     |

<br>

**Review**

| uID | mID | Rating | ratingDate |
| --- | --- | ------ | ---------- |
| 201 | 101 | 2      | 2014-03-09 |
| 201 | 101 | 4      | 2014-03-02 |
| 202 | 104 | 4      | `<null>`   |
| 203 | 107 | 2      | 2014-03-24 |
| 204 | 103 | 4      | 2014-03-17 |
| 204 | 104 | 2      | 2014-03-13 |
| 205 | 108 | 3      | 2014-03-24 |
| 206 | 102 | 3      | 2014-03-02 |
| 207 | 104 | 5      | `<null>`   |
| 207 | 106 | 4      | 2014-03-07 |
| 207 | 102 | 5      | 2014-03-26 |
| 208 | 105 | 2      | 2014-03-13 |
