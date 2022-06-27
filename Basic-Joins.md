# Basic Join Challenges

## Given the CITY and COUNTRY tables, solve the challenges.  
**Note:** CITY.CountryCode and COUNTRY.Code are matching key columns. <br/>
**Input Format:** <br/>
The CITY and COUNTRY tables are described as follows:<br/>
![City Table](https://s3.amazonaws.com/hr-challenge-images/8137/1449729804-f21d187d0f-CITY.jpg) <br/><br/>
![Country Table](https://s3.amazonaws.com/hr-challenge-images/8342/1449769013-e54ce90480-Country.jpg) <br/><br/>

1. Query the names of all cities where the CONTINENT is 'Africa'. <br/>
```
SELECT City.Name
FROM City
INNER JOIN Country
ON City.CountryCode = Country.Code
WHERE Country.Continent='Africa'
```
2. Query the sum of the populations of all cities where the CONTINENT is 'Asia'.
```
SELECT SUM(City.Population)
FROM City
INNER JOIN Country
ON City.CountryCode=Country.Code
WHERE Country.Continent ='Asia'
```
3. Query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.
```
/*Rounding up: CEIL(), Rounding down: FLOOR(); Proper Approximation: ROUND()*/

SELECT Country.Continent, FLOOR(AVG(City.Population))
FROM City
INNER JOIN Country ON City.CountryCode=Country.Code
GROUP BY Country.Continent
```
## You are given two tables: Students and Grades. Students contains three columns ID, Name and Marks.
![Students](https://s3.amazonaws.com/hr-challenge-images/12891/1443818137-69b76d805c-2.png)<br/><br/>

Grades Contain the following data: <br/><br/>
![Grades](https://s3.amazonaws.com/hr-challenge-images/12891/1443818137-69b76d805c-2.png)<br/><br/>

1. Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order. <br/>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Write a query to help Eve.
```
SELECT  
CASE
 WHEN Grades.Grade<8 THEN NULL
 WHEN Grades.Grade>=8 THEN Students.Name
END, Grades.Grade, Students.Marks
FROM Students
CROSS JOIN Grades
WHERE Students.Marks>=Grades.Min_Mark AND Students.Marks<=Max_Mark
ORDER BY Grades.Grade DESC, Students.Name
```

## Top Competitors - Creating a Leaderboard
**Input Format:** <br/>
The following tables contain contest data: <br/>

**Hackers:** <br/>
- The hacker_id is the id of the hacker, and name is the name of the hacker.<br/>
![Hacker Table](https://s3.amazonaws.com/hr-challenge-images/19504/1458526776-67667350b4-ScreenShot2016-03-21at7.45.59AM.png)<br/>

**Difficulty:**<br/>
- The difficult_level is the level of difficulty of the challenge, and score is the score of the challenge for the difficulty level<br/>
![Difficulty Table](https://s3.amazonaws.com/hr-challenge-images/19504/1458526915-57eb75d9a2-ScreenShot2016-03-21at7.46.09AM.png)<br/>

**Challenges:**<br/>
- The challenge_id is the id of the challenge, the hacker_id is the id of the hacker who created the challenge, and difficulty_level is the level of difficulty of the challenge.<br/>
![Challenges Table](https://s3.amazonaws.com/hr-challenge-images/19504/1458527032-f9ca650442-ScreenShot2016-03-21at7.46.17AM.png)<br/>

**Submissions:**<br/>
- The submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission, challenge_id is the id of the challenge that the submission belongs to, and score is the score of submission <br/>
![Submissions Table](https://s3.amazonaws.com/hr-challenge-images/19504/1458527077-298f8e922a-ScreenShot2016-03-21at7.46.29AM.png)<br/>

### Question:<br/>
Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id. <br/>

```
/*
First Join: Gives the number of submissions for each challenge with the hacker_id and the score
Seond Join: Gives the Maximum Score that is possible for a submission based on the difficulty level
Third Join: Gives the Hacker Name and Hacker ID to map the resultant table
Having condition filters out the number of submissions by each hacker that has gotten a full score. 
*/

SELECT h.hacker_id, h.name 
FROM Challenges c
INNER JOIN Submissions s
ON c.challenge_id=s.challenge_id
INNER JOIN Difficulty d
ON d.difficulty_level=c.difficulty_level
INNER JOIN Hackers h
ON h.hacker_id=s.hacker_id
WHERE d.score=s.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(*)>1
ORDER BY COUNT(*) DESC, hacker_id
```
