<!-- The Election Board suspects there's some cheating going on! 
Help them figure out what is going on and if anyone is to blame.

(a) Which Congress member received the most votes?
<!-- List his/her name and a count of their votes. -->
SELECT name, COUNT(politician_id)
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY politician_id
ORDER BY COUNT(politician_id) DESC LIMIT 1; -->


<!-- (b) Who were the people that voted for that politician?
<!-- List their names, gender and party. -->
SELECT first_name||last_name


<!-- How many votes were received by Congress members whose communication grade average was less than 9 
(this number can be found in the grade_current field)? List their name, location, grade since 1996, 
and the vote count. -->

<!-- What 10 states had the most voters turnout? (Does this correspond to the population of those states?) 
List the people that voted in the top state's elections. (It will be a big list, and you can use the results 
from your first query to help simplify this next query.) -->

<!-- List the people that voted more than 2 times? (It should only be once for their Senator and once for their 
representative!) Ay Caramba! We have some serious ballot stuffing! Report this to the Election Board! -->

<!-- Did anyone vote for the same politician twice? What was the name of the voter and the politician they voted for? 
Pretty sneaky... -->

<!-- Paste the text from your trace file into the repo and keep it for future reference on JOIN's and other SQL calls. 
Include the schemas of the 3 tables in the gist. 
Include some on the output of the queries if you're worried you won't remember all this syntax! --> -->