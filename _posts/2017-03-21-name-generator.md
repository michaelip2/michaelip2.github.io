---
layout: post
title: "Random Monster Name Generator"
date: 2017-03-21
---

This query generates a random monster name for when you feel you are lacking creativity to think of one. 

I began compiling a list of random adjectives and nouns into a text file and bulk inserted the words into my monster_names table. 
The cross join is used to generate a new name from all the possible rows from the adjective column with the nouns column. The ORDER BY newid() ensures random ordering and reduces the chance of the same adjective or noun appearing twice.
 
The compiled list I made can be found <a href="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/_files/monster%20adjectives%20and%20nouns.txt">here</a>.

<pre><code>
CREATE DATABASE monster_name_db;

Create table monster_names(
adjectives varchar(50),
nouns varchar(50));

BULK INSERT monster_names FROM 'C:/Users/Michael/Desktop/monster adjectives and nouns.txt' 
With (FIELDTERMINATOR = ' ', ROWTERMINATOR = '0x0a');

SELECT TOP 5 CONCAT(a.adjectives, ' ', b.nouns) as 'Name Generator'
FROM monster_names a 
CROSS JOIN monster_names b group by a.adjectives, b.nouns
ORDER BY newid();
</code></pre>

Output:
<p><img src="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/images/2017-04-01%2004_47_00-monster%20generator.sql%20-%20DESKTOP-BB2A4CL_SQLEXPRESS.master%20(DESKTOP-BB2A4CL_Micha.png"/></p>
