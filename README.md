# SQL_practice_repo
# Module 5 – SQL Query Assignment

This repository contains the SQL queries and results for Module 5 assignment on formulating query expressions using the SQL query language.
Documents to Submit
Document 1: SPOOL Log file (Mod5LastnameFirstname.txt)

This file demonstrates the work done for the assignment. It includes the creation of the Wizarding Schema and the queries in the SPOOL log file. If queries were done in multiple sessions, multiple SPOOL files can be combined into one before submitting. Please use Wordpad instead of Notepad for better formatting.
Document 2: SQL Queries and Results (Mod5LastnameFirstnameSQL.PDF)

This document contains the cleaned-up version of the queries and their corresponding results. Please copy and paste the queries and results tables into one document and save it as a PDF. Make sure to include the questions numbered and in order.

# Queries

1: Find the first and last names of all the wizards

    SQL> --ANSWER 1: 
    SQL> SELECT W.wFname, W.wLname
     2 FROM WIZARD W;
    WFNAME                         WLNAME 
    ------------------------------ ------------------------------ 
    Harry                          Potter 
    Ron                            Weasley 
    Draco                          Malfoy 
    Hermione                       Granger 
    Ginny                          Weasley 
    Luna                           Lovegood 
    Cedric                         Diggory 
    Tom                            Riddle 

2: Find the names of Teachers who have taught the class ' Defense Against Dark Arts '

    SQL> --ANSWER 2:
    SQL> SELECT T.Teacher
     2 FROM TAKES T
     3 JOIN CLASS C ON T.cID = C.cID
     4 AND C.cName = 'Defense Against Dark Arts';
    
    TEACHER 
    ------------------------------ 
    Quirrell 
    Moody 
    Lupin
    
3. Find all the names of the classes Harry Potter took

        SQL> --ANSWER 3: 
        SQL> SELECT C.cName
         2 FROM CLASS C
         3 JOIN TAKES T ON T.cID = C.cID
         4 JOIN WIZARD W ON W.wID = T.wID
         5 WHERE W.wFname = 'Harry' AND W.wLname = 'Potter';
        CNAME 
        ---------------------------------------- 
        Potions 
        Defense Against Dark Arts 

4. Find the first name of Wizards who have taken at least one class
SQL> --ANSWER 4:
SQL> 
SQL> SELECT DISTINCT W.wFname
 2 FROM WIZARD W
 3 JOIN TAKES T ON T.wID = W.wID;
WFNAME 
------------------------------ 
Ginny 
Draco 
Harry 
Ron 
Hermione 
5. Find the last names of any wizards who have taken at least one class, but did not take 
'Potions'
SQL> --ANSWER 5:
SQL> 
SQL> SELECT W.wLname
 2 FROM WIZARD W
 3 JOIN TAKES T ON T.wID = W.wID
 4 MINUS
 5 SELECT W.wLname
 6 FROM WIZARD W
 7 JOIN TAKES T ON T.wID = W.wID
 8 JOIN CLASS C on T.cID = C.cID AND C.cName ='Potions';
no rows selected
6. Find the first and last names of any wizards who have taken 
at least one class, but did not take 'Muggle Studies'
SQL> --ANSWER 6:
SQL> 
SQL> SELECT wFname,wLname
2 FROM WIZARD
3 WHERE wID IN ((SELECT wID
4 FROM TAKES)
5 MINUS
6 (SELECT wID
7 FROM TAKES
8 JOIN CLASS ON TAKES.cID = CLASS.cID
9 WHERE cName = 'Muggle Studies'));
WFNAME WLNAME 
------------------------------ ------------------------------ 
Draco Malfoy 
Ginny Weasley 
Harry Potter 
Ron Weasley 
7. Find the first names of Wizards who do not belong to House 
'Gryffindor'
SQL> --ANSWER 7
SQL> 
SQL> SELECT W.wFname
 2 FROM WIZARD W
 3 JOIN BELONGS B ON B.wID = W.wID AND B.hNAME <> 'Gryffindor';
