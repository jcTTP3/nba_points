# Machine learning project to cluster NBA players by how they score.

## The Goal

The aim of this project is to use machine learning to better understand groups of basketball players based on what types of shots they attempt. Traditionally, players have been classified into 3 main positions: guard, forward, and center. Historically these positions were useful to infer how a player operates within the context of a game. However due to evolution of the game over the past 10 or so years, it has become more difficult to rely on these positions to determine how a player scores the ball on offense. 

![Distribution of Shot Distance by Traditional Position](https://github.com/jcTTP3/nba_points/blob/master/images/jc_distribution_shot_distance_traditional.jpeg)

I attempted to use clustering techniques to re-group players based on what types of shots they take, what distance they shoot these shots from, and their proficiency shooting these types of shots. This will hopefully provide a better understanding as to how players operate on the floor. The example below shows two players who play the same "position" but attack in very different ways.

![Traditional](https://github.com/jcTTP3/nba_points/blob/master/images/pgs.png)

## The Data

The National Basketball Association provides publicly available information on all games through their API. For the purpose of this project, I gathered box scores, advanced box scores, and shot specific data from the 2017 season through the present (January 21, 2020). This data set includes data from:

* 3,000+ games
* 700+ players
* 540,000+ shots


I dropped players from the dataset who do not average at least 5 shot attempts per game. 

## Features

For this project, I wanted to group players based on **how** they score on offense. The NBA tags every shot taken in a game with 46 different shot types. However, some of these shots are very similar to one another and using them to differentiate players would create unwanted sensitivities between player groups. For example, there is not much difference between a "layup" and "driving layup". Plays like these are essentially the same thing. Therefore I grouped the NBA's 46 shot types into 8 broader categories which I felt best captured the essence of each offensive action: 


| Group #1| Group #2 | Group #3 | Group #4 | Group #5 | Group #6 | Group #7 | Group #8 |
|------------------------------|---------------------------|--------------------------------|---------------------------------|---------------------------|------------------------------------|--------------------|--------------------|
| Alley Oop Dunk Shot | Cutting Dunk Shot | Running Finger Roll Layup Shot | Driving Floating Bank Jump Shot | Driving Bank Hook Shot | Pullup Jump shot | Jump Bank Shot | Putback Dunk Shot |
| Alley Oop Layup shot | Driving Dunk Shot | Running Layup Shot | Driving Floating Jump Shot | Driving Hook Shot | Running Pull-Up Jump Shot | Jump Shot | Putback Layup Shot |
| Running Alley Oop Dunk Shot | Driving Reverse Dunk Shot | Cutting Finger Roll Layup Shot | Floating Jump shot | Hook Bank Shot | Step Back Bank Jump Shot | Fadeaway Jump Shot | Tip Dunk Shot |
| Running Alley Oop Layup Shot | Dunk Shot | Cutting Layup Shot |  | Hook Shot | Step Back Jump shot | Running Jump Shot | Tip Layup Shot |
|  | Reverse Dunk Shot | Driving Finger Roll Layup Shot |  | Turnaround Bank Hook Shot | Turnaround Fadeaway shot |  |  |
|  | Running Dunk Shot | Driving Layup Shot |  | Turnaround Hook Shot | Turnaround Fadeaway Bank Jump Shot |  |  |
|  | Running Reverse Dunk Shot | Driving Reverse Layup Shot |  |  | Turnaround Jump Shot |  |  |
|  |  | Finger Roll Layup Shot |  |  |  |  |  |
|  |  | Layup Shot |  |  |  |  |  |
|  |  | Reverse Layup Shot |  |  |  |  |  |
|  |  | Running Reverse Layup Shot |  |  |  |  |  |

For each of these 8 shot groups, I calculated **average shot distance**, **field goal percentage**, and **frequency of each kind of shot** in order to find groups of players who are similar based on their actions on the court. I also included other features for clustering including such as field goal attempts per game, percentage of 2FGA, percentage of 3FGA, percentage of unnassisted versus assisted field goals, and percentage of points off turnovers along with a few others.

## Results
Ultimately, I used hierarchical agglomerative clustering for my groups of players with the following parameters:

* n_clusters = 25
* linkage = 'ward'

These parameters yielded results with a fair balance of clusters, yet differentiated players in a logical way based on the features described above. To explore the results of the clustering in detail or any spefic player's detailed shot breakdown, please see the [shot_visualizations notebook](https://github.com/jcTTP3/nba_points/blob/master/shot_visualizations.ipynb) (Interactive NBA Player Data). For an example of the interactive charts in the notebook, please see the below:

![Cluster 9](https://github.com/jcTTP3/nba_points/blob/master/images/clust_9.png)
![Cluster 10](https://github.com/jcTTP3/nba_points/blob/master/images/clust_10.png)

In the notebook mentioned above, I also found which teams are most and least successful against each cluster along with which teams are most and least successful against each player's dominant shot type.

The accompanying presentation for this project can be found [here](https://docs.google.com/presentation/d/1vInFUKrXTP86o5pRafPj92p64Ll-aROihX9GJdGi2EE/edit?usp=sharing).

## Other Methods Attempted

* Kmeans clustering
* n_clusters = 20, 30
* linkage = 'complete'

## Conclusion and Ideas for Further Exploration

Through these techniques, one can can get a better feel for how a player scores much more clearly than purely using his position to infer his playing style. In the context of game planning for opponents, the results of this project can be used to assess which players have similar tendencies on offense and which teams have been successful defending against a specific type of player. However, the methods used in this project only analyze scoring, one component of a player's offensive repertoire. To understand other aspects of someone's game, other features could be considered like passing and rebounding.

In the future, I would like to explore whether using these cluster's to predict offensive output against certain teams is useful. For example, do players in a specific cluster score more than their average against certain teams than they normally do? I would also like to enhance this model with additional features like time of possession a player has the ball in his hands before attempting a shot, number of dribbles, and distance from the closest defender when shooting.
