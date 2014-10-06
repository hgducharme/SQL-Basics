
# The Select Statement

The Three Clauses:     The select statement has three clauses: the FROM clause, the WHERE clause, and the SELECT clause. The basic concept is that the FROM clause identifies the relation that you want to query over, the condition is used to combine the relations and filter the relations, and the SELECT tells you what to return.

     The syntax looks like this:

     SELECT A1, A2, . . . , An
     FROM R1, R2, . . . , Rm
     WHERE condition

Since relational query languages are compositional, when you run a query over relations, you get a relation as a result. Thus, the result of the above select statement is a relation, but it doesn’t have a name. The schema of that relation is the set of attributes that are returned.


The Basics:     Assume we have a database with two tables: we have a COLLEGE table that has columns labeled cName (college name), state, and  enrollment. We also have a table labeled STUDENT that has columns labeled sID (student ID), sName (student name), gpa, and sizeHS. Then the third table labeled APPLICATION, that has columns labeled sID, cName, major, and decision.

     These tables look like so:

                                                                        College
cName
state
enrollment
Stanford
CA
15 000
Berkeley
CA
36 000
MIT
MA
10 000
Cornell
NY
21 000

                                                                        Student
sID
sName
gpa
sizeHS
123
Amy
3.9
1000
234
Bob
3.4
1600
345
Craig
3.5
500
456
Dorris
3.9
1000

                                                                    Application
sID
cName
major
decision
123
Stanford
CS
Y
123
Stanford
EE
N
234
Berkeley
biology
Y
345
MIT
bioengineering
Y


We’re going to do a basic query that finds the ID, name, and GPA of students whose GPA is greater than 3.6. Where the SELECT tells use what we want to get out of the query, the FROM tells us our table name, and the WHERE gives us the filtering condition.

     The query would look like this:

     SELECT sID, sName, GPA
     FROM Student
     WHERE GPA > 3.6;

We would then get a table back that would have three columns labeled sID, SNAME, and gpa. It would then display all the students that meet the criteria.


Combing Two Relations:     Now let’s create a query that combines two relations, such as finding the student’s names and the majors for which they applied. We’re now involving the STUDENT table, and the APPLICATION table.

     Combining relations looks like this:

     SELECT sName, major
     FROM Student, Application
     WHERE Student.sID = Application.sID;

The condition above is called a join condition and is saying that we want combine students with application records that have the same sID. We would then get a table as a result with two columns labeled sID, and major. It would then display all the students and their majors.


Combing Two Relations w/ a Condition:     The next query is going to find the names and GPA of students whose high school class was less than 1000, and who applied for CS at Stanford.

     It would look like this:

     SELECT sName, gpa, decision
     FROM Student, Application
     WHERE Student.sID = Application.sID
          and sizeHS < 1000 and major = ‘CS' and cName = ‘Stanford’;

So in this case, we are looking for SNAME, gpa, and decision. We are looking inside the STUDENT and APPLICATION tables, and we have a join condition making sure that we are matching up the same students in each table. We are filtering the results based on size of high school, their major, and the college they applied to. We would then get a table with the results of the query. The results would include all students who applied to Stanford with a major of CS, and whose high school was less than 1000.


Combining Three Relations:     This time we are going to combine all three relations, and we’re going to get a table with the results of all the student’s student IDs, names, college, major, GPA, and decision.

     It would look like this:

     SELECT Student.sID, sName, gpa, Application.cName, decision
     FROM Student, Application, College
     WHERE Student.sID = Application.sID and Application.cName = College.cName;

Notice how in the SELECT and WHERE statements, we specify which table we want to pull some of the attributes out of. Since there is more than one table with sID and cName, we need to specify which table we want to pull it out of. It doesn’t really matter, but if we don’t we will get an error because it confuses the computer.


Sorting Table Results:     SQL by default does not order table results in any particular order. However, if we specify a specific order that we want, we can get results sorted by a specific attribute, or set of attributes. Say we want to sort all of our students by descending GPA. In order to do this, we need to add an additional clause called the ORDER BY clause. If we want to get a descending order, we write what we want to search for and then use the keyword DESC.

     It would look like this:

     SELECT Student.sID, sName, gpa, Application.cName, decision
     FROM Student, Application, College
     WHERE Student.sID = Application.sID and Application.cName = College.cName
     ORDER BY gpa DESC;

If we wanted to have it sort by additional attributes, we would just put a comma after DESC, and add another attribute. However, SQL defaults to ascending order, so you need to specify what you prefer for any additional attributes that you add.


Doing Arithmetic within Select Statements:     While doing a SELECT statement, SQL allows for doing arithmetic operations. Say we want to find all the student’s attributes, but add it to a scaled GPA. Where we are going to boost their GPA if they are from a big high school, and reduce it if they are from a small one.

     The query would look like this:

     SELECT Student.sID, sName, gpa, sizeHS, gpa*(sizeHS/1000)
     FROM Student;

We would then get a table with all of the above attributes, and then an additional column that shows the student’s GPA’s after being calculated and scaled according to the size of their high school. However, we will get a column labeled “ gpa*(sizeHS/1000) ”, and say that we wan’t to change it to a particular label.

     We would just use the AS clause like so:

     SELECT Student.sID, sName, gpa, sizeHS, gpa*(sizeHS/1000) AS scaledGPA
     FROM Student;

     # This would then give us a column labeled — scaledGPA