WFNAME 
------------------------------ 
Draco 
Luna 
Cedric 
Tom 
8. Find the first names of wizards who have taken both 'Potions' 
and 'Transfiguration'
SQL> --ANSWER 8
SQL> 
SQL> SELECT W.wFname
 2 FROM WIZARD W
 3 JOIN TAKES T ON T.wID = W.wID
 4 JOIN CLASS C on T.cID = C.cID AND C.cName ='Potions'
 5 INTERSECT
 6 SELECT W.wFname
 7 FROM WIZARD W
 8 JOIN TAKES T ON T.wID = W.wID
 9 JOIN CLASS C on T.cID = C.cID AND C.cName ='Transfiguration';
WFNAME 
------------------------------ 
Ginny 
Hermione 
9. Find the first and last names of wizards who have taken all 7 
classes
SQL> --ANSWER 9
SQL> 
SQL> SELECT W.wFname, W.wLname
 2 FROM WIZARD W
 3 JOIN TAKES T ON T.wID = W.wID
 4 JOIN CLASS C ON T.cID = C.cID
 5 GROUP BY W.wID, W.wFname, W.wLname
 6 HAVING COUNT(DISTINCT C.cID) = (SELECT COUNT(DISTINCT cID) FROM CLASS);
WFNAME WLNAME 
------------------------------ ------------------------------ 
Hermione Granger 
 
10. Find the average points and the highest points from BELONGS 
table
SQL> --ANSWER 10:
SQL> 
SQL> SELECT AVG(B.Points), MAX(B.Points)
 2 FROM BELONGS B;
AVG(B.POINTS) MAX(B.POINTS) 
------------- -------------------- 
 459.75 488 
11. Find the earliest year in the Belongs table and the first 
and last name of the Wizard
SQL> --ANSWER 11:
SQL> 
SQL> SELECT W.wFname, W.wLname, B.hYear
 2 FROM BELONGS B
 3 JOIN WIZARD W ON B.wID = W.wID
 4 WHERE B.hYear =(SELECT MIN(B2.hYear) FROM BELONGS B2);
WFNAME WLNAME HYEAR 
------------------------------ ------------------------------ -------- 
Tom Riddle 1943 
12. Find the wizard first name, class name and teacher for any 
wizards who took classes in 1993
SQL> --ANSWER 12:
SQL> 
SQL> SELECT W.wFname, C.cName, T.Teacher
 2 FROM WIZARD W
 3 JOIN TAKES T ON T.wID = W.wID
 4 JOIN CLASS C ON T.cID = C.cID
 5 WHERE T.cYear = 1993;
WFNAME CNAME TEACHER 
------------------------------ ---------------------------------------- ---------
--------------------- 
Draco Divination Trelawney 
Hermione Divination Trelawney 
Harry Potions Snape 
Ron Potions Snape 
Draco Potions Snape 
Hermione Potions Snape 
Hermione Muggle Studies Burbage 
Hermione Transfiguration McGonagall 
Hermione Care of Magical Creatures Hagrid 
Hermione Herbology Sprout 
12. Find the wizard first name, class name and teacher for any wizards who took classes in 
1993
SQL> --ANSWER 12:
SQL> SELECT C.cName, T.Teacher
 2 FROM CLASS C
 3 JOIN TAKES T ON C.cID = T.cID
 4 WHERE C.cNAME LIKE 'D%';
CNAME TEACHER 
---------------------------------------- ------------------------------ 
Divination Trelawney 
Divination Trelawney 
Defense Against Dark Arts Quirrell 
Defense Against Dark Arts Moody 
Defense Against Dark Arts Lupin 

    
![image](https://github.com/mSakib20/SQL_practice_repo/assets/97981916/a64ba6ce-792e-4178-9f25-0a4ecbf20763)
