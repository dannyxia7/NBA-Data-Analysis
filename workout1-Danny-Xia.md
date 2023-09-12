Workout1
================
Danny Xia
9/26/2018

Workout 1 - Data Wrangling and Visualization
============================================

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.4.4

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
```

Ranking of Teams
----------------

``` r
teams = read.csv("../data/nba2018-teams.csv")
team_sal = arrange(teams, desc(salary))
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

``` r
avg_salary = mean(teams$salary)
ggplot(team_sal, aes(x = reorder(team, 30:1), y = salary)) +
  geom_bar(aes(fill = salary), stat = "identity") +
  coord_flip() +
  geom_abline(intercept = avg_salary, slope = 0, color = "red") +
  labs(x = "Team", y = "Salary (in millions)", title = "NBA Teams Ranked by Total Salary")
```

![](https://github.com/dannyxia7/NBA-Data-Analysis/blob/4eb72cbb88fc1dd56a5a12157f12f2571ba0c13c/barplot%20of%20Score-1.png)

``` r
team_pts = arrange(teams, desc(points))
avg_pts = mean(teams$points)
ggplot(team_pts, aes(x = reorder(team, 30:1), y = points)) +
  geom_bar(aes(fill = points), stat = "identity") +
  coord_flip() +
  geom_abline(slope = 0, intercept = avg_pts, color = "red") +
  labs(x = "Team", y = "Points", title = "NBA Teams Ranked by Total Points")
```

![](workout1-Danny-Xia_files/figure-markdown_github/barplot%20of%20total%20points-1.png)

``` r
team_eff = arrange(teams, desc(efficiency))
avg_eff = mean(teams$efficiency)
ggplot(team_eff, aes(x = reorder(team, 30:1), y = efficiency)) +
  geom_bar(aes(fill = efficiency), stat = "identity") +
  coord_flip() +
  geom_abline(slope = 0, intercept = avg_eff, color = "red") +
  labs(x = "Team", y = "Efficiency", title = "NBA Teams Ranked by Total Efficiency")
```

![](workout1-Danny-Xia_files/figure-markdown_github/barplot%20of%20efficiency-1.png)

``` r
team_score = 
  teams %>% 
  mutate(score = 1.5*points3 + points2 + points1 + 1.5*assists + salary - turnovers)
team_score = arrange(team_score, desc(score))
avg_score = mean(team_score$score)
ggplot(team_score, aes(x = reorder(team, 30:1), y = score)) +
  geom_bar(aes(fill = score), stat = "identity") +
  coord_flip() +
  geom_abline(slope = 0, intercept = avg_score, color = "red") +
  labs(x = "Team", y = "Score", title = "NBA Teams Ranked by Score")
```

![](workout1-Danny-Xia_files/figure-markdown_github/barplot%20of%20Score-1.png)

-   The statistic that I have come up with, named Score, is
    1.5 \* *p**o**i**n**t**s*3 + *p**o**i**n**t**s*2 + *p**o**i**n**t**s*1 + 1.5 \* *a**s**s**i**s**t**s* + *s**a**l**a**r**y* − *t**u**r**n**o**v**e**r**s*
-   I have weighted 3 points made and assists because there aren't as many per game as the other statistics like free throws and 2-point field goals, but teams with high 3-pointers and assists in the current NBA tend to perform better. Rebounds don't always indicate a stronger team, since many good teams look for higher quality shots and do not miss as often, so there are less chances to get rebounds. These trends can be easily seen by teams like the Warriors, Rockets, and Cavaliers. Salary is also a large factor, as teams that go over the cap tend to do so because they have a strong team that they do not want to separate in spite of the salary cap and luxury tax and other NBA cap rules. Also, salary correlates with performance. Finally, limiting turnovers is an essential part of how well a team plays, given that possessions are opportunities to score, and a turnover is losing that opportunity and giving one more to an opponent.

