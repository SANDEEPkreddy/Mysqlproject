create database world_cup;
use world_cup;
SELECT COUNT(*) FROM players_info;
SELECT COUNT(*) FROM fow_card;
select count(*) from batting_card;
select count(*) from bowling_card;
select count(*) from matches;
select count(*) from odi_partnership_card;
select * from batting_card;
# 1 Retrieve the names of all the teams in the dataset.
select distinct Team1_Name from matches;

# 2 List all the players who have scored a century (100 runs or more) in a single match.
select b.runs , m.match_Date ,m.Match_ID,p.player_name
from batting_card as b inner join matches as m on m.Match_ID = b.Match_ID
inner join players_info as p on p.player_id = b.player_id
where b.runs >= 100;
select * from batting_card;
# 3 Find the highest individual score by a player in the dataset.
select max(b.runs) as totals , p.player_name , p.player_id from batting_card as b
inner join players_info as p on p.player_id = b.player_id group by p.player_name , p.player_id order by totals desc;

# 4 Retrieve the total number of centuries scored by the player in the dataset.
select count(*) as total_centuries , p.player_name from batting_card 
as b inner join players_info as p on p.player_id = b.player_id where runs >= 100 group by p.player_name order by total_centuries desc;

# 5 Retrieve the total number of half-centuries scored by the player in the dataset.
select count(*) as total_centuries , p.player_name from batting_card 
as b inner join players_info as p on p.player_id = b.player_id where runs >= 50 group by p.player_name order by total_centuries desc;

# 6 List the players who have taken five or more wickets in a single match.
select b.wickets , m.match_Date ,m.Match_ID,p.player_name
from bowling_card as b inner join matches as m on m.Match_ID = b.Match_ID
inner join players_info as p on p.player_id = b.bowler_id
where b.wickets >= 5;

# 7 Find the team that has won the most matches.
select Team1_Name,count(Match_Winner)as winner from matches group by Team1_Name order by winner desc;
select Team2_Name,count(Match_Winner)as winner from matches group by Team2_Name order by winner desc;

# 8 Display the total number of matches played in the dataset.
select distinct count(Match_Id) as total_matches from matches;

# 9 Display the average runs scored by each team in the dataset.
select round(avg(Team1_Runs_Scored)) as avg_runs ,Team1_Name from matches group by Team1_Name order by avg_runs desc;
select round(avg(Team2_Runs_Scored)) as avg_runs ,Team2_Name from matches group by Team2_Name order by avg_runs desc;

# 10 Retrieve the details of the matches where a team scored more than 300 runs.
select Team1_Runs_Scored , Team1_Name , Match_ID ,match_Date from matches where Team1_Runs_Scored >= 300;

# 11 List the players who have scored more than 500 runs in total in year wise
select sum(b.runs) as total_runs , extract(year from m.match_Date) as year ,p.player_name
from batting_card as b inner join matches as m on m.Match_ID = b.Match_ID
inner join players_info as p on p.player_id = b.player_id
 group by year,p.player_name having total_runs >= 500 order by total_runs desc;

# 12 Display the number of matches played in each year.
select count(Match_Name) as count_matches ,extract(year from match_Date)
as year from matches group by year order by  count_matches desc;

# 13 List the players who have played in more than 100 matches.
select count(m.Match_ID) as total_matches,p.player_name from batting_card as b
inner join players_info as p on b.player_id = p.player_id
inner join matches as m on m.Match_ID = b.Match_ID
group by p.player_name having total_matches >=100 order by total_matches desc;

# 14 List the players who have taken  more than 100 wickets.
select sum(wickets) as total_wickets , p.player_name from bowling_card as b
inner join players_info as p on p.player_id = b.bowler_id group by p.player_name
having total_wickets >= 100 order by total_wickets desc;

# 16 List the player who have taken highest wickets in 2023.
select sum(wickets) as total_wickets , p.player_name ,extract( year from match_Date) as year from bowling_card as b
inner join players_info as p on p.player_id = b.bowler_id 
inner join matches as m on m.Match_ID = b.Match_ID
group by p.player_name , year
having year = 2023 order by total_wickets desc;

# 17 list the player who have taken highest catches
select count(b.wicketType) as total_taken_catches , p.player_name  , wicketType from batting_card as b
inner join players_info as p on p.player_id = b.player_id
inner join bowling_card as bw on b.Match_ID = bw.Match_ID and b.player_id = bw.bowler_id where b.wicketType = "caught" 
group by p.player_name,b.wicketType order by total_taken_catches desc;

# 18 list the player who have taken wicket by the bowled
select count(b.wicketType) as total_taken_catches , p.player_name  , wicketType from batting_card as b
inner join players_info as p on p.player_id = b.player_id
inner join bowling_card as bw on b.Match_ID = bw.Match_ID and b.player_id = bw.bowler_id where b.wicketType = "bowled" 
group by p.player_name,b.wicketType order by total_taken_catches desc;

# 19 list the player who have taken wicket by the lbw
select count(b.wicketType) as total_taken_catches , p.player_name  , wicketType from batting_card as b
inner join players_info as p on p.player_id = b.player_id
inner join bowling_card as bw on b.Match_ID = bw.Match_ID and b.player_id = bw.bowler_id where b.wicketType = "lbw" 
group by p.player_name,b.wicketType order by total_taken_catches desc;

# 20 list the player who have taken wicket by the stumping
select count(b.wicketType) as total_taken_catches , p.player_name  , wicketType from batting_card as b
inner join players_info as p on p.player_id = b.player_id
inner join bowling_card as bw on b.Match_ID = bw.Match_ID and b.player_id = bw.bowler_id where b.wicketType = "stumping" 
group by p.player_name,b.wicketType order by total_taken_catches desc;

# 21 list the player who have hitted the highest fours
select p.player_name, sum(b.fours) as total_fours , count(Match_ID) as total_innings from batting_card as b
inner join players_info as p on p.player_id = b.player_id
group by p.player_name order by total_fours desc;

# 22 list the player who have hitted the highest sixes
select p.player_name, sum(b.sixes) as total_sixes , count(Match_ID) as total_innings  from batting_card as b
inner join players_info as p on p.player_id = b.player_id
group by p.player_name order by total_sixes desc;
 
# 23 list the which player is  out for the first ball
select count(b.wicketType) as first_ball , p.player_name ,count(ops.innings) as innings , wicketType from batting_card as b
inner join players_info as p on p.player_id = b.player_id
inner join odi_partnership_card as ops on b.Match_ID = ops.Match_ID
inner join bowling_card as bw on b.Match_ID = bw.Match_ID and b.player_id = bw.bowler_id
where player1ID_balls = '1' and player2ID_balls = '1' and b.wicketType is not null group by 
b.wicketType,p.player_name order by first_ball desc;
 
 # 24 list the bowler name highest madien
select sum(maidens) as highest_madiens, p.player_name from bowling_card as b
inner join  players_info as p on p.player_id = b.bowler_id 
group by p.player_name order by highest_madiens  desc;


 




