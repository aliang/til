You can calculate the Pearson correlation coefficient
in pure SQL pretty easily.

https://www.vanheusden.com/misc/pearson.php

Here it is in case of link rot.

## Input data

For this query I created a relative simple table with only 3 columns: one for the user, one for the movie he/she saw and one with the rating as a floating point value:
+------+-----------------------+--------+
| user | movie                 | rating |
+------+-----------------------+--------+
| John | Donny Darko           |      3 | 
| John | Existenze             |      5 | 
| John | Sex and the city      |      4 | 
| John | The Hours             |    2.5 | 
| John | The Matrix            |      0 | 
| Tim  | Blade                 |      4 | 
| Tim  | Donny Darko           |      2 | 
| Tim  | Starwars 4            |      5 | 
| Tim  | The Matrix            |      5 | 
| Toby | Alien versus Predator |      3 | 
| Toby | Donny Darko           |      5 | 
| Toby | The Hours             |    4.5 | 
| Toby | The Matrix            |    2.5 | 
+------+-----------------------+--------+

## The query explained

The query itself is relatively simple: one query goes through all data and calculates the sum/sum square/psum for each user/user combination:
+-------+-------+------+------+--------+--------+-------+---+
| user1 | user2 | sum1 | sum2 | sum1sq | sum2sq | psum  | n |
+-------+-------+------+------+--------+--------+-------+---+
| Tim   | John  |    7 |    3 |     29 |      9 |     6 | 2 | 
| Toby  | John  |   12 |  5.5 |   51.5 |  15.25 | 26.25 | 3 | 
| Toby  | Tim   |  7.5 |    7 |  31.25 |     29 |  22.5 | 2 | 
+-------+-------+------+------+--------+--------+-------+---+
The query around that then picks the results and calculates the Pearson coefficient from that:
+-------+-------+------------------+---+
| user1 | user2 | r                | n |
+-------+-------+------------------+---+
| Toby  | John  | 0.99942379712877 | 3 | 
| Tim   | John  |               -1 | 2 | 
| Toby  | Tim   |               -1 | 2 | 
+-------+-------+------------------+---+

## The query

SELECT  
        user1, user2,
        ((psum - (sum1 * sum2 / n)) / sqrt((sum1sq - pow(sum1, 2.0) / n) * (sum2sq - pow(sum2, 2.0) / n))) AS r,
        n
FROM
        (SELECT 
                n1.user AS user1,
                n2.user AS user2,
                SUM(n1.rating) AS sum1,
                SUM(n2.rating) AS sum2,
                SUM(n1.rating * n1.rating) AS sum1sq,
                SUM(n2.rating * n2.rating) AS sum2sq,
                SUM(n1.rating * n2.rating) AS psum,
                COUNT(*) AS n
        FROM
                testdata AS n1
	LEFT JOIN
		testdata AS n2
	ON
		n1.movie = n2.movie
        WHERE   
                n1.user > n2.user
	GROUP BY
		n1.user, n2.user) AS step1
ORDER BY
        r DESC,
        n DESC

