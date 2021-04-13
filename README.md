# NHL-Statistics
Data visualization of NHL Games with Power BI
- Data Source: NHL Game Data https://www.kaggle.com/martinellis/nhl-game-data (Game, goalie_stats, skater_stats, team_stats)
- Image Sources
  - Background: https://wallpaperaccess.com/ice-hockey
  - Team Logos: https://www.stickpng.com/cat/sports/ice-hockey/national-hockey-league?page=1
- Software: Microsoft Power BI
#### DAX: Time on Ice - Converting format seconds to MM:SS
```
Duration = 
VAR Duration = [Goalie Time on Ice]
VAR Minutes =
    INT ( Duration / 60)
VAR Seconds =
    ROUNDUP(MOD ( Duration - (Minutes * 60), 60), 0) 

VAR M =
    IF (
        LEN ( Minutes ) = 1,
        CONCATENATE ( "0", Minutes ),
        CONCATENATE ( "", Minutes )
    )
VAR S =
    IF (
        LEN ( Seconds ) = 1,
        CONCATENATE ( "0", Seconds ),
        CONCATENATE ( "", Seconds )
    )
RETURN
    CONCATENATE (
        M,
        CONCATENATE ( ":", S )  
    )
```
#### DAX: Retrieve Season Best Goalie's stat
```
Top 1 = FIRSTNONBLANK(TOPN(1, VALUES(Players[FullName]),[Goalie Wins]),1)
Top 1 (Duration) = CALCULATE([Goalie Duration], TOPN(1, ALL(Fact_GoalieStats[player_id]), [Goalie Wins],DESC))
Top 1 (Save%) = CALCULATE([Save %], TOPN(1, ALL(Fact_GoalieStats[player_id]), [Goalie Wins],DESC))
```
#### DAX: Caculate Rankings
```
Team Ranking = RANKX(ALL(Teams), [Team Points],,DESC)
Skater Ranking = RANKX( FILTER(ALL(Players), [position]<>"Goaltender"), [Skater Points],, DESC)
Goalie Ranking = RANKX( FILTER(ALL(Players), [position]="Goaltender"), [Goalie Wins],, DESC)
```
#### Model
![Model](https://github.com/Helena-ys/NHL-Statistics/blob/main/Model.PNG?raw=true)
