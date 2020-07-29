<!-- The Election Board suspects there's some cheating going on! 
Help them figure out what is going on and if anyone is to blame.

(a) Which Congress member received the most votes?
<!-- List his/her name and a count of their votes. -->
SELECT name, COUNT(politician_id)
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY politician_id
ORDER BY COUNT(politician_id) DESC LIMIT 1;


<!-- (b) Who were the people that voted for that politician?
<!-- List their names, gender and party. -->
SELECT first_name||' '||last_name, gender, voters.party
FROM votes JOIN voters ON voters.id = voter_id JOIN congress_members ON congress_members.id = votes.politician_id
WHERE congress_members.name = (SELECT name
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY politician_id
ORDER BY COUNT(politician_id) DESC LIMIT 1);


<!-- How many votes were received by Congress members whose communication grade average was less than 9 
(this number can be found in the grade_current field)? List their name, location, grade since 1996, 
and the vote count. -->
SELECT name, location, grade_current, grade_1996, COUNT(politician_id)
FROM congress_members JOIN votes ON congress_members.id = politician_id
WHERE grade_current <9
GROUP BY politician_id
ORDER BY grade_1996;

<!-- What 10 states had the most voters turnout? (Does this correspond to the population of those states?) 
List the people that voted in the top state's elections. (It will be a big list, and you can use the results 
from your first query to help simplify this next query.) -->
SELECT location, COUNT(location) 
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY location
ORDER BY COUNT(location) DESC LIMIT 10;

SELECT location, first_name||' '||last_name
FROM congress_members JOIN votes ON congress_members.id = politician_id JOIN voters ON voters.id = voter_id
WHERE location = 'CA'
LIMIT 10;


<!-- List the people that voted more than 2 times? (It should only be once for their Senator and once for their 
representative!) Ay Caramba! We have some serious ballot stuffing! Report this to the Election Board! -->
SELECT first_name||' '||last_name, COUNT(voter_id)
FROM votes JOIN voters ON voter_id=voters.id
GROUP BY voter_id
HAVING COUNT(voter_id)>2
ORDER BY COUNT(voter_id) DESC, first_name LIMIT 10;

<!-- Did anyone vote for the same politician twice? What was the name of the voter and the politician they voted for? 
Pretty sneaky... -->
SELECT first_name||' '||last_name, name, COUNT(politician_id)
FROM voters JOIN votes ON voters.id= voter_id JOIN congress_members ON congress_members.id = politician_id
GROUP BY politician_id,voter_id
HAVING COUNT(politician_id)>=2 AND COUNT(voter_id)>=2
ORDER BY COUNT(politician_id) DESC


<!-- Paste the text from your trace file into the repo and keep it for future reference on JOIN's and other SQL calls. 
Include the schemas of the 3 tables in the gist. 
Include some on the output of the queries if you're worried you won't remember all this syntax! --> -->

<!-- The Texas vote is close! Add 2 new voters, and fabricate a vote for each of the 2 incumbent senators of Texas. Make up their names and info. -->
INSERT INTO voters
(first_name,last_name,gender,party,created_at,updated_at)
VALUES
('Kesihain','Selvarajoo','male','independant',DATETIME('now'),DATETIME('now'))

INSERT INTO voters
(first_name,last_name,gender,party,created_at,updated_at)
VALUES
('Harriwin','Selvarajoo','male','republican',DATETIME('now'),DATETIME('now'))

SELECT * FROM congress_members WHERE location = 'TX' AND name LIKE 'SEN%'; <!--RESULT: 145,499-->

INSERT INTO votes
(voter_id,politician_id,created_at,updated_at)
VALUES
(5001,145,DATETIME('now'),DATETIME('now'));

INSERT INTO votes
(voter_id,politician_id,created_at,updated_at)
VALUES
(5002,449,DATETIME('now'),DATETIME('now'));
<!-- Insert another politician, "Donald Trump". Add suitable attributes and delete one of the senators from New Jersey. Add Trump there instead. Give Trump all the votes from the deleted politician. -->
INSERT INTO congress_members
(name,party,location,created_at,updated_at)
VALUES
('Sen. Donald Trump','R','NJ',DATETIME('now'),DATETIME('now'));

SELECT * FROM congress_members WHERE location LIKE 'NJ' AND name LIKE 'Sen%';

DELETE FROM congress_members WHERE id=102;

UPDATE votes
SET politician_id=531
WHERE politician_id=102;
<!-- Extra Credit: SQL Shortcuts and Intricacies - Chances are you wrote out a pretty long SQL statement to do your INSERT statement. How could you shorten it? Careful! SQLite, just like PostgreSQL and MySQL, have subtle and annoying differences to the standard SQL language. Make sure you use SQLite syntax for this! -->

<!-- Find all the voters that are not registered as republican or democrat (AND only voted once), and delete them. -->
DELETE FROM voters
WHERE voters.id IN (SELECT voter_id
FROM votes
GROUP BY voter_id
HAVING COUNT(voter_id)<2) AND NOT party IN ('republican','democrat');

SELECT * FROM voters WHERE voters.id IN (SELECT voter_id
FROM votes
GROUP BY voter_id
HAVING COUNT(voter_id)<2) AND NOT party IN ('republican','democrat');

SELECT * FROM voters WHERE NOT party IN ('republican','democrat') LIMIT 5;

<!-- Delete all the voters (and their votes) that are homeowners, employed, have no children, and have been with their party for less than 3 years AND have voted for politicians that speak at a grade level higher than 12. -->

SELECT voter_id 
FROM voters JOIN votes ON voters.id=voter_id JOIN congress_members on congress_members.id=politician_id
WHERE homeowner>0 AND employed>0 AND children_count=0 AND party_duration<3 AND congress_members.grade_current>12
GROUP BY voter_id;

<!-- Update the votes for all the men over 80 that have no children. Change their vote to be for the secret politician with ID 346. -->

<!-- Update the votes for top smarty pants politician (based on their speaking level - grade_1996). Shift the votes instead to the congress person that speaks at the lowest grade level. -->

<!-- Paste the text from your trace file into the repo and keep it for future reference on JOIN's and other SQL calls. Include the schemas of the 3 tables in the gist. Include some on the output of the queries if you're worried you won't remember all this syntax! -->