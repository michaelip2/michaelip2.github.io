---
layout: post
title: "Rhyme Dictionary"
date: 2017-03-20
---

I was initially interested in writing poetry and wanted to create a query which would return a list of words with matching phoneme or similar rhyming words. 

For Example: Words rhyming with 'love'  
<pre><code>
Word               Arpabet  

COLGROVE           K OW1 L G R AH0 V 
DOVE               D AH1 V 
FOXGLOVE           F AA1 K S G L AH2 V 
FREELOVE           F R IY1 L AH2 V 
GLOVE              G L AH1 V 
</code></pre>
The numbers above denote the lexical stress marker and are as follows: 
<pre><code>
0    — No stress 
1    — Primary stress 
2    — Secondary stress 
</code></pre>

<p>The query below sources the text file &ldquo;cmudict-0.7b comma.txt&rdquo; from the <a href="http://www.speech.cs.cmu.edu/cgi-bin/cmudict">Carnegie Mellon University Pronouncing Dictionary</a>&nbsp;and parses the data into table 'cmudict' and more specifically into the 'word' field for the list of words and the 'arpabet' field for the phonetic pronunciation of the word.&nbsp;</p>

The txt file used in this query can be downloaded <a href="https://github.com/michaelip2/michaelip2.github.io/blob/master/import_tbl_files/cmudict-0.7b%20comma.txt">here</a>:

<pre><code>
CREATE TABLE cmudict  ( 
word VARCHAR(255), 
arpabet VARCHAR(255)
);

BULK INSERT cmudict FROM 'C:/Users/Michael/Desktop/cmudict-0.7b comma.txt' 
With (FIELDTERMINATOR = ',', ROWTERMINATOR = '0x0a');
</code></pre>

The multiple iterations of replace in the where clause is to remove all the lexical stress markers (0,1,2) so if we were to try to find a word that would rhyme with ‘love’, which has the arpabet of 'AH0 V' then the list of words with the arpabet containing 'AH1 V' or 'AH2 V' would be returned because of the similar phonetic or matching row values.
<pre><code>
CREATE PROCEDURE rhymingDictionary
@word_input nvarchar(50)
AS
SELECT word, arpabet
    FROM cmudict 
	WHERE REPLACE(REPLACE(REPLACE(reverse(reverse(right(arpabet, 5))),'0',''),'1',''),'2','') 
	LIKE CONCAT('%',  
	(SELECT REPLACE(REPLACE(REPLACE(reverse(reverse(right(arpabet, 5))),'0',''),'1',''),'2','') 
	FROM cmudict WHERE word = @word_input));

exec rhymingDictionary @word_input = 'love' 
</code></pre>
The output produces the following: 
<p><img src="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/images/2017-03-22%2020_07_47-rhymefinder%20poem.sql%20-%20DESKTOP-BB2A4CL_SQLEXPRESS.cmudict_database%20(DESKTOP-BB2A.png" alt="" width="312" height="1161" /></p>
