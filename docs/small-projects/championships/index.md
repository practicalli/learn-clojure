# Project: Championships

A project to show the winner, predicted winner and leaderboard from a competition.

Competitions are defined in a hashmap


```clojure
{:competition :competition-id
 :year :2023
 :winner ""
 :predicted-winner ""
 :drivers
  [{:name "Max Verstapen" :score 364}
   {:name "Sergio Perez" :score 219}
   {:name "Fernando Alonso" :score 219}
   ,,,
  ]}
```


Data for each race

```clojure
{:name :monza
 :date "month-day (main race)"
 :results 
 [{:driver "" :score 25}]]}
```


## Edge cases 

- Drivers with equal scores considers overall results and highest scores over current season
