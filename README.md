Project 1
================
Jingjing Li
6/13/2021

-   [Contents](#contents)
    -   [Introduction](#introduction)
    -   [Link to repo](#link-to-repo)
    -   [Required packages](#required-packages)
    -   [Functions](#functions)
        -   [NHL records API](#nhl-records-api)
        -   [NHL stats API](#nhl-stats-api)
    -   [wrapper function](#wrapper-function)
    -   [Exploratory Data analysis](#exploratory-data-analysis)
        -   [Data from two endpoints](#data-from-two-endpoints)
        -   [New variables](#new-variables)
        -   [contingency tables](#contingency-tables)
        -   [Five plots](#five-plots)

# Contents

## Introduction

This project aims to create a vignette for reading and summarizing data
from the National Hockey League’s (NHL) API.

## Link to repo

## Required packages

-   httr  
-   jsonlite

## Functions

### NHL records API

#### Load packages

``` r
# load package to get data from API link
library(httr)
library(jsonlite)
library(dplyr)
```

#### franchise function

``` r
# Construct a function for API data acquisition

franchise <- function (id,name){
  base_url <- "https://records.nhl.com/site/api/franchise"
  get_url <- GET (base_url)
  record_txt <- content (get_url, "text", encoding = "UTF-8")
  record_json <- fromJSON(record_txt, flatten=TRUE)
  Data_df <- data.frame( record_json)
  if (id==0 & name == "full") {
   return(Data_df) 
  }
   else{record <- filter(Data_df, data.id==id | data.fullName==name)
   return(record)
   }
}
record_franchise1 <- franchise (2, "Montreal Wanderers")
record_franchise1
```

    ##   data.id data.firstSeasonId      data.fullName data.lastSeasonId
    ## 1       2           19171918 Montreal Wanderers          19171918
    ##   data.mostRecentTeamId data.teamAbbrev data.teamCommonName
    ## 1                    41             MWN           Wanderers
    ##   data.teamPlaceName total
    ## 1           Montreal    39

``` r
record_franchise2 <- franchise (0,"full")
record_franchise2
```

    ##    data.id data.firstSeasonId         data.fullName data.lastSeasonId
    ## 1        1           19171918    Montréal Canadiens                NA
    ## 2        2           19171918    Montreal Wanderers          19171918
    ## 3        3           19171918      St. Louis Eagles          19341935
    ## 4        4           19191920       Hamilton Tigers          19241925
    ## 5        5           19171918   Toronto Maple Leafs                NA
    ## 6        6           19241925         Boston Bruins                NA
    ## 7        7           19241925      Montreal Maroons          19371938
    ## 8        8           19251926    Brooklyn Americans          19411942
    ## 9        9           19251926  Philadelphia Quakers          19301931
    ## 10      10           19261927      New York Rangers                NA
    ## 11      11           19261927    Chicago Blackhawks                NA
    ## 12      12           19261927     Detroit Red Wings                NA
    ## 13      13           19671968      Cleveland Barons          19771978
    ## 14      14           19671968     Los Angeles Kings                NA
    ## 15      15           19671968          Dallas Stars                NA
    ## 16      16           19671968   Philadelphia Flyers                NA
    ## 17      17           19671968   Pittsburgh Penguins                NA
    ## 18      18           19671968       St. Louis Blues                NA
    ## 19      19           19701971        Buffalo Sabres                NA
    ## 20      20           19701971     Vancouver Canucks                NA
    ## 21      21           19721973        Calgary Flames                NA
    ## 22      22           19721973    New York Islanders                NA
    ## 23      23           19741975     New Jersey Devils                NA
    ## 24      24           19741975   Washington Capitals                NA
    ## 25      25           19791980       Edmonton Oilers                NA
    ## 26      26           19791980   Carolina Hurricanes                NA
    ## 27      27           19791980    Colorado Avalanche                NA
    ## 28      28           19791980       Arizona Coyotes                NA
    ## 29      29           19911992       San Jose Sharks                NA
    ## 30      30           19921993       Ottawa Senators                NA
    ## 31      31           19921993   Tampa Bay Lightning                NA
    ## 32      32           19931994         Anaheim Ducks                NA
    ## 33      33           19931994      Florida Panthers                NA
    ## 34      34           19981999   Nashville Predators                NA
    ## 35      35           19992000         Winnipeg Jets                NA
    ## 36      36           20002001 Columbus Blue Jackets                NA
    ## 37      37           20002001        Minnesota Wild                NA
    ## 38      38           20172018  Vegas Golden Knights                NA
    ## 39      39           20212022        Seattle Kraken                NA
    ##    data.mostRecentTeamId data.teamAbbrev data.teamCommonName
    ## 1                      8             MTL           Canadiens
    ## 2                     41             MWN           Wanderers
    ## 3                     45             SLE              Eagles
    ## 4                     37             HAM              Tigers
    ## 5                     10             TOR         Maple Leafs
    ## 6                      6             BOS              Bruins
    ## 7                     43             MMR             Maroons
    ## 8                     51             BRK           Americans
    ## 9                     39             QUA             Quakers
    ## 10                     3             NYR             Rangers
    ## 11                    16             CHI          Blackhawks
    ## 12                    17             DET           Red Wings
    ## 13                    49             CLE              Barons
    ## 14                    26             LAK               Kings
    ## 15                    25             DAL               Stars
    ## 16                     4             PHI              Flyers
    ## 17                     5             PIT            Penguins
    ## 18                    19             STL               Blues
    ## 19                     7             BUF              Sabres
    ## 20                    23             VAN             Canucks
    ## 21                    20             CGY              Flames
    ## 22                     2             NYI           Islanders
    ## 23                     1             NJD              Devils
    ## 24                    15             WSH            Capitals
    ## 25                    22             EDM              Oilers
    ## 26                    12             CAR          Hurricanes
    ## 27                    21             COL           Avalanche
    ## 28                    53             ARI             Coyotes
    ## 29                    28             SJS              Sharks
    ## 30                     9             OTT            Senators
    ## 31                    14             TBL           Lightning
    ## 32                    24             ANA               Ducks
    ## 33                    13             FLA            Panthers
    ## 34                    18             NSH           Predators
    ## 35                    52             WPG                Jets
    ## 36                    29             CBJ        Blue Jackets
    ## 37                    30             MIN                Wild
    ## 38                    54             VGK      Golden Knights
    ## 39                    55             SEA              Kraken
    ##    data.teamPlaceName total
    ## 1            Montréal    39
    ## 2            Montreal    39
    ## 3           St. Louis    39
    ## 4            Hamilton    39
    ## 5             Toronto    39
    ## 6              Boston    39
    ## 7            Montreal    39
    ## 8            Brooklyn    39
    ## 9        Philadelphia    39
    ## 10           New York    39
    ## 11            Chicago    39
    ## 12            Detroit    39
    ## 13          Cleveland    39
    ## 14        Los Angeles    39
    ## 15             Dallas    39
    ## 16       Philadelphia    39
    ## 17         Pittsburgh    39
    ## 18          St. Louis    39
    ## 19            Buffalo    39
    ## 20          Vancouver    39
    ## 21            Calgary    39
    ## 22           New York    39
    ## 23         New Jersey    39
    ## 24         Washington    39
    ## 25           Edmonton    39
    ## 26           Carolina    39
    ## 27           Colorado    39
    ## 28            Arizona    39
    ## 29           San Jose    39
    ## 30             Ottawa    39
    ## 31          Tampa Bay    39
    ## 32            Anaheim    39
    ## 33            Florida    39
    ## 34          Nashville    39
    ## 35           Winnipeg    39
    ## 36           Columbus    39
    ## 37          Minnesota    39
    ## 38              Vegas    39
    ## 39            Seattle    39

``` r
total <- function (id, name){
  base_url <- "https://records.nhl.com/site/api"
  full_url <- paste0 (base_url, "/franchise-team-totals")
  get_url <- GET (full_url)
  record_txt <- content (get_url, "text", encoding = "UTF-8")
  record_json <- fromJSON(record_txt, flatten=TRUE)
  Data_df <- data.frame( record_json)
   if (id==0 & name == "full") {
   return(Data_df) 
  }
   else{record <- filter(Data_df, data.franchiseId==id | data.teamName==name )
   }
  
  return(record)
  }
record_total1 <- total (2, "Montreal Wanderers")
record_total1
```

    ##   data.id data.activeFranchise data.firstSeasonId data.franchiseId
    ## 1      79                    0           19171918                2
    ##   data.gameTypeId data.gamesPlayed data.goalsAgainst data.goalsFor
    ## 1               2                6                37            17
    ##   data.homeLosses data.homeOvertimeLosses data.homeTies data.homeWins
    ## 1               2                      NA             0             1
    ##   data.lastSeasonId data.losses data.overtimeLosses data.penaltyMinutes
    ## 1          19171918           5                  NA                  27
    ##   data.pointPctg data.points data.roadLosses data.roadOvertimeLosses
    ## 1         0.1667           2               3                      NA
    ##   data.roadTies data.roadWins data.shootoutLosses data.shootoutWins
    ## 1             0             0                   0                 0
    ##   data.shutouts data.teamId      data.teamName data.ties data.triCode
    ## 1             0          41 Montreal Wanderers         0          MWN
    ##   data.wins total
    ## 1         1   105

``` r
record_total2 <- total (0, "full")
record_total2
```

    ##    data.id data.activeFranchise data.firstSeasonId data.franchiseId
    ## 1        1                    1           19821983               23
    ## 2        2                    1           19821983               23
    ## 3        3                    1           19721973               22
    ## 4        4                    1           19721973               22
    ## 5        5                    1           19261927               10
    ## 6        6                    1           19261927               10
    ## 7        7                    1           19671968               16
    ## 8        8                    1           19671968               16
    ## 9        9                    1           19671968               17
    ## 10      10                    1           19671968               17
    ## 11      11                    1           19241925                6
    ## 12      12                    1           19241925                6
    ## 13      13                    1           19701971               19
    ## 14      14                    1           19701971               19
    ## 15      15                    1           19171918                1
    ## 16      16                    1           19171918                1
    ## 17      17                    1           19921993               30
    ## 18      18                    1           19921993               30
    ## 19      19                    1           19271928                5
    ## 20      20                    1           19271928                5
    ## 21      21                    1           19992000               35
    ## 22      22                    1           19992000               35
    ## 23      23                    1           19971998               26
    ## 24      24                    1           19971998               26
    ## 25      25                    1           19931994               33
    ## 26      26                    1           19931994               33
    ## 27      27                    1           19921993               31
    ## 28      28                    1           19921993               31
    ## 29      29                    1           19741975               24
    ## 30      30                    1           19741975               24
    ## 31      31                    1           19261927               11
    ## 32      32                    1           19261927               11
    ##    data.gameTypeId data.gamesPlayed data.goalsAgainst data.goalsFor
    ## 1                2             2993              8902          8792
    ## 2                3              257               634           697
    ## 3                2             3788             11907         12045
    ## 4                3              309               897           983
    ## 5                2             6560             20020         20041
    ## 6                3              518              1447          1404
    ## 7                3              449              1332          1335
    ## 8                2             4171             12255         13690
    ## 9                2             4171             14049         13874
    ## 10               3              391              1131          1190
    ## 11               2             6626             19137         21112
    ## 12               3              675              1907          1956
    ## 13               2             3945             11966         12471
    ## 14               3              256               765           763
    ## 15               3              773              1959          2306
    ## 16               2             6787             18260         21791
    ## 17               2             2195              6580          6250
    ## 18               3              151               372           357
    ## 19               2             6516             19953         19980
    ## 20               3              545              1491          1398
    ## 21               2              902              3014          2465
    ## 22               3                4                17             6
    ## 23               3              112               282           272
    ## 24               2             1812              5140          4914
    ## 25               2             2109              6122          5665
    ## 26               3               54               152           132
    ## 27               2             2194              6646          6216
    ## 28               3              176               458           485
    ## 29               2             3633             11553         11516
    ## 30               3              295               837           836
    ## 31               3              548              1669          1566
    ## 32               2             6560             19687         19537
    ##    data.homeLosses data.homeOvertimeLosses data.homeTies data.homeWins
    ## 1              525                      85            96           790
    ## 2               53                       0            NA            74
    ## 3              678                      84           170           963
    ## 4               53                       1            NA            94
    ## 5             1143                      76           448          1614
    ## 6              104                       0             1           137
    ## 7               97                       0            NA           135
    ## 8              584                      93           193          1216
    ## 9              683                      60           205          1138
    ## 10              85                       0            NA           113
    ## 11             960                      92           376          1885
    ## 12             151                       2             3           194
    ## 13             639                      84           197          1053
    ## 14              54                       0            NA            73
    ## 15             133                       0             3           258
    ## 16             881                      95           381          2038
    ## 17             413                      93            60           533
    ## 18              35                       0            NA            37
    ## 19            1082                      85           388          1702
    ## 20             120                       0             2           149
    ## 21             204                      38            26           183
    ## 22               2                       0            NA             0
    ## 23              24                       0            NA            32
    ## 24             323                      77            52           453
    ## 25             390                     115            65           485
    ## 26              15                       0            NA            13
    ## 27             414                      67            56           559
    ## 28              43                       0            NA            48
    ## 29             620                      83           153           959
    ## 30              77                       1            NA            75
    ## 31             104                       0             1           166
    ## 32            1128                      86           410          1655
    ##    data.lastSeasonId data.losses data.overtimeLosses data.penaltyMinutes
    ## 1                 NA        1211                 169               44773
    ## 2                 NA         120                   0                4266
    ## 3                 NA        1587                 166               57792
    ## 4                 NA         139                   0                5689
    ## 5                 NA        2716                 153               86129
    ## 6                 NA         266                   0                8181
    ## 7                 NA         218                   0                9104
    ## 8                 NA        1452                 183               76208
    ## 9                 NA        1734                 151               66221
    ## 10                NA         182                   0                6106
    ## 11                NA        2403                 191               88570
    ## 12                NA         337                   0               10607
    ## 13                NA        1564                 167               60671
    ## 14                NA         132                   0                4692
    ## 15                NA         321                   0               12150
    ## 16                NA        2302                 175               87484
    ## 17                NA         940                 169               29684
    ## 18                NA          79                   0                2102
    ## 19                NA        2696                 174               92331
    ## 20                NA         283                   0                8550
    ## 21          20102011         437                  78               13727
    ## 22          20102011           4                   0                 115
    ## 23                NA          54                   0                1310
    ## 24                NA         725                 174               19429
    ## 25                NA         870                 208               29171
    ## 26                NA          33                   0                 775
    ## 27                NA         947                 150               31086
    ## 28                NA          75                   0                2444
    ## 29                NA        1467                 163               57455
    ## 30                NA         156                   1                5152
    ## 31                NA         275                   0                8855
    ## 32                NA        2761                 173               92285
    ##    data.pointPctg data.points data.roadLosses data.roadOvertimeLosses
    ## 1          0.5306        3176             686                      84
    ## 2          0.0039           2              67                       0
    ## 3          0.5133        3889             909                      82
    ## 4          0.0129           8              86                       2
    ## 5          0.5127        6727            1573                      77
    ## 6          0.0000           0             162                       0
    ## 7          0.0045           4             121                       0
    ## 8          0.5752        4798             868                      90
    ## 9          0.5203        4340            1051                      91
    ## 10         0.0153          12              97                       1
    ## 11         0.5632        7464            1443                      99
    ## 12         0.0296          40             186                       2
    ## 13         0.5305        4186             925                      83
    ## 14         0.0000           0              78                       0
    ## 15         0.0000           0             188                       0
    ## 16         0.5863        7958            1421                      80
    ## 17         0.5071        2226             527                      76
    ## 18         0.0000           0              44                       0
    ## 19         0.5136        6693            1614                      89
    ## 20         0.0110          12             163                       1
    ## 21         0.4473         807             233                      40
    ## 22         0.0000           0               2                       0
    ## 23         0.0714          16              30                       2
    ## 24         0.5281        1914             402                      97
    ## 25         0.5045        2128             480                      93
    ## 26         0.0000           0              18                       0
    ## 27         0.5087        2232             533                      83
    ## 28         0.0625          22              32                       0
    ## 29         0.5321        3866             847                      80
    ## 30         0.0644          38              79                       2
    ## 31         0.0000           0             171                       1
    ## 32         0.5039        6611            1633                      87
    ##    data.roadTies data.roadWins data.shootoutLosses data.shootoutWins
    ## 1            123           604                  84                78
    ## 2             NA            63                   0                 0
    ## 3            177           725                  70                86
    ## 4             NA            76                   0                 0
    ## 5            360          1269                  68                79
    ## 6              7           107                   0                 0
    ## 7             NA            96                   0                 0
    ## 8            264           863                  92                53
    ## 9            178           765                  54                83
    ## 10            NA            96                   0                 0
    ## 11           415          1356                  82                68
    ## 12             3           138                   0                 0
    ## 13           212           752                  74                81
    ## 14            NA            51                   0                 0
    ## 15             5           186                   0                 0
    ## 16           456          1435                  66                69
    ## 17            55           438                  79                58
    ## 18            NA            35                   0                 0
    ## 19           385          1171                  77                59
    ## 20             1           110                   0                 0
    ## 21            19           159                  29                37
    ## 22            NA             0                   0                 0
    ## 23            NA            26                   0                 0
    ## 24            34           374                  61                50
    ## 25            77           404                  97                71
    ## 26            NA             8                   0                 0
    ## 27            56           426                  59                68
    ## 28            NA            53                   0                 1
    ## 29           150           741                  71                68
    ## 30            NA            63                   1                 0
    ## 31             4           102                   0                 0
    ## 32           404          1157                  70                75
    ##    data.shutouts data.teamId       data.teamName data.ties data.triCode
    ## 1            196           1   New Jersey Devils       219          NJD
    ## 2             25           1   New Jersey Devils        NA          NJD
    ## 3            177           2  New York Islanders       347          NYI
    ## 4             12           2  New York Islanders        NA          NYI
    ## 5            408           3    New York Rangers       808          NYR
    ## 6             44           3    New York Rangers         8          NYR
    ## 7             33           4 Philadelphia Flyers        NA          PHI
    ## 8            248           4 Philadelphia Flyers       457          PHI
    ## 9            189           5 Pittsburgh Penguins       383          PIT
    ## 10            30           5 Pittsburgh Penguins        NA          PIT
    ## 11           506           6       Boston Bruins       791          BOS
    ## 12            49           6       Boston Bruins         6          BOS
    ## 13           194           7      Buffalo Sabres       409          BUF
    ## 14            18           7      Buffalo Sabres        NA          BUF
    ## 15            68           8  Montréal Canadiens         8          MTL
    ## 16           543           8  Montréal Canadiens       837          MTL
    ## 17           137           9     Ottawa Senators       115          OTT
    ## 18            12           9     Ottawa Senators        NA          OTT
    ## 19           422          10 Toronto Maple Leafs       773          TOR
    ## 20            50          10 Toronto Maple Leafs         3          TOR
    ## 21            41          11   Atlanta Thrashers        45          ATL
    ## 22             0          11   Atlanta Thrashers        NA          ATL
    ## 23            11          12 Carolina Hurricanes        NA          CAR
    ## 24            99          12 Carolina Hurricanes        86          CAR
    ## 25           115          13    Florida Panthers       142          FLA
    ## 26             3          13    Florida Panthers        NA          FLA
    ## 27           124          14 Tampa Bay Lightning       112          TBL
    ## 28            14          14 Tampa Bay Lightning        NA          TBL
    ## 29           178          15 Washington Capitals       303          WSH
    ## 30            19          15 Washington Capitals        NA          WSH
    ## 31            32          16  Chicago Blackhawks         5          CHI
    ## 32           439          16  Chicago Blackhawks       814          CHI
    ##    data.wins total
    ## 1       1394   105
    ## 2        137   105
    ## 3       1688   105
    ## 4        170   105
    ## 5       2883   105
    ## 6        244   105
    ## 7        231   105
    ## 8       2079   105
    ## 9       1903   105
    ## 10       209   105
    ## 11      3241   105
    ## 12       332   105
    ## 13      1805   105
    ## 14       124   105
    ## 15       444   105
    ## 16      3473   105
    ## 17       971   105
    ## 18        72   105
    ## 19      2873   105
    ## 20       259   105
    ## 21       342   105
    ## 22         0   105
    ## 23        58   105
    ## 24       827   105
    ## 25       889   105
    ## 26        21   105
    ## 27       985   105
    ## 28       101   105
    ## 29      1700   105
    ## 30       138   105
    ## 31       268   105
    ## 32      2812   105
    ##  [ reached 'max' / getOption("max.print") -- omitted 73 rows ]

#### season function

``` r
season <- function (id){
  base_url <- "https://records.nhl.com/site/api/franchise-season-records?cayenneExp=franchiseId"
  if (id==0 ) {
   get_url <- GET (base_url) 
   record_txt <- content (get_url , "text", encoding = "UTF-8")
   record_json <- fromJSON(record_txt, flatten=TRUE)
   Data_df <- data.frame(record_json)
    return( list(base_url,Data_df))
  }
    else {
      full_url  <-  paste0(base_url,"=",id )    
      get_url <- GET (full_url)
      record_txt <- content (get_url , "text", encoding = "UTF-8")
      record_json <- fromJSON(record_txt, flatten=TRUE)
      Data_df <- data.frame(record_json)
      return( list(full_url,Data_df))
    }

 
}
record_season <- season (9)
record_season       
```

    ## [[1]]
    ## [1] "https://records.nhl.com/site/api/franchise-season-records?cayenneExp=franchiseId=9"
    ## 
    ## [[2]]
    ##   data.id data.fewestGoals data.fewestGoalsAgainst
    ## 1      38               NA                      NA
    ##   data.fewestGoalsAgainstSeasons data.fewestGoalsSeasons data.fewestLosses
    ## 1                             NA                      NA                NA
    ##   data.fewestLossesSeasons data.fewestPoints data.fewestPointsSeasons
    ## 1                       NA                NA                       NA
    ##   data.fewestTies data.fewestTiesSeasons data.fewestWins
    ## 1              NA                     NA              NA
    ##   data.fewestWinsSeasons data.franchiseId   data.franchiseName
    ## 1                     NA                9 Philadelphia Quakers
    ##   data.homeLossStreak  data.homeLossStreakDates data.homePointStreak
    ## 1                   8 Nov 29 1930 - Jan 08 1931                    5
    ##                              data.homePointStreakDates data.homeWinStreak
    ## 1 Jan 15 1926 - Feb 13 1926, Feb 23 1926 - Mar 15 1926                  5
    ##                                data.homeWinStreakDates
    ## 1 Jan 15 1926 - Feb 13 1926, Feb 23 1926 - Mar 15 1926
    ##   data.homeWinlessStreak data.homeWinlessStreakDates data.lossStreak
    ## 1                      8   Nov 29 1930 - Jan 08 1931              15
    ##        data.lossStreakDates data.mostGameGoals
    ## 1 Nov 29 1930 - Jan 08 1931                 10
    ##        data.mostGameGoalsDates data.mostGoals data.mostGoalsAgainst
    ## 1 Nov 19 1929 - TOR 5 @ PIR 10            102                   185
    ##   data.mostGoalsAgainstSeasons data.mostGoalsSeasons data.mostLosses
    ## 1                 1929-30 (44)          1929-30 (44)              36
    ##       data.mostLossesSeasons data.mostPenaltyMinutes
    ## 1 1929-30 (44), 1930-31 (44)                     503
    ##   data.mostPenaltyMinutesSeasons data.mostPoints data.mostPointsSeasons
    ## 1                   1930-31 (44)              46           1927-28 (44)
    ##   data.mostShutouts   data.mostShutoutsSeasons data.mostTies
    ## 1                11 1927-28 (44), 1928-29 (44)             8
    ##         data.mostTiesSeasons data.mostWins       data.mostWinsSeasons
    ## 1 1927-28 (44), 1928-29 (44)            19 1925-26 (36), 1927-28 (44)
    ##   data.pointStreak     data.pointStreakDates data.roadLossStreak
    ## 1                6 Mar 10 1928 - Mar 24 1928                  12
    ##                               data.roadLossStreakDates
    ## 1 Nov 26 1929 - Jan 21 1930, Nov 15 1930 - Jan 22 1931
    ##   data.roadPointStreak data.roadPointStreakDates data.roadWinStreak
    ## 1                    6 Jan 24 1928 - Feb 16 1928                  4
    ##                                data.roadWinStreakDates
    ## 1 Jan 01 1927 - Jan 20 1927, Jan 31 1928 - Feb 14 1928
    ##   data.roadWinlessStreak data.roadWinlessStreakDates data.winStreak
    ## 1                     22   Nov 26 1929 - Mar 13 1930              6
    ##         data.winStreakDates data.winlessStreak   data.winlessStreakDates
    ## 1 Mar 10 1928 - Mar 24 1928                 16 Feb 04 1930 - Mar 18 1930
    ##   total
    ## 1     1

``` r
#### goalie function
```

``` r
goalie<- function (id){
  base_url <- "https://records.nhl.com/site/api/franchise-goalie-records?cayenneExp=franchiseId"
   if (id==0 ) {
  get_url <- GET (base_url) 
   record_txt <- content (get_url , "text", encoding = "UTF-8")
   record_json <- fromJSON(record_txt, flatten=TRUE)
   Data_df <- data.frame(record_json)
    return( list(base_url,Data_df))
  }
    else {
      full_url  <-  paste0(base_url,"=",id )    
      get_url <- GET (full_url)
      record_txt <- content (get_url , "text", encoding = "UTF-8")
      record_json <- fromJSON(record_txt, flatten=TRUE)
      Data_df <- data.frame(record_json)
      return( list(full_url,Data_df))
    }
}
record_goalie1<- goalie (6)
record_goalie1
```

    ## [[1]]
    ## [1] "https://records.nhl.com/site/api/franchise-goalie-records?cayenneExp=franchiseId=6"
    ## 
    ## [[2]]
    ##    data.id data.activePlayer data.firstName data.franchiseId
    ## 1      347             FALSE           Yves                6
    ## 2      352             FALSE         Daniel                6
    ## 3      356             FALSE          Craig                6
    ## 4      374             FALSE            Jon                6
    ## 5      380             FALSE            Tim                6
    ## 6      427             FALSE         Gilles                6
    ## 7      432             FALSE            Ron                6
    ## 8      438             FALSE           Jeff                6
    ## 9      479             FALSE           Doug                6
    ## 10     487             FALSE         Rejean                6
    ## 11     528             FALSE           Andy                6
    ## 12     542             FALSE           Paul                6
    ## 13    1185             FALSE          Frank                6
    ## 14     555             FALSE             Ed                6
    ## 15     247             FALSE          Gerry                6
    ## 16     566             FALSE           Wilf                6
    ## 17     581             FALSE            Jim                6
    ## 18     586             FALSE           Bert                6
    ## 19     589             FALSE           Jack                6
    ## 20     592             FALSE          Benny                6
    ## 21     601             FALSE            Jim                6
    ## 22     300             FALSE          Eddie                6
    ## 23     617             FALSE         Howard                6
    ## 24     619             FALSE          Harry                6
    ## 25    1217             FALSE           Jack                6
    ## 26     628             FALSE        Jacques                6
    ## 27     639             FALSE          Terry                6
    ## 28     290             FALSE           Tiny                6
    ## 29    1213             FALSE            Hal                6
    ## 30     660             FALSE           Pete                6
    ## 31     682             FALSE        Vincent                6
    ## 32     684             FALSE            Pat                6
    ## 33     688             FALSE        Roberto                6
    ##    data.franchiseName data.gameTypeId data.gamesPlayed data.lastName
    ## 1       Boston Bruins               2                8      Belanger
    ## 2       Boston Bruins               2                8    Berthiaume
    ## 3       Boston Bruins               2               35    Billington
    ## 4       Boston Bruins               2               57         Casey
    ## 5       Boston Bruins               2                2     Cheveldae
    ## 6       Boston Bruins               2              277       Gilbert
    ## 7       Boston Bruins               2               40       Grahame
    ## 8       Boston Bruins               2               18       Hackett
    ## 9       Boston Bruins               2              154         Keans
    ## 10      Boston Bruins               2              183       Lemelin
    ## 11      Boston Bruins               2              261          Moog
    ## 12      Boston Bruins               2               43      Bibeault
    ## 13      Boston Bruins               2              444       Brimsek
    ## 14      Boston Bruins               2                4      Chadwick
    ## 15      Boston Bruins               2              416      Cheevers
    ## 16      Boston Bruins               2                2          Cude
    ## 17      Boston Bruins               2                1        Franks
    ## 18      Boston Bruins               2               41      Gardiner
    ## 19      Boston Bruins               2              141      Gelineau
    ## 20      Boston Bruins               2                1         Grant
    ## 21      Boston Bruins               2              237         Henry
    ## 22      Boston Bruins               2              444      Johnston
    ## 23      Boston Bruins               2                2      Lockhart
    ## 24      Boston Bruins               2               77        Lumley
    ## 25      Boston Bruins               2               23        Norris
    ## 26      Boston Bruins               2                8        Plante
    ## 27      Boston Bruins               2              102       Sawchuk
    ## 28      Boston Bruins               2              468      Thompson
    ## 29      Boston Bruins               2               67       Winkler
    ## 30      Boston Bruins               2              171       Peeters
    ## 31      Boston Bruins               2               29      Riendeau
    ## 32      Boston Bruins               2               49        Riggin
    ## 33      Boston Bruins               2                1        Romano
    ##    data.losses                                 data.mostGoalsAgainstDates
    ## 1            0                                                 1979-10-23
    ## 2            4                                     1992-03-08, 1992-01-25
    ## 3           14                                                 1995-10-14
    ## 4           15                                                 1993-11-24
    ## 5            1                                                 1997-01-13
    ## 6           73                                     1976-03-09, 1974-10-10
    ## 7            6                                                 1978-04-01
    ## 8            9                         2003-03-03, 2003-02-14, 2003-02-08
    ## 9           46                                                 1985-03-18
    ## 10          62                                                 1990-10-19
    ## 11          75 1992-11-11, 1991-03-02, 1989-12-30, 1989-12-09, 1989-03-09
    ## 12          22                                                 1945-01-27
    ## 13         144                                                 1948-12-29
    ## 14           3                                                 1961-10-22
    ## 15         103 1977-02-12, 1970-01-24, 1966-12-04, 1965-12-15, 1965-12-11
    ## 16           1                                                 1932-02-06
    ## 17           1                                                 1944-01-29
    ## 18          19                                                 1943-11-21
    ## 19          62                                                 1949-10-30
    ## 20           1                                                 1944-03-18
    ## 21          99                                     1953-03-02, 1952-12-11
    ## 22         192                                                 1967-03-15
    ## 23           2                                                 1925-01-31
    ## 24          33                                                 1959-11-14
    ## 25          11                                                 1965-03-18
    ## 26           1                                                 1973-03-07
    ## 27          43                                     1956-02-12, 1955-12-18
    ## 28         153                                     1934-02-24, 1934-01-04
    ## 29          22                                                 1928-03-24
    ## 30          58 1985-11-05, 1985-03-30, 1985-02-23, 1984-02-15, 1984-01-17
    ## 31          12                                                 1994-03-08
    ## 32          16             1986-03-25, 1985-12-31, 1985-12-05, 1985-11-18
    ## 33           1                                                 1987-02-25
    ##    data.mostGoalsAgainstOneGame    data.mostSavesDates
    ## 1                             5             1979-11-02
    ## 2                             4             1992-03-07
    ## 3                             6             1996-01-13
    ## 4                             7             1994-03-14
    ## 5                             4             1997-01-13
    ## 6                             9             1975-02-01
    ## 7                             7             1978-03-28
    ## 8                             5             2003-02-27
    ## 9                             8 1987-10-15, 1983-10-20
    ## 10                            8             1991-12-29
    ## 11                            7             1989-03-05
    ## 12                           11                   <NA>
    ## 13                           10                   <NA>
    ## 14                            9             1961-10-21
    ## 15                            8             1969-02-06
    ## 16                            6                   <NA>
    ## 17                            6                   <NA>
    ## 18                           13                   <NA>
    ## 19                           10                   <NA>
    ## 20                           10                   <NA>
    ## 21                           10                   <NA>
    ## 22                           11             1965-01-03
    ## 23                            8                   <NA>
    ## 24                            8             1958-03-22
    ## 25                           10             1965-03-18
    ## 26                            5             1973-03-15
    ## 27                            7 1956-10-30, 1955-11-12
    ## 28                            9                   <NA>
    ## 29                            7                   <NA>
    ## 30                            7             1984-10-21
    ## 31                            7             1995-03-05
    ## 32                            6             1985-12-12
    ## 33                            6             1987-02-25
    ##    data.mostSavesOneGame         data.mostShotsAgainstDates
    ## 1                     24                         1979-11-02
    ## 2                     26             1992-03-07, 1992-01-23
    ## 3                     34                         1996-01-13
    ## 4                     31                         1994-03-14
    ## 5                     15                         1997-01-13
    ## 6                     41                         1975-02-01
    ## 7                     27             1978-04-01, 1978-03-28
    ## 8                     43                         2003-02-27
    ## 9                     37 1987-11-09, 1985-01-27, 1983-10-20
    ## 10                    39                         1991-12-29
    ## 11                    41                         1991-11-05
    ## 12                    NA                               <NA>
    ## 13                    NA                               <NA>
    ## 14                    37                         1961-10-21
    ## 15                    45                         1969-02-06
    ## 16                    NA                               <NA>
    ## 17                    NA                               <NA>
    ## 18                    NA                               <NA>
    ## 19                    NA                               <NA>
    ## 20                    NA                               <NA>
    ## 21                    NA                               <NA>
    ## 22                    48                         1965-01-03
    ## 23                    NA                               <NA>
    ## 24                    43                         1958-03-22
    ## 25                    47                         1965-03-18
    ## 26                    33                         1973-03-15
    ## 27                    45                         1956-10-30
    ## 28                    NA                               <NA>
    ## 29                    NA                               <NA>
    ## 30                    39                         1985-11-05
    ## 31                    37                         1995-03-05
    ## 32                    41                         1985-12-12
    ## 33                    28                         1987-02-25
    ##    data.mostShotsAgainstOneGame data.mostShutoutsOneSeason
    ## 1                            27                          0
    ## 2                            28                          0
    ## 3                            36                          1
    ## 4                            36                          4
    ## 5                            19                          0
    ## 6                            44                          6
    ## 7                            31                          3
    ## 8                            47                          1
    ## 9                            40                          2
    ## 10                           42                          3
    ## 11                           44                          4
    ## 12                           NA                          2
    ## 13                           NA                         10
    ## 14                           43                          0
    ## 15                           48                          4
    ## 16                           NA                          1
    ## 17                           NA                          0
    ## 18                           NA                          1
    ## 19                           NA                          4
    ## 20                           NA                          0
    ## 21                           NA                          8
    ## 22                           56                          6
    ## 23                           NA                          0
    ## 24                           48                          3
    ## 25                           57                          1
    ## 26                           34                          2
    ## 27                           49                          9
    ## 28                           NA                         12
    ## 29                           NA                         15
    ## 30                           44                          8
    ## 31                           39                          1
    ## 32                           42                          1
    ## 33                           34                          0
    ##    data.mostShutoutsSeasonIds data.mostWinsOneSeason
    ## 1                    19791980                      2
    ## 2                    19911992                      1
    ## 3                    19951996                     10
    ## 4                    19931994                     30
    ## 5                    19961997                      0
    ## 6                    19731974                     34
    ## 7                    19771978                     26
    ## 8                    20022003                      8
    ## 9                    19831984                     19
    ## 10                   19871988                     24
    ## 11                   19901991                     37
    ## 12                   19451946                      8
    ## 13                   19381939                     33
    ## 14                   19611962                      0
    ## 15         19691970, 19791980                     30
    ## 16                   19311932                      1
    ## 17                   19431944                      0
    ## 18                   19431944                     17
    ## 19                   19501951                     22
    ## 20                   19431944                      0
    ## 21         19521953, 19531954                     32
    ## 22                   19631964                     30
    ## 23                   19241925                      0
    ## 24                   19571958                     16
    ## 25                   19641965                     10
    ## 26                   19721973                      7
    ## 27                   19551956                     22
    ## 28                   19281929                     38
    ## 29                   19271928                     20
    ## 30                   19821983                     40
    ## 31                   19931994                      7
    ## 32                   19851986                     17
    ## 33                   19861987                      0
    ##    data.mostWinsSeasonIds data.overtimeLosses data.playerId
    ## 1                19791980                  NA       8445403
    ## 2                19911992                  NA       8445462
    ## 3                19951996                  NA       8445470
    ## 4                19931994                  NA       8446011
    ## 5                19961997                  NA       8446082
    ## 6                19731974                  NA       8447170
    ## 7                19771978                  NA       8447344
    ## 8                20022003                  NA       8447449
    ## 9                19831984                  NA       8448410
    ## 10               19871988                  NA       8448759
    ## 11               19921993                  NA       8449681
    ## 12               19451946                  NA       8449823
    ## 13               19381939                  NA       8449836
    ## 14               19611962                  NA       8449851
    ## 15               19761977                  NA       8449853
    ## 16               19311932                  NA       8449861
    ## 17               19431944                  NA       8449923
    ## 18               19431944                  NA       8449960
    ## 19     19491950, 19501951                  NA       8449977
    ## 20               19431944                  NA       8449983
    ## 21               19531954                  NA       8449993
    ## 22               19701971                  NA       8450005
    ## 23               19241925                  NA       8450017
    ## 24               19591960                  NA       8450019
    ## 25               19641965                  NA       8450054
    ## 26               19721973                  NA       8450066
    ## 27               19551956                  NA       8450111
    ## 28               19291930                  NA       8450127
    ## 29               19271928                  NA       8450150
    ## 30               19821983                  NA       8450343
    ## 31               19931994                  NA       8450834
    ## 32               19851986                  NA       8450835
    ## 33               19861987                  NA       8450993
    ##    data.positionCode data.rookieGamesPlayed data.rookieShutouts
    ## 1                  G                     NA                  NA
    ## 2                  G                     NA                  NA
    ## 3                  G                     NA                  NA
    ## 4                  G                     NA                  NA
    ## 5                  G                     NA                  NA
    ## 6                  G                     NA                  NA
    ## 7                  G                     NA                  NA
    ## 8                  G                     NA                  NA
    ## 9                  G                     NA                  NA
    ## 10                 G                     NA                  NA
    ## 11                 G                     NA                  NA
    ## 12                 G                     NA                  NA
    ## 13                 G                     43                  10
    ## 14                 G                     NA                  NA
    ## 15                 G                     22                   1
    ## 16                 G                     NA                  NA
    ## 17                 G                     NA                  NA
    ## 18                 G                     NA                  NA
    ## 19                 G                     67                   3
    ## 20                 G                     NA                  NA
    ## 21                 G                     NA                  NA
    ## 22                 G                     50                   1
    ## 23                 G                     NA                  NA
    ## 24                 G                     NA                  NA
    ## 25                 G                     23                   1
    ## 26                 G                     NA                  NA
    ## 27                 G                     NA                  NA
    ## 28                 G                     44                  12
    ## 29                 G                     23                   4
    ## 30                 G                     NA                  NA
    ## 31                 G                     NA                  NA
    ## 32                 G                     NA                  NA
    ## 33                 G                     NA                  NA
    ##    data.rookieWins data.seasons data.shutouts data.ties data.wins total
    ## 1               NA            1             0         3         2    51
    ## 2               NA            1             0         2         1    51
    ## 3               NA            2             1         3        15    51
    ## 4               NA            1             4         9        30    51
    ## 5               NA            1             0         0         0    51
    ## 6               NA            7            16        39       155    51
    ## 7               NA            1             3         7        26    51
    ## 8               NA            1             1         0         8    51
    ## 9               NA            5             4        13        83    51
    ## 10              NA            6             6        17        92    51
    ## 11              NA            6            13        36       136    51
    ## 12              NA            2             2         6        14    51
    ## 13              33            9            35        70       230    51
    ## 14              NA            1             0         1         0    51
    ## 15               5           12            26        76       226    51
    ## 16              NA            1             1         0         1    51
    ## 17              NA            1             0         0         0    51
    ## 18              NA            1             1         5        17    51
    ## 19              22            3             7        33        46    51
    ## 20              NA            1             0         0         0    51
    ## 21              NA            4            24        44        93    51
    ## 22              11           11            27        54       182    51
    ## 23              NA            1             0         0         0    51
    ## 24              NA            3             6         9        35    51
    ## 25              10            1             1         2        10    51
    ## 26              NA            1             2         0         7    51
    ## 27              NA            2            11        19        40    51
    ## 28              26           11            74        63       252    51
    ## 29              12            2            19        13        32    51
    ## 30              NA            4             9        16        91    51
    ## 31              NA            2             1         2        10    51
    ## 32              NA            2             1         9        20    51
    ## 33              NA            1             0         0         0    51
    ##  [ reached 'max' / getOption("max.print") -- omitted 18 rows ]

skater function

``` r
skater <- function (id,...){
  base_url <- "https://records.nhl.com/site/api/franchise-skater-records?cayenneExp=franchiseId"
  if (id==0 ) {
get_url <- GET (base_url) 
   record_txt <- content (get_url , "text", encoding = "UTF-8")
   record_json <- fromJSON(record_txt, flatten=TRUE)
   Data_df <- data.frame(record_json)
    return( list(base_url,Data_df))
  }
    else {
      full_url  <-  paste0(base_url,"=",id )    
      get_url <- GET (full_url)
      record_txt <- content (get_url , "text", encoding = "UTF-8")
      record_json <- fromJSON(record_txt, flatten=TRUE)
      Data_df <- data.frame(record_json)
      return( list(full_url,Data_df))
    }
}
record_skater1 <- skater( 6 )
record_skater1
```

    ## [[1]]
    ## [1] "https://records.nhl.com/site/api/franchise-skater-records?cayenneExp=franchiseId=6"
    ## 
    ## [[2]]
    ##    data.id data.activePlayer data.assists data.firstName data.franchiseId
    ## 1    17259             FALSE            0           Rick                6
    ## 2    17335             FALSE            1           John                6
    ## 3    17348             FALSE            3          Barry                6
    ## 4    17357             FALSE            0          Steve                6
    ## 5    17406             FALSE            2         Murray                6
    ## 6    17414             FALSE            0           Stan                6
    ## 7    17488             FALSE            2          Bobby                6
    ## 8    17500             FALSE            0           Fred                6
    ## 9    17511             FALSE            0           Phil                6
    ## 10   17525             FALSE            5            Don                6
    ## 11   17643             FALSE            0           John                6
    ## 12   17644             FALSE            0           Bart                6
    ## 13   17745             FALSE            0            Ron                6
    ## 14   17826             FALSE            1           Gord                6
    ## 15   17839             FALSE            1        Charles                6
    ## 16   17898             FALSE            0         George                6
    ## 17   17915             FALSE            0          Billy                6
    ## 18   17935             FALSE            0           Dick                6
    ## 19   17942             FALSE            0            Art                6
    ## 20   17998             FALSE            0           Paul                6
    ## 21   18062             FALSE            0          Nobby                6
    ## 22   18179             FALSE            0           Norm                6
    ## 23   18196             FALSE            2           Ivan                6
    ## 24   18376             FALSE            0           John                6
    ## 25   18384             FALSE            0            Bob                6
    ## 26   18454             FALSE            0          Terry                6
    ## 27   18501             FALSE            1            Bob                6
    ## 28   18507             FALSE            0         Murray                6
    ## 29   18528             FALSE            0         Armand                6
    ## 30   18535             FALSE            0             Ab                6
    ## 31   18575             FALSE            4           Wade                6
    ##    data.franchiseName data.gameTypeId data.gamesPlayed data.goals
    ## 1       Boston Bruins               2                1          0
    ## 2       Boston Bruins               2                6          0
    ## 3       Boston Bruins               2               14          0
    ## 4       Boston Bruins               2                1          0
    ## 5       Boston Bruins               2               15          0
    ## 6       Boston Bruins               2                7          0
    ## 7       Boston Bruins               2                8          0
    ## 8       Boston Bruins               2                2          0
    ## 9       Boston Bruins               2                8          0
    ## 10      Boston Bruins               2                6          0
    ## 11      Boston Bruins               2                7          0
    ## 12      Boston Bruins               2                1          0
    ## 13      Boston Bruins               2                3          0
    ## 14      Boston Bruins               2                1          0
    ## 15      Boston Bruins               2               32          0
    ## 16      Boston Bruins               2               10          0
    ## 17      Boston Bruins               2                8          0
    ## 18      Boston Bruins               2                6          0
    ## 19      Boston Bruins               2                3          0
    ## 20      Boston Bruins               2               10          0
    ## 21      Boston Bruins               2                2          0
    ## 22      Boston Bruins               2                4          0
    ## 23      Boston Bruins               2               13          0
    ## 24      Boston Bruins               2                1          0
    ## 25      Boston Bruins               2               12          0
    ## 26      Boston Bruins               2                3          0
    ## 27      Boston Bruins               2               41          0
    ## 28      Boston Bruins               2                1          0
    ## 29      Boston Bruins               2                1          0
    ## 30      Boston Bruins               2                3          0
    ## 31      Boston Bruins               2               28          0
    ##     data.lastName
    ## 1         Adduono
    ## 2          Arbour
    ## 3          Ashbee
    ## 4        Atkinson
    ## 5         Balfour
    ## 6          Baluik
    ## 7          Benson
    ## 8       Bergdinon
    ## 9          Besler
    ## 10      Blackburn
    ## 11 Brackenborough
    ## 12        Bradley
    ## 13       Buchanan
    ## 14          Byers
    ## 15         Cahill
    ## 16        Carroll
    ## 17         Carter
    ## 18         Cherry
    ## 19       Chisholm
    ## 20        Beraldo
    ## 21          Clark
    ## 22       Corcoran
    ## 23       Boldirev
    ## 24         Ingram
    ## 25          Blake
    ## 26          Crisp
    ## 27          Davie
    ## 28        Davison
    ## 29       Delmonte
    ## 30    Demarco Jr.
    ## 31       Campbell
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 data.mostAssistsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1975-10-09, 1975-10-12, 1975-10-16, 1975-10-18, 1975-10-19, 1975-10-23, 1975-10-25, 1975-10-26, 1975-10-30, 1975-11-01, 1975-11-02, 1975-11-05, 1975-11-08, 1975-11-09, 1975-11-13, 1975-11-15, 1975-11-16, 1975-11-19, 1975-11-20, 1975-11-23, 1975-11-25, 1975-11-26, 1975-11-29, 1975-11-30, 1975-12-04, 1975-12-06, 1975-12-07, 1975-12-11, 1975-12-13, 1975-12-14, 1975-12-17, 1975-12-20, 1975-12-21, 1975-12-23, 1975-12-26, 1975-12-28, 1975-12-31, 1976-01-02, 1976-01-03, 1976-01-10, 1976-01-11, 1976-01-13, 1976-01-15, 1976-01-17, 1976-01-22, 1976-01-24, 1976-01-25, 1976-01-29, 1976-01-30, 1976-02-01, 1976-02-05, 1976-02-07, 1976-02-08, 1976-02-11, 1976-02-13, 1976-02-15, 1976-02-18, 1976-02-21, 1976-02-22, 1976-02-26, 1976-02-27, 1976-02-29, 1976-03-03, 1976-03-05, 1976-03-07, 1976-03-09, 1976-03-11, 1976-03-13, 1976-03-14, 1976-03-16, 1976-03-18, 1976-03-20, 1976-03-24, 1976-03-25, 1976-03-27, 1976-03-28, 1976-03-30, 1976-04-01, 1976-04-03, 1976-04-04
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1968-01-21
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1965-11-25, 1965-12-01, 1965-12-15
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1968-10-11, 1968-10-13, 1968-10-16, 1968-10-17, 1968-10-19, 1968-10-24, 1968-10-26, 1968-10-27, 1968-10-30, 1968-10-31, 1968-11-03, 1968-11-06, 1968-11-10, 1968-11-13, 1968-11-14, 1968-11-17, 1968-11-21, 1968-11-23, 1968-11-24, 1968-11-27, 1968-11-30, 1968-12-01, 1968-12-05, 1968-12-07, 1968-12-08, 1968-12-11, 1968-12-14, 1968-12-15, 1968-12-19, 1968-12-21, 1968-12-22, 1968-12-25, 1968-12-28, 1968-12-29, 1969-01-02, 1969-01-04, 1969-01-09, 1969-01-11, 1969-01-12, 1969-01-15, 1969-01-16, 1969-01-18, 1969-01-19, 1969-01-23, 1969-01-25, 1969-01-26, 1969-01-29, 1969-01-30, 1969-02-02, 1969-02-05, 1969-02-06, 1969-02-08, 1969-02-09, 1969-02-11, 1969-02-15, 1969-02-16, 1969-02-19, 1969-02-23, 1969-02-26, 1969-02-27, 1969-03-01, 1969-03-02, 1969-03-05, 1969-03-08, 1969-03-09, 1969-03-11, 1969-03-13, 1969-03-15, 1969-03-16, 1969-03-19, 1969-03-20, 1969-03-22, 1969-03-23, 1969-03-27, 1969-03-29, 1969-03-30
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1964-10-17, 1964-11-01
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1959-10-08, 1959-10-10, 1959-10-11, 1959-10-14, 1959-10-17, 1959-10-18, 1959-10-22, 1959-10-24, 1959-10-29, 1959-10-31, 1959-11-01, 1959-11-03, 1959-11-05, 1959-11-08, 1959-11-11, 1959-11-12, 1959-11-14, 1959-11-15, 1959-11-21, 1959-11-22, 1959-11-25, 1959-11-26, 1959-11-28, 1959-11-29, 1959-12-02, 1959-12-05, 1959-12-06, 1959-12-10, 1959-12-12, 1959-12-13, 1959-12-16, 1959-12-20, 1959-12-25, 1959-12-27, 1959-12-29, 1960-01-01, 1960-01-02, 1960-01-03, 1960-01-07, 1960-01-09, 1960-01-10, 1960-01-14, 1960-01-16, 1960-01-17, 1960-01-20, 1960-01-21, 1960-01-23, 1960-01-24, 1960-01-30, 1960-01-31, 1960-02-04, 1960-02-06, 1960-02-07, 1960-02-11, 1960-02-13, 1960-02-14, 1960-02-17, 1960-02-20, 1960-02-21, 1960-02-27, 1960-03-01, 1960-03-03, 1960-03-05, 1960-03-06, 1960-03-10, 1960-03-12, 1960-03-13, 1960-03-16, 1960-03-19, 1960-03-20
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1925-01-10, 1925-01-24
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1963-01-10
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1966-10-19, 1966-10-22, 1966-10-23, 1966-10-29, 1966-10-30, 1966-11-01, 1966-11-03, 1966-11-06, 1966-11-09, 1966-11-10, 1966-11-13, 1966-11-19, 1966-11-20, 1966-11-23, 1966-11-24, 1966-11-26, 1966-11-27, 1966-12-01, 1966-12-03, 1966-12-04, 1966-12-07, 1966-12-08, 1966-12-11, 1966-12-14, 1966-12-15, 1966-12-18, 1966-12-21, 1966-12-24, 1966-12-25, 1966-12-27, 1966-12-28, 1966-12-31, 1967-01-01, 1967-01-07, 1967-01-08, 1967-01-12, 1967-01-14, 1967-01-15, 1967-01-19, 1967-01-21, 1967-01-22, 1967-01-25, 1967-01-26, 1967-01-29, 1967-02-01, 1967-02-02, 1967-02-04, 1967-02-05, 1967-02-08, 1967-02-11, 1967-02-12, 1967-02-14, 1967-02-16, 1967-02-18, 1967-02-23, 1967-02-25, 1967-02-26, 1967-03-02, 1967-03-04, 1967-03-05, 1967-03-08, 1967-03-12, 1967-03-15, 1967-03-18, 1967-03-19, 1967-03-23, 1967-03-25, 1967-03-26, 1967-03-30, 1967-04-02
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1950-03-22
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1925-12-19
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-17, 1925-01-20, 1925-01-31
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1960-10-05, 1960-10-08, 1960-10-09, 1960-10-11, 1960-10-15, 1960-10-16, 1960-10-20, 1960-10-23, 1960-10-27, 1960-10-29, 1960-10-30, 1960-11-02, 1960-11-03, 1960-11-06, 1960-11-10, 1960-11-13, 1960-11-16, 1960-11-17, 1960-11-19, 1960-11-20, 1960-11-23, 1960-11-24, 1960-11-27, 1960-11-30, 1960-12-01, 1960-12-03, 1960-12-04, 1960-12-08, 1960-12-10, 1960-12-11, 1960-12-17, 1960-12-18, 1960-12-22, 1960-12-25, 1960-12-28, 1960-12-31, 1961-01-01, 1961-01-05, 1961-01-07, 1961-01-08, 1961-01-12, 1961-01-14, 1961-01-15, 1961-01-19, 1961-01-21, 1961-01-22, 1961-01-25, 1961-01-26, 1961-01-29, 1961-02-02, 1961-02-04, 1961-02-05, 1961-02-09, 1961-02-11, 1961-02-12, 1961-02-16, 1961-02-18, 1961-02-19, 1961-02-23, 1961-02-26, 1961-03-01, 1961-03-02, 1961-03-05, 1961-03-07, 1961-03-09, 1961-03-11, 1961-03-12, 1961-03-15, 1961-03-18, 1961-03-19
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1956-10-11, 1956-10-13, 1956-10-14, 1956-10-17, 1956-10-20, 1956-10-21, 1956-10-27, 1956-10-30, 1956-11-01, 1956-11-04, 1956-11-07, 1956-11-08, 1956-11-10, 1956-11-11, 1956-11-15, 1956-11-17, 1956-11-18, 1956-11-22, 1956-11-24, 1956-11-25, 1956-11-28, 1956-11-29, 1956-12-02, 1956-12-06, 1956-12-08, 1956-12-09, 1956-12-13, 1956-12-15, 1956-12-16, 1956-12-20, 1956-12-22, 1956-12-23, 1956-12-25, 1956-12-27, 1956-12-30, 1957-01-01, 1957-01-05, 1957-01-06, 1957-01-10, 1957-01-12, 1957-01-13, 1957-01-17, 1957-01-19, 1957-01-20, 1957-01-26, 1957-01-27, 1957-01-31, 1957-02-02, 1957-02-03, 1957-02-06, 1957-02-07, 1957-02-09, 1957-02-10, 1957-02-13, 1957-02-16, 1957-02-17, 1957-02-20, 1957-02-23, 1957-02-24, 1957-02-28, 1957-03-02, 1957-03-03, 1957-03-07, 1957-03-09, 1957-03-10, 1957-03-13, 1957-03-16, 1957-03-17, 1957-03-21, 1957-03-23
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1960-10-05, 1960-10-08, 1960-10-09, 1960-10-11, 1960-10-15, 1960-10-16, 1960-10-20, 1960-10-23, 1960-10-27, 1960-10-29, 1960-10-30, 1960-11-02, 1960-11-03, 1960-11-06, 1960-11-10, 1960-11-13, 1960-11-16, 1960-11-17, 1960-11-19, 1960-11-20, 1960-11-23, 1960-11-24, 1960-11-27, 1960-11-30, 1960-12-01, 1960-12-03, 1960-12-04, 1960-12-08, 1960-12-10, 1960-12-11, 1960-12-17, 1960-12-18, 1960-12-22, 1960-12-25, 1960-12-28, 1960-12-31, 1961-01-01, 1961-01-05, 1961-01-07, 1961-01-08, 1961-01-12, 1961-01-14, 1961-01-15, 1961-01-19, 1961-01-21, 1961-01-22, 1961-01-25, 1961-01-26, 1961-01-29, 1961-02-02, 1961-02-04, 1961-02-05, 1961-02-09, 1961-02-11, 1961-02-12, 1961-02-16, 1961-02-18, 1961-02-19, 1961-02-23, 1961-02-26, 1961-03-01, 1961-03-02, 1961-03-05, 1961-03-07, 1961-03-09, 1961-03-11, 1961-03-12, 1961-03-15, 1961-03-18, 1961-03-19
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1988-01-30, 1988-02-01, 1988-02-04, 1988-02-06, 1988-02-07, 1988-02-12, 1988-02-13, 1988-02-17, 1988-02-21, 1988-02-23, 1988-02-25, 1988-02-27, 1988-03-03, 1988-03-05, 1988-03-06, 1988-03-08, 1988-03-10, 1988-03-12, 1988-03-13, 1988-03-17, 1988-03-19, 1988-03-20, 1988-03-22, 1988-03-24, 1988-03-26, 1988-03-31, 1988-04-02, 1988-04-03, 1988-12-06, 1988-12-08, 1988-12-10, 1988-12-12, 1988-12-15, 1988-12-17, 1988-12-18, 1988-12-21, 1988-12-22, 1988-12-26, 1988-12-29, 1989-01-02, 1989-01-05, 1989-01-07, 1989-01-08, 1989-01-12, 1989-01-14, 1989-01-15, 1989-01-19, 1989-01-21, 1989-01-22, 1989-01-25, 1989-01-26, 1989-01-28, 1989-02-01, 1989-02-03, 1989-02-05, 1989-02-09, 1989-02-11, 1989-02-14, 1989-02-15, 1989-02-18, 1989-02-19, 1989-02-25, 1989-02-28, 1989-03-02, 1989-03-04, 1989-03-05, 1989-03-07, 1989-03-09, 1989-03-11, 1989-03-12, 1989-03-14, 1989-03-16, 1989-03-18, 1989-03-19, 1989-03-22, 1989-03-23, 1989-03-25, 1989-03-27, 1989-04-01, 1989-04-02
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1927-11-15, 1927-11-19, 1927-11-22, 1927-11-26, 1927-11-27, 1927-11-29, 1927-12-01, 1927-12-03, 1927-12-06, 1927-12-10, 1927-12-11, 1927-12-13, 1927-12-17, 1927-12-20, 1927-12-27, 1927-12-29, 1928-01-01, 1928-01-03, 1928-01-07, 1928-01-10, 1928-01-12, 1928-01-14, 1928-01-17, 1928-01-21, 1928-01-22, 1928-01-24, 1928-01-28, 1928-01-31, 1928-02-07, 1928-02-11, 1928-02-14, 1928-02-19, 1928-02-21, 1928-02-25, 1928-02-28, 1928-03-03, 1928-03-06, 1928-03-10, 1928-03-11, 1928-03-13, 1928-03-15, 1928-03-17, 1928-03-20, 1928-03-24
    ## 22 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26, 1952-10-12, 1952-10-16, 1952-10-18, 1952-10-19, 1952-10-22, 1952-10-25, 1952-10-26, 1952-10-30, 1952-11-01, 1952-11-02, 1952-11-06, 1952-11-09, 1952-11-11, 1952-11-13, 1952-11-15, 1952-11-16, 1952-11-19, 1952-11-20, 1952-11-23, 1952-11-27, 1952-11-30, 1952-12-04, 1952-12-06, 1952-12-07, 1952-12-10, 1952-12-11, 1952-12-14, 1952-12-17, 1952-12-18, 1952-12-20, 1952-12-21, 1952-12-25, 1952-12-27, 1952-12-28, 1953-01-01, 1953-01-03, 1953-01-04, 1953-01-08, 1953-01-10, 1953-01-11, 1953-01-15, 1953-01-18, 1953-01-22, 1953-01-24, 1953-01-25, 1953-01-29, 1953-01-31, 1953-02-01, 1953-02-05, 1953-02-08, 1953-02-12, 1953-02-14, 1953-02-15, 1953-02-18, 1953-02-21, 1953-02-22, 1953-02-25, 1953-02-27, 1953-03-01, 1953-03-02, 1953-03-05, 1953-03-07, 1953-03-08, 1953-03-12, 1953-03-14, 1953-03-15, 1953-03-18, 1953-03-19, 1953-03-21, 1953-03-22, 1954-10-09, 1954-10-11, 1954-10-14, 1954-10-17, 1954-10-20, 1954-10-21, 1954-10-23, 1954-10-30, 1954-11-04, 1954-11-07, 1954-11-10, 1954-11-13, 1954-11-14, 1954-11-17, 1954-11-18, 1954-11-20, 1954-11-21, 1954-11-24, 1954-11-25, 1954-11-28, 1954-12-01, 1954-12-02, 1954-12-04, 1954-12-05, 1954-12-09, 1954-12-11, 1954-12-12, 1954-12-16, 1954-12-18, 1954-12-19, 1954-12-25, 1954-12-30, 1955-01-01, 1955-01-02, 1955-01-05, 1955-01-06, 1955-01-08, 1955-01-09, 1955-01-12, 1955-01-13, 1955-01-15, 1955-01-16, 1955-01-20, 1955-01-22, 1955-01-23, 1955-01-27, 1955-01-29, 1955-01-30, 1955-02-02, 1955-02-03, 1955-02-05, 1955-02-06, 1955-02-10, 1955-02-12, 1955-02-13, 1955-02-16, 1955-02-17, 1955-02-19, 1955-02-21, 1955-02-23, 1955-02-26, 1955-03-02, 1955-03-03, 1955-03-05, 1955-03-06, 1955-03-10, 1955-03-12, 1955-03-13, 1955-03-16, 1955-03-20
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1971-11-04
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1924-12-01, 1924-12-03, 1924-12-08, 1924-12-10, 1924-12-15, 1924-12-17, 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-12, 1925-01-17, 1925-01-20, 1925-01-24, 1925-01-27, 1925-01-31, 1925-02-03, 1925-02-07, 1925-02-10, 1925-02-14, 1925-02-17, 1925-02-21, 1925-02-24, 1925-02-28, 1925-03-03, 1925-03-07, 1925-03-09
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 27                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1935-02-10
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1945-10-24, 1945-10-27, 1945-10-28, 1945-11-04, 1945-11-07, 1945-11-10, 1945-11-11, 1945-11-21, 1945-11-25, 1945-11-28, 1945-12-02, 1945-12-05, 1945-12-09, 1945-12-12, 1945-12-15, 1945-12-16, 1945-12-19, 1945-12-23, 1945-12-29, 1945-12-30, 1946-01-01, 1946-01-05, 1946-01-06, 1946-01-10, 1946-01-12, 1946-01-16, 1946-01-17, 1946-01-19, 1946-01-20, 1946-01-23, 1946-01-26, 1946-01-27, 1946-01-30, 1946-02-02, 1946-02-03, 1946-02-06, 1946-02-10, 1946-02-13, 1946-02-14, 1946-02-16, 1946-02-20, 1946-02-23, 1946-02-24, 1946-02-27, 1946-03-03, 1946-03-06, 1946-03-10, 1946-03-12, 1946-03-13, 1946-03-17
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1978-10-12, 1978-10-14, 1978-10-15, 1978-10-18, 1978-10-20, 1978-10-22, 1978-10-24, 1978-10-25, 1978-10-28, 1978-11-02, 1978-11-04, 1978-11-05, 1978-11-09, 1978-11-11, 1978-11-12, 1978-11-16, 1978-11-17, 1978-11-19, 1978-11-23, 1978-11-25, 1978-11-26, 1978-11-30, 1978-12-02, 1978-12-03, 1978-12-05, 1978-12-07, 1978-12-09, 1978-12-10, 1978-12-12, 1978-12-14, 1978-12-16, 1978-12-17, 1978-12-21, 1978-12-23, 1978-12-27, 1978-12-30, 1978-12-31, 1979-01-03, 1979-01-05, 1979-01-06, 1979-01-11, 1979-01-13, 1979-01-14, 1979-01-16, 1979-01-18, 1979-01-20, 1979-01-22, 1979-01-25, 1979-01-27, 1979-01-28, 1979-01-31, 1979-02-01, 1979-02-03, 1979-02-04, 1979-02-14, 1979-02-15, 1979-02-17, 1979-02-20, 1979-02-21, 1979-02-24, 1979-02-27, 1979-03-01, 1979-03-03, 1979-03-04, 1979-03-08, 1979-03-10, 1979-03-11, 1979-03-13, 1979-03-15, 1979-03-17, 1979-03-19, 1979-03-22, 1979-03-24, 1979-03-28, 1979-03-29, 1979-03-31, 1979-04-01, 1979-04-04, 1979-04-05, 1979-04-08
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1987-03-05, 1987-03-14, 1987-03-19, 1988-03-10
    ##    data.mostAssistsOneGame data.mostAssistsOneSeason
    ## 1                        0                         0
    ## 2                        1                         1
    ## 3                        1                         3
    ## 4                        0                         0
    ## 5                        1                         2
    ## 6                        0                         0
    ## 7                        1                         2
    ## 8                        0                         0
    ## 9                        0                         0
    ## 10                       2                         5
    ## 11                       0                         0
    ## 12                       0                         0
    ## 13                       0                         0
    ## 14                       1                         1
    ## 15                       1                         1
    ## 16                       0                         0
    ## 17                       0                         0
    ## 18                       0                         0
    ## 19                       0                         0
    ## 20                       0                         0
    ## 21                       0                         0
    ## 22                       0                         0
    ## 23                       2                         2
    ## 24                       0                         0
    ## 25                       0                         0
    ## 26                       0                         0
    ## 27                       1                         1
    ## 28                       0                         0
    ## 29                       0                         0
    ## 30                       0                         0
    ## 31                       1                         3
    ##       data.mostAssistsSeasonIds
    ## 1                      19751976
    ## 2                      19671968
    ## 3                      19651966
    ## 4                      19681969
    ## 5                      19641965
    ## 6                      19591960
    ## 7                      19241925
    ## 8                      19251926
    ## 9                      19351936
    ## 10                     19621963
    ## 11                     19251926
    ## 12                     19491950
    ## 13                     19661967
    ## 14                     19491950
    ## 15                     19251926
    ## 16                     19241925
    ## 17                     19601961
    ## 18                     19561957
    ## 19                     19601961
    ## 20           19871988, 19881989
    ## 21                     19271928
    ## 22 19491950, 19521953, 19541955
    ## 23                     19711972
    ## 24                     19241925
    ## 25                     19351936
    ## 26                     19651966
    ## 27                     19341935
    ## 28                     19651966
    ## 29                     19451946
    ## 30                     19781979
    ## 31                     19861987
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   data.mostGoalsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1975-10-09, 1975-10-12, 1975-10-16, 1975-10-18, 1975-10-19, 1975-10-23, 1975-10-25, 1975-10-26, 1975-10-30, 1975-11-01, 1975-11-02, 1975-11-05, 1975-11-08, 1975-11-09, 1975-11-13, 1975-11-15, 1975-11-16, 1975-11-19, 1975-11-20, 1975-11-23, 1975-11-25, 1975-11-26, 1975-11-29, 1975-11-30, 1975-12-04, 1975-12-06, 1975-12-07, 1975-12-11, 1975-12-13, 1975-12-14, 1975-12-17, 1975-12-20, 1975-12-21, 1975-12-23, 1975-12-26, 1975-12-28, 1975-12-31, 1976-01-02, 1976-01-03, 1976-01-10, 1976-01-11, 1976-01-13, 1976-01-15, 1976-01-17, 1976-01-22, 1976-01-24, 1976-01-25, 1976-01-29, 1976-01-30, 1976-02-01, 1976-02-05, 1976-02-07, 1976-02-08, 1976-02-11, 1976-02-13, 1976-02-15, 1976-02-18, 1976-02-21, 1976-02-22, 1976-02-26, 1976-02-27, 1976-02-29, 1976-03-03, 1976-03-05, 1976-03-07, 1976-03-09, 1976-03-11, 1976-03-13, 1976-03-14, 1976-03-16, 1976-03-18, 1976-03-20, 1976-03-24, 1976-03-25, 1976-03-27, 1976-03-28, 1976-03-30, 1976-04-01, 1976-04-03, 1976-04-04
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03, 1967-10-11, 1967-10-15, 1967-10-18, 1967-10-19, 1967-10-21, 1967-10-26, 1967-10-29, 1967-11-01, 1967-11-05, 1967-11-08, 1967-11-11, 1967-11-12, 1967-11-15, 1967-11-18, 1967-11-19, 1967-11-22, 1967-11-23, 1967-11-25, 1967-11-26, 1967-11-29, 1967-12-02, 1967-12-03, 1967-12-07, 1967-12-09, 1967-12-10, 1967-12-13, 1967-12-15, 1967-12-16, 1967-12-20, 1967-12-23, 1967-12-25, 1967-12-27, 1967-12-30, 1967-12-31, 1968-01-03, 1968-01-04, 1968-01-06, 1968-01-07, 1968-01-11, 1968-01-13, 1968-01-14, 1968-01-18, 1968-01-20, 1968-01-21, 1968-01-24, 1968-01-25, 1968-01-27, 1968-01-28, 1968-02-01, 1968-02-03, 1968-02-04, 1968-02-07, 1968-02-10, 1968-02-11, 1968-02-14, 1968-02-17, 1968-02-18, 1968-02-21, 1968-02-22, 1968-02-24, 1968-02-27, 1968-02-29, 1968-03-03, 1968-03-06, 1968-03-07, 1968-03-10, 1968-03-13, 1968-03-16, 1968-03-17, 1968-03-21, 1968-03-24, 1968-03-28, 1968-03-30, 1968-03-31
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1968-10-11, 1968-10-13, 1968-10-16, 1968-10-17, 1968-10-19, 1968-10-24, 1968-10-26, 1968-10-27, 1968-10-30, 1968-10-31, 1968-11-03, 1968-11-06, 1968-11-10, 1968-11-13, 1968-11-14, 1968-11-17, 1968-11-21, 1968-11-23, 1968-11-24, 1968-11-27, 1968-11-30, 1968-12-01, 1968-12-05, 1968-12-07, 1968-12-08, 1968-12-11, 1968-12-14, 1968-12-15, 1968-12-19, 1968-12-21, 1968-12-22, 1968-12-25, 1968-12-28, 1968-12-29, 1969-01-02, 1969-01-04, 1969-01-09, 1969-01-11, 1969-01-12, 1969-01-15, 1969-01-16, 1969-01-18, 1969-01-19, 1969-01-23, 1969-01-25, 1969-01-26, 1969-01-29, 1969-01-30, 1969-02-02, 1969-02-05, 1969-02-06, 1969-02-08, 1969-02-09, 1969-02-11, 1969-02-15, 1969-02-16, 1969-02-19, 1969-02-23, 1969-02-26, 1969-02-27, 1969-03-01, 1969-03-02, 1969-03-05, 1969-03-08, 1969-03-09, 1969-03-11, 1969-03-13, 1969-03-15, 1969-03-16, 1969-03-19, 1969-03-20, 1969-03-22, 1969-03-23, 1969-03-27, 1969-03-29, 1969-03-30
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1964-10-12, 1964-10-14, 1964-10-17, 1964-10-18, 1964-10-22, 1964-10-25, 1964-10-28, 1964-10-29, 1964-10-31, 1964-11-01, 1964-11-08, 1964-11-10, 1964-11-11, 1964-11-14, 1964-11-15, 1964-11-21, 1964-11-22, 1964-11-26, 1964-11-28, 1964-11-29, 1964-12-03, 1964-12-05, 1964-12-10, 1964-12-12, 1964-12-13, 1964-12-16, 1964-12-17, 1964-12-20, 1964-12-25, 1964-12-26, 1964-12-27, 1965-01-01, 1965-01-02, 1965-01-03, 1965-01-06, 1965-01-07, 1965-01-09, 1965-01-14, 1965-01-16, 1965-01-17, 1965-01-20, 1965-01-21, 1965-01-23, 1965-01-24, 1965-01-27, 1965-01-28, 1965-01-30, 1965-01-31, 1965-02-04, 1965-02-06, 1965-02-07, 1965-02-11, 1965-02-13, 1965-02-14, 1965-02-20, 1965-02-21, 1965-02-24, 1965-02-27, 1965-02-28, 1965-03-03, 1965-03-04, 1965-03-06, 1965-03-07, 1965-03-13, 1965-03-14, 1965-03-17, 1965-03-18, 1965-03-21, 1965-03-27, 1965-03-28
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1959-10-08, 1959-10-10, 1959-10-11, 1959-10-14, 1959-10-17, 1959-10-18, 1959-10-22, 1959-10-24, 1959-10-29, 1959-10-31, 1959-11-01, 1959-11-03, 1959-11-05, 1959-11-08, 1959-11-11, 1959-11-12, 1959-11-14, 1959-11-15, 1959-11-21, 1959-11-22, 1959-11-25, 1959-11-26, 1959-11-28, 1959-11-29, 1959-12-02, 1959-12-05, 1959-12-06, 1959-12-10, 1959-12-12, 1959-12-13, 1959-12-16, 1959-12-20, 1959-12-25, 1959-12-27, 1959-12-29, 1960-01-01, 1960-01-02, 1960-01-03, 1960-01-07, 1960-01-09, 1960-01-10, 1960-01-14, 1960-01-16, 1960-01-17, 1960-01-20, 1960-01-21, 1960-01-23, 1960-01-24, 1960-01-30, 1960-01-31, 1960-02-04, 1960-02-06, 1960-02-07, 1960-02-11, 1960-02-13, 1960-02-14, 1960-02-17, 1960-02-20, 1960-02-21, 1960-02-27, 1960-03-01, 1960-03-03, 1960-03-05, 1960-03-06, 1960-03-10, 1960-03-12, 1960-03-13, 1960-03-16, 1960-03-19, 1960-03-20
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1924-12-01, 1924-12-03, 1924-12-08, 1924-12-10, 1924-12-15, 1924-12-17, 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-12, 1925-01-17, 1925-01-20, 1925-01-24, 1925-01-27, 1925-01-31, 1925-02-03, 1925-02-07, 1925-02-10, 1925-02-14, 1925-02-17, 1925-02-21, 1925-02-24, 1925-02-28, 1925-03-03, 1925-03-07, 1925-03-09
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1962-10-11, 1962-10-13, 1962-10-14, 1962-10-18, 1962-10-20, 1962-10-21, 1962-10-25, 1962-11-01, 1962-11-04, 1962-11-07, 1962-11-10, 1962-11-11, 1962-11-14, 1962-11-18, 1962-11-21, 1962-11-22, 1962-11-24, 1962-11-25, 1962-11-29, 1962-12-01, 1962-12-02, 1962-12-05, 1962-12-06, 1962-12-08, 1962-12-09, 1962-12-13, 1962-12-15, 1962-12-16, 1962-12-19, 1962-12-20, 1962-12-23, 1962-12-25, 1962-12-27, 1962-12-30, 1963-01-01, 1963-01-03, 1963-01-05, 1963-01-06, 1963-01-10, 1963-01-12, 1963-01-13, 1963-01-16, 1963-01-17, 1963-01-19, 1963-01-20, 1963-01-24, 1963-01-26, 1963-01-27, 1963-01-31, 1963-02-02, 1963-02-03, 1963-02-07, 1963-02-10, 1963-02-12, 1963-02-14, 1963-02-16, 1963-02-17, 1963-02-20, 1963-02-23, 1963-02-24, 1963-02-28, 1963-03-03, 1963-03-06, 1963-03-07, 1963-03-10, 1963-03-14, 1963-03-17, 1963-03-20, 1963-03-21, 1963-03-24
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1966-10-19, 1966-10-22, 1966-10-23, 1966-10-29, 1966-10-30, 1966-11-01, 1966-11-03, 1966-11-06, 1966-11-09, 1966-11-10, 1966-11-13, 1966-11-19, 1966-11-20, 1966-11-23, 1966-11-24, 1966-11-26, 1966-11-27, 1966-12-01, 1966-12-03, 1966-12-04, 1966-12-07, 1966-12-08, 1966-12-11, 1966-12-14, 1966-12-15, 1966-12-18, 1966-12-21, 1966-12-24, 1966-12-25, 1966-12-27, 1966-12-28, 1966-12-31, 1967-01-01, 1967-01-07, 1967-01-08, 1967-01-12, 1967-01-14, 1967-01-15, 1967-01-19, 1967-01-21, 1967-01-22, 1967-01-25, 1967-01-26, 1967-01-29, 1967-02-01, 1967-02-02, 1967-02-04, 1967-02-05, 1967-02-08, 1967-02-11, 1967-02-12, 1967-02-14, 1967-02-16, 1967-02-18, 1967-02-23, 1967-02-25, 1967-02-26, 1967-03-02, 1967-03-04, 1967-03-05, 1967-03-08, 1967-03-12, 1967-03-15, 1967-03-18, 1967-03-19, 1967-03-23, 1967-03-25, 1967-03-26, 1967-03-30, 1967-04-02
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16, 1926-11-16, 1926-11-18, 1926-11-20, 1926-11-23, 1926-11-30, 1926-12-04, 1926-12-07, 1926-12-12, 1926-12-14, 1926-12-16, 1926-12-18, 1926-12-21, 1926-12-23, 1926-12-28, 1926-12-30, 1927-01-02, 1927-01-04, 1927-01-08, 1927-01-11, 1927-01-13, 1927-01-15, 1927-01-18, 1927-01-20, 1927-01-22, 1927-01-25, 1927-01-29, 1927-02-01, 1927-02-05, 1927-02-08, 1927-02-12, 1927-02-15, 1927-02-20, 1927-02-22, 1927-02-26, 1927-03-01, 1927-03-05, 1927-03-08, 1927-03-13, 1927-03-15, 1927-03-17, 1927-03-19, 1927-03-22, 1927-03-24, 1927-03-26
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-17, 1925-01-20, 1925-01-31
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1960-10-05, 1960-10-08, 1960-10-09, 1960-10-11, 1960-10-15, 1960-10-16, 1960-10-20, 1960-10-23, 1960-10-27, 1960-10-29, 1960-10-30, 1960-11-02, 1960-11-03, 1960-11-06, 1960-11-10, 1960-11-13, 1960-11-16, 1960-11-17, 1960-11-19, 1960-11-20, 1960-11-23, 1960-11-24, 1960-11-27, 1960-11-30, 1960-12-01, 1960-12-03, 1960-12-04, 1960-12-08, 1960-12-10, 1960-12-11, 1960-12-17, 1960-12-18, 1960-12-22, 1960-12-25, 1960-12-28, 1960-12-31, 1961-01-01, 1961-01-05, 1961-01-07, 1961-01-08, 1961-01-12, 1961-01-14, 1961-01-15, 1961-01-19, 1961-01-21, 1961-01-22, 1961-01-25, 1961-01-26, 1961-01-29, 1961-02-02, 1961-02-04, 1961-02-05, 1961-02-09, 1961-02-11, 1961-02-12, 1961-02-16, 1961-02-18, 1961-02-19, 1961-02-23, 1961-02-26, 1961-03-01, 1961-03-02, 1961-03-05, 1961-03-07, 1961-03-09, 1961-03-11, 1961-03-12, 1961-03-15, 1961-03-18, 1961-03-19
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1956-10-11, 1956-10-13, 1956-10-14, 1956-10-17, 1956-10-20, 1956-10-21, 1956-10-27, 1956-10-30, 1956-11-01, 1956-11-04, 1956-11-07, 1956-11-08, 1956-11-10, 1956-11-11, 1956-11-15, 1956-11-17, 1956-11-18, 1956-11-22, 1956-11-24, 1956-11-25, 1956-11-28, 1956-11-29, 1956-12-02, 1956-12-06, 1956-12-08, 1956-12-09, 1956-12-13, 1956-12-15, 1956-12-16, 1956-12-20, 1956-12-22, 1956-12-23, 1956-12-25, 1956-12-27, 1956-12-30, 1957-01-01, 1957-01-05, 1957-01-06, 1957-01-10, 1957-01-12, 1957-01-13, 1957-01-17, 1957-01-19, 1957-01-20, 1957-01-26, 1957-01-27, 1957-01-31, 1957-02-02, 1957-02-03, 1957-02-06, 1957-02-07, 1957-02-09, 1957-02-10, 1957-02-13, 1957-02-16, 1957-02-17, 1957-02-20, 1957-02-23, 1957-02-24, 1957-02-28, 1957-03-02, 1957-03-03, 1957-03-07, 1957-03-09, 1957-03-10, 1957-03-13, 1957-03-16, 1957-03-17, 1957-03-21, 1957-03-23
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1960-10-05, 1960-10-08, 1960-10-09, 1960-10-11, 1960-10-15, 1960-10-16, 1960-10-20, 1960-10-23, 1960-10-27, 1960-10-29, 1960-10-30, 1960-11-02, 1960-11-03, 1960-11-06, 1960-11-10, 1960-11-13, 1960-11-16, 1960-11-17, 1960-11-19, 1960-11-20, 1960-11-23, 1960-11-24, 1960-11-27, 1960-11-30, 1960-12-01, 1960-12-03, 1960-12-04, 1960-12-08, 1960-12-10, 1960-12-11, 1960-12-17, 1960-12-18, 1960-12-22, 1960-12-25, 1960-12-28, 1960-12-31, 1961-01-01, 1961-01-05, 1961-01-07, 1961-01-08, 1961-01-12, 1961-01-14, 1961-01-15, 1961-01-19, 1961-01-21, 1961-01-22, 1961-01-25, 1961-01-26, 1961-01-29, 1961-02-02, 1961-02-04, 1961-02-05, 1961-02-09, 1961-02-11, 1961-02-12, 1961-02-16, 1961-02-18, 1961-02-19, 1961-02-23, 1961-02-26, 1961-03-01, 1961-03-02, 1961-03-05, 1961-03-07, 1961-03-09, 1961-03-11, 1961-03-12, 1961-03-15, 1961-03-18, 1961-03-19
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1988-01-30, 1988-02-01, 1988-02-04, 1988-02-06, 1988-02-07, 1988-02-12, 1988-02-13, 1988-02-17, 1988-02-21, 1988-02-23, 1988-02-25, 1988-02-27, 1988-03-03, 1988-03-05, 1988-03-06, 1988-03-08, 1988-03-10, 1988-03-12, 1988-03-13, 1988-03-17, 1988-03-19, 1988-03-20, 1988-03-22, 1988-03-24, 1988-03-26, 1988-03-31, 1988-04-02, 1988-04-03, 1988-12-06, 1988-12-08, 1988-12-10, 1988-12-12, 1988-12-15, 1988-12-17, 1988-12-18, 1988-12-21, 1988-12-22, 1988-12-26, 1988-12-29, 1989-01-02, 1989-01-05, 1989-01-07, 1989-01-08, 1989-01-12, 1989-01-14, 1989-01-15, 1989-01-19, 1989-01-21, 1989-01-22, 1989-01-25, 1989-01-26, 1989-01-28, 1989-02-01, 1989-02-03, 1989-02-05, 1989-02-09, 1989-02-11, 1989-02-14, 1989-02-15, 1989-02-18, 1989-02-19, 1989-02-25, 1989-02-28, 1989-03-02, 1989-03-04, 1989-03-05, 1989-03-07, 1989-03-09, 1989-03-11, 1989-03-12, 1989-03-14, 1989-03-16, 1989-03-18, 1989-03-19, 1989-03-22, 1989-03-23, 1989-03-25, 1989-03-27, 1989-04-01, 1989-04-02
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1927-11-15, 1927-11-19, 1927-11-22, 1927-11-26, 1927-11-27, 1927-11-29, 1927-12-01, 1927-12-03, 1927-12-06, 1927-12-10, 1927-12-11, 1927-12-13, 1927-12-17, 1927-12-20, 1927-12-27, 1927-12-29, 1928-01-01, 1928-01-03, 1928-01-07, 1928-01-10, 1928-01-12, 1928-01-14, 1928-01-17, 1928-01-21, 1928-01-22, 1928-01-24, 1928-01-28, 1928-01-31, 1928-02-07, 1928-02-11, 1928-02-14, 1928-02-19, 1928-02-21, 1928-02-25, 1928-02-28, 1928-03-03, 1928-03-06, 1928-03-10, 1928-03-11, 1928-03-13, 1928-03-15, 1928-03-17, 1928-03-20, 1928-03-24
    ## 22 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26, 1952-10-12, 1952-10-16, 1952-10-18, 1952-10-19, 1952-10-22, 1952-10-25, 1952-10-26, 1952-10-30, 1952-11-01, 1952-11-02, 1952-11-06, 1952-11-09, 1952-11-11, 1952-11-13, 1952-11-15, 1952-11-16, 1952-11-19, 1952-11-20, 1952-11-23, 1952-11-27, 1952-11-30, 1952-12-04, 1952-12-06, 1952-12-07, 1952-12-10, 1952-12-11, 1952-12-14, 1952-12-17, 1952-12-18, 1952-12-20, 1952-12-21, 1952-12-25, 1952-12-27, 1952-12-28, 1953-01-01, 1953-01-03, 1953-01-04, 1953-01-08, 1953-01-10, 1953-01-11, 1953-01-15, 1953-01-18, 1953-01-22, 1953-01-24, 1953-01-25, 1953-01-29, 1953-01-31, 1953-02-01, 1953-02-05, 1953-02-08, 1953-02-12, 1953-02-14, 1953-02-15, 1953-02-18, 1953-02-21, 1953-02-22, 1953-02-25, 1953-02-27, 1953-03-01, 1953-03-02, 1953-03-05, 1953-03-07, 1953-03-08, 1953-03-12, 1953-03-14, 1953-03-15, 1953-03-18, 1953-03-19, 1953-03-21, 1953-03-22, 1954-10-09, 1954-10-11, 1954-10-14, 1954-10-17, 1954-10-20, 1954-10-21, 1954-10-23, 1954-10-30, 1954-11-04, 1954-11-07, 1954-11-10, 1954-11-13, 1954-11-14, 1954-11-17, 1954-11-18, 1954-11-20, 1954-11-21, 1954-11-24, 1954-11-25, 1954-11-28, 1954-12-01, 1954-12-02, 1954-12-04, 1954-12-05, 1954-12-09, 1954-12-11, 1954-12-12, 1954-12-16, 1954-12-18, 1954-12-19, 1954-12-25, 1954-12-30, 1955-01-01, 1955-01-02, 1955-01-05, 1955-01-06, 1955-01-08, 1955-01-09, 1955-01-12, 1955-01-13, 1955-01-15, 1955-01-16, 1955-01-20, 1955-01-22, 1955-01-23, 1955-01-27, 1955-01-29, 1955-01-30, 1955-02-02, 1955-02-03, 1955-02-05, 1955-02-06, 1955-02-10, 1955-02-12, 1955-02-13, 1955-02-16, 1955-02-17, 1955-02-19, 1955-02-21, 1955-02-23, 1955-02-26, 1955-03-02, 1955-03-03, 1955-03-05, 1955-03-06, 1955-03-10, 1955-03-12, 1955-03-13, 1955-03-16, 1955-03-20
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1970-10-11, 1970-10-14, 1970-10-16, 1970-10-18, 1970-10-22, 1970-10-25, 1970-10-29, 1970-10-31, 1970-11-01, 1970-11-05, 1970-11-07, 1970-11-08, 1970-11-10, 1970-11-14, 1970-11-15, 1970-11-18, 1970-11-21, 1970-11-22, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-02, 1970-12-03, 1970-12-05, 1970-12-06, 1970-12-10, 1970-12-12, 1970-12-13, 1970-12-16, 1970-12-19, 1970-12-20, 1970-12-23, 1970-12-25, 1970-12-26, 1970-12-30, 1971-01-01, 1971-01-03, 1971-01-07, 1971-01-09, 1971-01-10, 1971-01-14, 1971-01-16, 1971-01-17, 1971-01-23, 1971-01-24, 1971-01-27, 1971-01-28, 1971-01-31, 1971-02-03, 1971-02-06, 1971-02-07, 1971-02-09, 1971-02-11, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-20, 1971-02-23, 1971-02-25, 1971-02-28, 1971-03-02, 1971-03-04, 1971-03-06, 1971-03-07, 1971-03-10, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-18, 1971-03-20, 1971-03-21, 1971-03-24, 1971-03-27, 1971-03-28, 1971-03-31, 1971-04-03, 1971-04-04, 1971-10-13, 1971-10-14, 1971-10-17, 1971-10-20, 1971-10-22, 1971-10-24, 1971-10-27, 1971-10-28, 1971-10-31, 1971-11-04, 1971-11-06, 1971-11-07, 1971-11-10, 1971-11-11, 1971-11-14
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1924-12-01, 1924-12-03, 1924-12-08, 1924-12-10, 1924-12-15, 1924-12-17, 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-12, 1925-01-17, 1925-01-20, 1925-01-24, 1925-01-27, 1925-01-31, 1925-02-03, 1925-02-07, 1925-02-10, 1925-02-14, 1925-02-17, 1925-02-21, 1925-02-24, 1925-02-28, 1925-03-03, 1925-03-07, 1925-03-09
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 27                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1933-11-09, 1933-11-11, 1933-11-14, 1933-11-18, 1933-11-21, 1933-11-23, 1933-11-26, 1933-11-28, 1933-11-30, 1933-12-02, 1933-12-05, 1933-12-09, 1933-12-12, 1933-12-14, 1933-12-17, 1933-12-19, 1933-12-23, 1933-12-26, 1933-12-28, 1934-01-02, 1934-01-04, 1934-01-06, 1934-01-09, 1934-01-11, 1934-01-14, 1934-01-16, 1934-01-18, 1934-01-21, 1934-01-23, 1934-01-28, 1934-01-30, 1934-02-01, 1934-02-04, 1934-02-06, 1934-02-10, 1934-02-13, 1934-02-15, 1934-02-17, 1934-02-20, 1934-02-22, 1934-02-24, 1934-02-27, 1934-03-01, 1934-03-03, 1934-03-06, 1934-03-13, 1934-03-15, 1934-03-18, 1934-11-08, 1934-11-11, 1934-11-17, 1934-11-20, 1934-11-24, 1934-11-25, 1934-11-27, 1934-12-01, 1934-12-04, 1934-12-08, 1934-12-11, 1934-12-13, 1934-12-16, 1934-12-18, 1934-12-22, 1934-12-25, 1934-12-27, 1934-12-30, 1935-01-01, 1935-01-03, 1935-01-05, 1935-01-08, 1935-01-10, 1935-01-13, 1935-01-15, 1935-01-19, 1935-01-22, 1935-01-26, 1935-01-27, 1935-01-29, 1935-02-02, 1935-02-05, 1935-02-07, 1935-02-10, 1935-02-12, 1935-02-16, 1935-02-17, 1935-02-19, 1935-02-24, 1935-02-26, 1935-03-02, 1935-03-05, 1935-03-09, 1935-03-10, 1935-03-12, 1935-03-14, 1935-03-16, 1935-03-19, 1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1945-10-24, 1945-10-27, 1945-10-28, 1945-11-04, 1945-11-07, 1945-11-10, 1945-11-11, 1945-11-21, 1945-11-25, 1945-11-28, 1945-12-02, 1945-12-05, 1945-12-09, 1945-12-12, 1945-12-15, 1945-12-16, 1945-12-19, 1945-12-23, 1945-12-29, 1945-12-30, 1946-01-01, 1946-01-05, 1946-01-06, 1946-01-10, 1946-01-12, 1946-01-16, 1946-01-17, 1946-01-19, 1946-01-20, 1946-01-23, 1946-01-26, 1946-01-27, 1946-01-30, 1946-02-02, 1946-02-03, 1946-02-06, 1946-02-10, 1946-02-13, 1946-02-14, 1946-02-16, 1946-02-20, 1946-02-23, 1946-02-24, 1946-02-27, 1946-03-03, 1946-03-06, 1946-03-10, 1946-03-12, 1946-03-13, 1946-03-17
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1978-10-12, 1978-10-14, 1978-10-15, 1978-10-18, 1978-10-20, 1978-10-22, 1978-10-24, 1978-10-25, 1978-10-28, 1978-11-02, 1978-11-04, 1978-11-05, 1978-11-09, 1978-11-11, 1978-11-12, 1978-11-16, 1978-11-17, 1978-11-19, 1978-11-23, 1978-11-25, 1978-11-26, 1978-11-30, 1978-12-02, 1978-12-03, 1978-12-05, 1978-12-07, 1978-12-09, 1978-12-10, 1978-12-12, 1978-12-14, 1978-12-16, 1978-12-17, 1978-12-21, 1978-12-23, 1978-12-27, 1978-12-30, 1978-12-31, 1979-01-03, 1979-01-05, 1979-01-06, 1979-01-11, 1979-01-13, 1979-01-14, 1979-01-16, 1979-01-18, 1979-01-20, 1979-01-22, 1979-01-25, 1979-01-27, 1979-01-28, 1979-01-31, 1979-02-01, 1979-02-03, 1979-02-04, 1979-02-14, 1979-02-15, 1979-02-17, 1979-02-20, 1979-02-21, 1979-02-24, 1979-02-27, 1979-03-01, 1979-03-03, 1979-03-04, 1979-03-08, 1979-03-10, 1979-03-11, 1979-03-13, 1979-03-15, 1979-03-17, 1979-03-19, 1979-03-22, 1979-03-24, 1979-03-28, 1979-03-29, 1979-03-31, 1979-04-01, 1979-04-04, 1979-04-05, 1979-04-08
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1986-02-02, 1986-02-06, 1986-02-08, 1986-02-09, 1986-02-11, 1986-02-15, 1986-02-16, 1986-02-18, 1986-10-09, 1986-10-11, 1986-10-12, 1986-10-14, 1986-10-16, 1986-10-18, 1986-10-22, 1986-10-24, 1986-10-26, 1986-10-30, 1986-11-01, 1986-11-02, 1986-11-05, 1986-11-08, 1986-11-12, 1986-11-13, 1986-11-15, 1986-11-17, 1986-11-19, 1986-11-20, 1986-11-22, 1986-11-24, 1986-11-26, 1986-11-28, 1986-11-29, 1986-12-04, 1986-12-06, 1986-12-07, 1986-12-11, 1986-12-13, 1986-12-14, 1986-12-18, 1986-12-20, 1986-12-23, 1986-12-27, 1986-12-30, 1987-01-02, 1987-01-03, 1987-01-05, 1987-01-08, 1987-01-10, 1987-01-12, 1987-01-14, 1987-01-15, 1987-01-17, 1987-01-20, 1987-01-22, 1987-01-24, 1987-01-26, 1987-01-29, 1987-01-31, 1987-02-01, 1987-02-05, 1987-02-07, 1987-02-08, 1987-02-14, 1987-02-16, 1987-02-18, 1987-02-20, 1987-02-21, 1987-02-25, 1987-02-26, 1987-02-28, 1987-03-02, 1987-03-03, 1987-03-05, 1987-03-07, 1987-03-11, 1987-03-12, 1987-03-14, 1987-03-17, 1987-03-19, 1987-03-21, 1987-03-22, 1987-03-26, 1987-03-28, 1987-03-29, 1987-03-31, 1987-04-04, 1987-04-05, 1988-03-06, 1988-03-08, 1988-03-10, 1988-03-12, 1988-03-13, 1988-03-17, 1988-03-19, 1988-03-20, 1988-03-22, 1988-03-24, 1988-03-26, 1988-03-31, 1988-04-02, 1988-04-03
    ##    data.mostGoalsOneGame data.mostGoalsOneSeason
    ## 1                      0                       0
    ## 2                      0                       0
    ## 3                      0                       0
    ## 4                      0                       0
    ## 5                      0                       0
    ## 6                      0                       0
    ## 7                      0                       0
    ## 8                      0                       0
    ## 9                      0                       0
    ## 10                     0                       0
    ## 11                     0                       0
    ## 12                     0                       0
    ## 13                     0                       0
    ## 14                     0                       0
    ## 15                     0                       0
    ## 16                     0                       0
    ## 17                     0                       0
    ## 18                     0                       0
    ## 19                     0                       0
    ## 20                     0                       0
    ## 21                     0                       0
    ## 22                     0                       0
    ## 23                     0                       0
    ## 24                     0                       0
    ## 25                     0                       0
    ## 26                     0                       0
    ## 27                     0                       0
    ## 28                     0                       0
    ## 29                     0                       0
    ## 30                     0                       0
    ## 31                     0                       0
    ##         data.mostGoalsSeasonIds data.mostPenaltyMinutesOneSeason
    ## 1                      19751976                                0
    ## 2            19651966, 19671968                               11
    ## 3                      19651966                               14
    ## 4                      19681969                                0
    ## 5                      19641965                               26
    ## 6                      19591960                                2
    ## 7                      19241925                                4
    ## 8                      19251926                                0
    ## 9                      19351936                                0
    ## 10                     19621963                                4
    ## 11                     19251926                                0
    ## 12                     19491950                                0
    ## 13                     19661967                                0
    ## 14                     19491950                                0
    ## 15           19251926, 19261927                                9
    ## 16                     19241925                                7
    ## 17                     19601961                                0
    ## 18                     19561957                                4
    ## 19                     19601961                                0
    ## 20           19871988, 19881989                                4
    ## 21                     19271928                                0
    ## 22 19491950, 19521953, 19541955                                2
    ## 23           19701971, 19711972                                6
    ## 24                     19241925                                0
    ## 25                     19351936                                0
    ## 26                     19651966                                0
    ## 27 19331934, 19341935, 19351936                               17
    ## 28                     19651966                                0
    ## 29                     19451946                                0
    ## 30                     19781979                                0
    ## 31 19851986, 19861987, 19871988                               24
    ##    data.mostPenaltyMinutesSeasonIds
    ## 1                          19751976
    ## 2                          19671968
    ## 3                          19651966
    ## 4                          19681969
    ## 5                          19641965
    ## 6                          19591960
    ## 7                          19241925
    ## 8                          19251926
    ## 9                          19351936
    ## 10                         19621963
    ## 11                         19251926
    ## 12                         19491950
    ## 13                         19661967
    ## 14                         19491950
    ## 15                         19251926
    ## 16                         19241925
    ## 17                         19601961
    ## 18                         19561957
    ## 19                         19601961
    ## 20                         19881989
    ## 21                         19271928
    ## 22                         19541955
    ## 23                         19711972
    ## 24                         19241925
    ## 25                         19351936
    ## 26                         19651966
    ## 27                         19341935
    ## 28                         19651966
    ## 29                         19451946
    ## 30                         19781979
    ## 31                         19861987
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  data.mostPointsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1975-10-09, 1975-10-12, 1975-10-16, 1975-10-18, 1975-10-19, 1975-10-23, 1975-10-25, 1975-10-26, 1975-10-30, 1975-11-01, 1975-11-02, 1975-11-05, 1975-11-08, 1975-11-09, 1975-11-13, 1975-11-15, 1975-11-16, 1975-11-19, 1975-11-20, 1975-11-23, 1975-11-25, 1975-11-26, 1975-11-29, 1975-11-30, 1975-12-04, 1975-12-06, 1975-12-07, 1975-12-11, 1975-12-13, 1975-12-14, 1975-12-17, 1975-12-20, 1975-12-21, 1975-12-23, 1975-12-26, 1975-12-28, 1975-12-31, 1976-01-02, 1976-01-03, 1976-01-10, 1976-01-11, 1976-01-13, 1976-01-15, 1976-01-17, 1976-01-22, 1976-01-24, 1976-01-25, 1976-01-29, 1976-01-30, 1976-02-01, 1976-02-05, 1976-02-07, 1976-02-08, 1976-02-11, 1976-02-13, 1976-02-15, 1976-02-18, 1976-02-21, 1976-02-22, 1976-02-26, 1976-02-27, 1976-02-29, 1976-03-03, 1976-03-05, 1976-03-07, 1976-03-09, 1976-03-11, 1976-03-13, 1976-03-14, 1976-03-16, 1976-03-18, 1976-03-20, 1976-03-24, 1976-03-25, 1976-03-27, 1976-03-28, 1976-03-30, 1976-04-01, 1976-04-03, 1976-04-04
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1968-01-21
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1965-11-25, 1965-12-01, 1965-12-15
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1968-10-11, 1968-10-13, 1968-10-16, 1968-10-17, 1968-10-19, 1968-10-24, 1968-10-26, 1968-10-27, 1968-10-30, 1968-10-31, 1968-11-03, 1968-11-06, 1968-11-10, 1968-11-13, 1968-11-14, 1968-11-17, 1968-11-21, 1968-11-23, 1968-11-24, 1968-11-27, 1968-11-30, 1968-12-01, 1968-12-05, 1968-12-07, 1968-12-08, 1968-12-11, 1968-12-14, 1968-12-15, 1968-12-19, 1968-12-21, 1968-12-22, 1968-12-25, 1968-12-28, 1968-12-29, 1969-01-02, 1969-01-04, 1969-01-09, 1969-01-11, 1969-01-12, 1969-01-15, 1969-01-16, 1969-01-18, 1969-01-19, 1969-01-23, 1969-01-25, 1969-01-26, 1969-01-29, 1969-01-30, 1969-02-02, 1969-02-05, 1969-02-06, 1969-02-08, 1969-02-09, 1969-02-11, 1969-02-15, 1969-02-16, 1969-02-19, 1969-02-23, 1969-02-26, 1969-02-27, 1969-03-01, 1969-03-02, 1969-03-05, 1969-03-08, 1969-03-09, 1969-03-11, 1969-03-13, 1969-03-15, 1969-03-16, 1969-03-19, 1969-03-20, 1969-03-22, 1969-03-23, 1969-03-27, 1969-03-29, 1969-03-30
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1964-10-17, 1964-11-01
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1959-10-08, 1959-10-10, 1959-10-11, 1959-10-14, 1959-10-17, 1959-10-18, 1959-10-22, 1959-10-24, 1959-10-29, 1959-10-31, 1959-11-01, 1959-11-03, 1959-11-05, 1959-11-08, 1959-11-11, 1959-11-12, 1959-11-14, 1959-11-15, 1959-11-21, 1959-11-22, 1959-11-25, 1959-11-26, 1959-11-28, 1959-11-29, 1959-12-02, 1959-12-05, 1959-12-06, 1959-12-10, 1959-12-12, 1959-12-13, 1959-12-16, 1959-12-20, 1959-12-25, 1959-12-27, 1959-12-29, 1960-01-01, 1960-01-02, 1960-01-03, 1960-01-07, 1960-01-09, 1960-01-10, 1960-01-14, 1960-01-16, 1960-01-17, 1960-01-20, 1960-01-21, 1960-01-23, 1960-01-24, 1960-01-30, 1960-01-31, 1960-02-04, 1960-02-06, 1960-02-07, 1960-02-11, 1960-02-13, 1960-02-14, 1960-02-17, 1960-02-20, 1960-02-21, 1960-02-27, 1960-03-01, 1960-03-03, 1960-03-05, 1960-03-06, 1960-03-10, 1960-03-12, 1960-03-13, 1960-03-16, 1960-03-19, 1960-03-20
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1925-01-10, 1925-01-24
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1963-01-10
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1925-11-26, 1925-11-28, 1925-12-01, 1925-12-03, 1925-12-05, 1925-12-08, 1925-12-11, 1925-12-15, 1925-12-19, 1925-12-22, 1925-12-29, 1926-01-05, 1926-01-07, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-19, 1926-01-23, 1926-01-26, 1926-01-30, 1926-02-02, 1926-02-04, 1926-02-06, 1926-02-09, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-02, 1926-03-04, 1926-03-06, 1926-03-09, 1926-03-12, 1926-03-16
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1966-10-19, 1966-10-22, 1966-10-23, 1966-10-29, 1966-10-30, 1966-11-01, 1966-11-03, 1966-11-06, 1966-11-09, 1966-11-10, 1966-11-13, 1966-11-19, 1966-11-20, 1966-11-23, 1966-11-24, 1966-11-26, 1966-11-27, 1966-12-01, 1966-12-03, 1966-12-04, 1966-12-07, 1966-12-08, 1966-12-11, 1966-12-14, 1966-12-15, 1966-12-18, 1966-12-21, 1966-12-24, 1966-12-25, 1966-12-27, 1966-12-28, 1966-12-31, 1967-01-01, 1967-01-07, 1967-01-08, 1967-01-12, 1967-01-14, 1967-01-15, 1967-01-19, 1967-01-21, 1967-01-22, 1967-01-25, 1967-01-26, 1967-01-29, 1967-02-01, 1967-02-02, 1967-02-04, 1967-02-05, 1967-02-08, 1967-02-11, 1967-02-12, 1967-02-14, 1967-02-16, 1967-02-18, 1967-02-23, 1967-02-25, 1967-02-26, 1967-03-02, 1967-03-04, 1967-03-05, 1967-03-08, 1967-03-12, 1967-03-15, 1967-03-18, 1967-03-19, 1967-03-23, 1967-03-25, 1967-03-26, 1967-03-30, 1967-04-02
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1950-03-22
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1925-12-19
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-17, 1925-01-20, 1925-01-31
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1960-10-05, 1960-10-08, 1960-10-09, 1960-10-11, 1960-10-15, 1960-10-16, 1960-10-20, 1960-10-23, 1960-10-27, 1960-10-29, 1960-10-30, 1960-11-02, 1960-11-03, 1960-11-06, 1960-11-10, 1960-11-13, 1960-11-16, 1960-11-17, 1960-11-19, 1960-11-20, 1960-11-23, 1960-11-24, 1960-11-27, 1960-11-30, 1960-12-01, 1960-12-03, 1960-12-04, 1960-12-08, 1960-12-10, 1960-12-11, 1960-12-17, 1960-12-18, 1960-12-22, 1960-12-25, 1960-12-28, 1960-12-31, 1961-01-01, 1961-01-05, 1961-01-07, 1961-01-08, 1961-01-12, 1961-01-14, 1961-01-15, 1961-01-19, 1961-01-21, 1961-01-22, 1961-01-25, 1961-01-26, 1961-01-29, 1961-02-02, 1961-02-04, 1961-02-05, 1961-02-09, 1961-02-11, 1961-02-12, 1961-02-16, 1961-02-18, 1961-02-19, 1961-02-23, 1961-02-26, 1961-03-01, 1961-03-02, 1961-03-05, 1961-03-07, 1961-03-09, 1961-03-11, 1961-03-12, 1961-03-15, 1961-03-18, 1961-03-19
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1956-10-11, 1956-10-13, 1956-10-14, 1956-10-17, 1956-10-20, 1956-10-21, 1956-10-27, 1956-10-30, 1956-11-01, 1956-11-04, 1956-11-07, 1956-11-08, 1956-11-10, 1956-11-11, 1956-11-15, 1956-11-17, 1956-11-18, 1956-11-22, 1956-11-24, 1956-11-25, 1956-11-28, 1956-11-29, 1956-12-02, 1956-12-06, 1956-12-08, 1956-12-09, 1956-12-13, 1956-12-15, 1956-12-16, 1956-12-20, 1956-12-22, 1956-12-23, 1956-12-25, 1956-12-27, 1956-12-30, 1957-01-01, 1957-01-05, 1957-01-06, 1957-01-10, 1957-01-12, 1957-01-13, 1957-01-17, 1957-01-19, 1957-01-20, 1957-01-26, 1957-01-27, 1957-01-31, 1957-02-02, 1957-02-03, 1957-02-06, 1957-02-07, 1957-02-09, 1957-02-10, 1957-02-13, 1957-02-16, 1957-02-17, 1957-02-20, 1957-02-23, 1957-02-24, 1957-02-28, 1957-03-02, 1957-03-03, 1957-03-07, 1957-03-09, 1957-03-10, 1957-03-13, 1957-03-16, 1957-03-17, 1957-03-21, 1957-03-23
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1960-10-05, 1960-10-08, 1960-10-09, 1960-10-11, 1960-10-15, 1960-10-16, 1960-10-20, 1960-10-23, 1960-10-27, 1960-10-29, 1960-10-30, 1960-11-02, 1960-11-03, 1960-11-06, 1960-11-10, 1960-11-13, 1960-11-16, 1960-11-17, 1960-11-19, 1960-11-20, 1960-11-23, 1960-11-24, 1960-11-27, 1960-11-30, 1960-12-01, 1960-12-03, 1960-12-04, 1960-12-08, 1960-12-10, 1960-12-11, 1960-12-17, 1960-12-18, 1960-12-22, 1960-12-25, 1960-12-28, 1960-12-31, 1961-01-01, 1961-01-05, 1961-01-07, 1961-01-08, 1961-01-12, 1961-01-14, 1961-01-15, 1961-01-19, 1961-01-21, 1961-01-22, 1961-01-25, 1961-01-26, 1961-01-29, 1961-02-02, 1961-02-04, 1961-02-05, 1961-02-09, 1961-02-11, 1961-02-12, 1961-02-16, 1961-02-18, 1961-02-19, 1961-02-23, 1961-02-26, 1961-03-01, 1961-03-02, 1961-03-05, 1961-03-07, 1961-03-09, 1961-03-11, 1961-03-12, 1961-03-15, 1961-03-18, 1961-03-19
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1988-01-30, 1988-02-01, 1988-02-04, 1988-02-06, 1988-02-07, 1988-02-12, 1988-02-13, 1988-02-17, 1988-02-21, 1988-02-23, 1988-02-25, 1988-02-27, 1988-03-03, 1988-03-05, 1988-03-06, 1988-03-08, 1988-03-10, 1988-03-12, 1988-03-13, 1988-03-17, 1988-03-19, 1988-03-20, 1988-03-22, 1988-03-24, 1988-03-26, 1988-03-31, 1988-04-02, 1988-04-03, 1988-12-06, 1988-12-08, 1988-12-10, 1988-12-12, 1988-12-15, 1988-12-17, 1988-12-18, 1988-12-21, 1988-12-22, 1988-12-26, 1988-12-29, 1989-01-02, 1989-01-05, 1989-01-07, 1989-01-08, 1989-01-12, 1989-01-14, 1989-01-15, 1989-01-19, 1989-01-21, 1989-01-22, 1989-01-25, 1989-01-26, 1989-01-28, 1989-02-01, 1989-02-03, 1989-02-05, 1989-02-09, 1989-02-11, 1989-02-14, 1989-02-15, 1989-02-18, 1989-02-19, 1989-02-25, 1989-02-28, 1989-03-02, 1989-03-04, 1989-03-05, 1989-03-07, 1989-03-09, 1989-03-11, 1989-03-12, 1989-03-14, 1989-03-16, 1989-03-18, 1989-03-19, 1989-03-22, 1989-03-23, 1989-03-25, 1989-03-27, 1989-04-01, 1989-04-02
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1927-11-15, 1927-11-19, 1927-11-22, 1927-11-26, 1927-11-27, 1927-11-29, 1927-12-01, 1927-12-03, 1927-12-06, 1927-12-10, 1927-12-11, 1927-12-13, 1927-12-17, 1927-12-20, 1927-12-27, 1927-12-29, 1928-01-01, 1928-01-03, 1928-01-07, 1928-01-10, 1928-01-12, 1928-01-14, 1928-01-17, 1928-01-21, 1928-01-22, 1928-01-24, 1928-01-28, 1928-01-31, 1928-02-07, 1928-02-11, 1928-02-14, 1928-02-19, 1928-02-21, 1928-02-25, 1928-02-28, 1928-03-03, 1928-03-06, 1928-03-10, 1928-03-11, 1928-03-13, 1928-03-15, 1928-03-17, 1928-03-20, 1928-03-24
    ## 22 1949-10-12, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-23, 1949-10-26, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-09, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-17, 1949-11-20, 1949-11-23, 1949-11-26, 1949-11-27, 1949-11-30, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-07, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-05, 1950-01-08, 1950-01-11, 1950-01-14, 1950-01-15, 1950-01-18, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-26, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-15, 1950-02-19, 1950-02-22, 1950-02-25, 1950-02-26, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-08, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26, 1952-10-12, 1952-10-16, 1952-10-18, 1952-10-19, 1952-10-22, 1952-10-25, 1952-10-26, 1952-10-30, 1952-11-01, 1952-11-02, 1952-11-06, 1952-11-09, 1952-11-11, 1952-11-13, 1952-11-15, 1952-11-16, 1952-11-19, 1952-11-20, 1952-11-23, 1952-11-27, 1952-11-30, 1952-12-04, 1952-12-06, 1952-12-07, 1952-12-10, 1952-12-11, 1952-12-14, 1952-12-17, 1952-12-18, 1952-12-20, 1952-12-21, 1952-12-25, 1952-12-27, 1952-12-28, 1953-01-01, 1953-01-03, 1953-01-04, 1953-01-08, 1953-01-10, 1953-01-11, 1953-01-15, 1953-01-18, 1953-01-22, 1953-01-24, 1953-01-25, 1953-01-29, 1953-01-31, 1953-02-01, 1953-02-05, 1953-02-08, 1953-02-12, 1953-02-14, 1953-02-15, 1953-02-18, 1953-02-21, 1953-02-22, 1953-02-25, 1953-02-27, 1953-03-01, 1953-03-02, 1953-03-05, 1953-03-07, 1953-03-08, 1953-03-12, 1953-03-14, 1953-03-15, 1953-03-18, 1953-03-19, 1953-03-21, 1953-03-22, 1954-10-09, 1954-10-11, 1954-10-14, 1954-10-17, 1954-10-20, 1954-10-21, 1954-10-23, 1954-10-30, 1954-11-04, 1954-11-07, 1954-11-10, 1954-11-13, 1954-11-14, 1954-11-17, 1954-11-18, 1954-11-20, 1954-11-21, 1954-11-24, 1954-11-25, 1954-11-28, 1954-12-01, 1954-12-02, 1954-12-04, 1954-12-05, 1954-12-09, 1954-12-11, 1954-12-12, 1954-12-16, 1954-12-18, 1954-12-19, 1954-12-25, 1954-12-30, 1955-01-01, 1955-01-02, 1955-01-05, 1955-01-06, 1955-01-08, 1955-01-09, 1955-01-12, 1955-01-13, 1955-01-15, 1955-01-16, 1955-01-20, 1955-01-22, 1955-01-23, 1955-01-27, 1955-01-29, 1955-01-30, 1955-02-02, 1955-02-03, 1955-02-05, 1955-02-06, 1955-02-10, 1955-02-12, 1955-02-13, 1955-02-16, 1955-02-17, 1955-02-19, 1955-02-21, 1955-02-23, 1955-02-26, 1955-03-02, 1955-03-03, 1955-03-05, 1955-03-06, 1955-03-10, 1955-03-12, 1955-03-13, 1955-03-16, 1955-03-20
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1971-11-04
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1924-12-01, 1924-12-03, 1924-12-08, 1924-12-10, 1924-12-15, 1924-12-17, 1924-12-22, 1924-12-25, 1924-12-29, 1925-01-01, 1925-01-03, 1925-01-05, 1925-01-10, 1925-01-12, 1925-01-17, 1925-01-20, 1925-01-24, 1925-01-27, 1925-01-31, 1925-02-03, 1925-02-07, 1925-02-10, 1925-02-14, 1925-02-17, 1925-02-21, 1925-02-24, 1925-02-28, 1925-03-03, 1925-03-07, 1925-03-09
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1935-11-16, 1935-11-19, 1935-11-24, 1935-11-26, 1935-11-30, 1935-12-01, 1935-12-03, 1935-12-05, 1935-12-08, 1935-12-10, 1935-12-12, 1935-12-15, 1935-12-17, 1935-12-19, 1935-12-22, 1935-12-25, 1935-12-28, 1935-12-29, 1936-01-01, 1936-01-04, 1936-01-07, 1936-01-12, 1936-01-14, 1936-01-18, 1936-01-19, 1936-01-21, 1936-01-26, 1936-01-28, 1936-02-02, 1936-02-04, 1936-02-06, 1936-02-09, 1936-02-11, 1936-02-13, 1936-02-16, 1936-02-18, 1936-02-23, 1936-02-25, 1936-02-27, 1936-03-01, 1936-03-03, 1936-03-05, 1936-03-08, 1936-03-10, 1936-03-15, 1936-03-17, 1936-03-19, 1936-03-22
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 27                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1935-02-10
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1965-10-24, 1965-10-27, 1965-10-30, 1965-11-03, 1965-11-04, 1965-11-06, 1965-11-07, 1965-11-10, 1965-11-14, 1965-11-20, 1965-11-21, 1965-11-24, 1965-11-25, 1965-11-27, 1965-11-28, 1965-12-01, 1965-12-02, 1965-12-04, 1965-12-05, 1965-12-08, 1965-12-11, 1965-12-12, 1965-12-15, 1965-12-16, 1965-12-18, 1965-12-19, 1965-12-25, 1965-12-26, 1965-12-28, 1966-01-01, 1966-01-02, 1966-01-06, 1966-01-08, 1966-01-09, 1966-01-13, 1966-01-15, 1966-01-16, 1966-01-20, 1966-01-22, 1966-01-23, 1966-01-27, 1966-01-29, 1966-01-30, 1966-02-03, 1966-02-05, 1966-02-06, 1966-02-10, 1966-02-12, 1966-02-13, 1966-02-16, 1966-02-19, 1966-02-20, 1966-02-23, 1966-02-26, 1966-02-27, 1966-03-02, 1966-03-03, 1966-03-06, 1966-03-09, 1966-03-12, 1966-03-13, 1966-03-16, 1966-03-17, 1966-03-20, 1966-03-24, 1966-03-26, 1966-03-27, 1966-03-29, 1966-03-31, 1966-04-03
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1945-10-24, 1945-10-27, 1945-10-28, 1945-11-04, 1945-11-07, 1945-11-10, 1945-11-11, 1945-11-21, 1945-11-25, 1945-11-28, 1945-12-02, 1945-12-05, 1945-12-09, 1945-12-12, 1945-12-15, 1945-12-16, 1945-12-19, 1945-12-23, 1945-12-29, 1945-12-30, 1946-01-01, 1946-01-05, 1946-01-06, 1946-01-10, 1946-01-12, 1946-01-16, 1946-01-17, 1946-01-19, 1946-01-20, 1946-01-23, 1946-01-26, 1946-01-27, 1946-01-30, 1946-02-02, 1946-02-03, 1946-02-06, 1946-02-10, 1946-02-13, 1946-02-14, 1946-02-16, 1946-02-20, 1946-02-23, 1946-02-24, 1946-02-27, 1946-03-03, 1946-03-06, 1946-03-10, 1946-03-12, 1946-03-13, 1946-03-17
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1978-10-12, 1978-10-14, 1978-10-15, 1978-10-18, 1978-10-20, 1978-10-22, 1978-10-24, 1978-10-25, 1978-10-28, 1978-11-02, 1978-11-04, 1978-11-05, 1978-11-09, 1978-11-11, 1978-11-12, 1978-11-16, 1978-11-17, 1978-11-19, 1978-11-23, 1978-11-25, 1978-11-26, 1978-11-30, 1978-12-02, 1978-12-03, 1978-12-05, 1978-12-07, 1978-12-09, 1978-12-10, 1978-12-12, 1978-12-14, 1978-12-16, 1978-12-17, 1978-12-21, 1978-12-23, 1978-12-27, 1978-12-30, 1978-12-31, 1979-01-03, 1979-01-05, 1979-01-06, 1979-01-11, 1979-01-13, 1979-01-14, 1979-01-16, 1979-01-18, 1979-01-20, 1979-01-22, 1979-01-25, 1979-01-27, 1979-01-28, 1979-01-31, 1979-02-01, 1979-02-03, 1979-02-04, 1979-02-14, 1979-02-15, 1979-02-17, 1979-02-20, 1979-02-21, 1979-02-24, 1979-02-27, 1979-03-01, 1979-03-03, 1979-03-04, 1979-03-08, 1979-03-10, 1979-03-11, 1979-03-13, 1979-03-15, 1979-03-17, 1979-03-19, 1979-03-22, 1979-03-24, 1979-03-28, 1979-03-29, 1979-03-31, 1979-04-01, 1979-04-04, 1979-04-05, 1979-04-08
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1987-03-05, 1987-03-14, 1987-03-19, 1988-03-10
    ##    data.mostPointsOneGame data.mostPointsOneSeason
    ## 1                       0                        0
    ## 2                       1                        1
    ## 3                       1                        3
    ## 4                       0                        0
    ## 5                       1                        2
    ## 6                       0                        0
    ## 7                       1                        2
    ## 8                       0                        0
    ## 9                       0                        0
    ## 10                      2                        5
    ## 11                      0                        0
    ## 12                      0                        0
    ## 13                      0                        0
    ## 14                      1                        1
    ## 15                      1                        1
    ## 16                      0                        0
    ## 17                      0                        0
    ## 18                      0                        0
    ## 19                      0                        0
    ## 20                      0                        0
    ## 21                      0                        0
    ## 22                      0                        0
    ## 23                      2                        2
    ## 24                      0                        0
    ## 25                      0                        0
    ## 26                      0                        0
    ## 27                      1                        1
    ## 28                      0                        0
    ## 29                      0                        0
    ## 30                      0                        0
    ## 31                      1                        3
    ##        data.mostPointsSeasonIds data.penaltyMinutes data.playerId
    ## 1                      19751976                   0       8444899
    ## 2                      19671968                  11       8444966
    ## 3                      19651966                  14       8444980
    ## 4                      19681969                   0       8444984
    ## 5                      19641965                  26       8445005
    ## 6                      19591960                   2       8445009
    ## 7                      19241925                   4       8445061
    ## 8                      19251926                   0       8445068
    ## 9                      19351936                   0       8445074
    ## 10                     19621963                   4       8445084
    ## 11                     19251926                   0       8445152
    ## 12                     19491950                   0       8445153
    ## 13                     19661967                   0       8445239
    ## 14                     19491950                   0       8445301
    ## 15                     19251926                   9       8445306
    ## 16                     19241925                   7       8445340
    ## 17                     19601961                   0       8445348
    ## 18                     19561957                   4       8445366
    ## 19                     19601961                   0       8445377
    ## 20           19871988, 19881989                   4       8445425
    ## 21                     19271928                   0       8445488
    ## 22 19491950, 19521953, 19541955                   2       8445573
    ## 23                     19711972                   6       8445591
    ## 24                     19241925                   0       8445743
    ## 25                     19351936                   0       8445747
    ## 26                     19651966                   0       8445816
    ## 27                     19341935                  25       8445848
    ## 28                     19651966                   0       8445852
    ## 29                     19451946                   0       8445863
    ## 30                     19781979                   0       8445867
    ## 31                     19861987                  60       8445901
    ##    data.points data.positionCode data.rookieGamesPlayed data.rookiePoints
    ## 1            0                 C                      1                 0
    ## 2            1                 D                      4                 1
    ## 3            3                 D                     14                 3
    ## 4            0                 R                      1                 0
    ## 5            2                 R                     NA                NA
    ## 6            0                 C                      7                 0
    ## 7            2                 D                      8                 2
    ## 8            0                 R                      2                 0
    ## 9            0                 R                      8                 0
    ## 10           5                 L                      6                 5
    ## 11           0                 L                      7                 0
    ## 12           0                 C                      1                 0
    ## 13           0                 C                      3                 0
    ## 14           1                 D                      1                 1
    ## 15           1                 R                     31                 1
    ## 16           0                 D                     10                 0
    ## 17           0                 C                      8                 0
    ## 18           0                 D                      6                 0
    ## 19           0                 C                      3                 0
    ## 20           0                 C                      7                 0
    ## 21           0                 D                      2                 0
    ## 22           0                 C                      2                 0
    ## 23           2                 C                     11                 2
    ## 24           0                 C                      1                 0
    ## 25           0                 L                     12                 0
    ## 26           0                 C                      3                 0
    ## 27           1                 D                      9                 0
    ## 28           0                 D                      1                 0
    ## 29           0                 R                      1                 0
    ## 30           0                 D                     NA                NA
    ## 31           4                 D                     NA                NA
    ##    data.seasons total
    ## 1             1   918
    ## 2             2   918
    ## 3             1   918
    ## 4             1   918
    ## 5             1   918
    ## 6             1   918
    ## 7             1   918
    ## 8             1   918
    ## 9             1   918
    ## 10            1   918
    ## 11            1   918
    ## 12            1   918
    ## 13            1   918
    ## 14            1   918
    ## 15            2   918
    ## 16            1   918
    ## 17            1   918
    ## 18            1   918
    ## 19            1   918
    ## 20            2   918
    ## 21            1   918
    ## 22            3   918
    ## 23            2   918
    ## 24            1   918
    ## 25            1   918
    ## 26            1   918
    ## 27            3   918
    ## 28            1   918
    ## 29            1   918
    ## 30            1   918
    ## 31            3   918
    ##  [ reached 'max' / getOption("max.print") -- omitted 887 rows ]

#### detail function

``` r
detail<- function (id,...){
  base_url <- "https://records.nhl.com/site/api/franchise-detail?cayenneExp=mostRecentTeamId"
  if (id==0 ) {
get_url <- GET (base_url) 
   record_txt <- content (get_url , "text", encoding = "UTF-8")
   record_json <- fromJSON(record_txt, flatten=TRUE)
   Data_df <- data.frame(record_json)
    return( list(base_url,Data_df))
  }
    else {
      full_url  <-  paste0(base_url,"=",id )    
      get_url <- GET (full_url)
      record_txt <- content (get_url , "text", encoding = "UTF-8")
      record_json <- fromJSON(record_txt, flatten=TRUE)
      Data_df <- data.frame(record_json)
      return( list(full_url,Data_df))
  }
}
record_detail <- detail( 2 )
record_detail
```

    ## [[1]]
    ## [1] "https://records.nhl.com/site/api/franchise-detail?cayenneExp=mostRecentTeamId=2"
    ## 
    ## [[2]]
    ##   data.id data.active
    ## 1      22        TRUE
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   data.captainHistory
    ## 1 <ul class="striped-list">\r\n\t<li>Anders Lee: 2018-19 &ndash;&nbsp;Present</li>\r\n\t<li>John Tavares: 2013-14 &ndash;&nbsp;2017-18</li>\r\n\t<li>Mark Streit: 2011-12 &ndash;&nbsp;2012-13</li>\r\n\t<li>Doug Weight: 2009-10 &ndash;&nbsp;2010-11</li>\r\n\t<li>Bill Guerin and (No Captain): 2008-09</li>\r\n\t<li>Bill Guerin: 2007-08</li>\r\n\t<li>Alexei Yashin: 2005-06 &ndash;&nbsp;2006-07</li>\r\n\t<li>Michael Peca: 2001-02 &ndash;&nbsp;2003-04</li>\r\n\t<li>Kenny Jonsson: 1999-00 &ndash;&nbsp;2000-01</li>\r\n\t<li>Trevor Linden: 1998-99</li>\r\n\t<li>Bryan McCabe and Trevor Linden: 1997-98</li>\r\n\t<li>(No Captain): 1996-97</li>\r\n\t<li>Patrick Flatley: 1992-93 &ndash;&nbsp;1995-96</li>\r\n\t<li>Brent Sutter and Patrick Flatley: 1991-92</li>\r\n\t<li>Brent Sutter: 1987-88 &ndash;&nbsp;1990-91</li>\r\n\t<li>Denis Potvin: 1979-80 &ndash;&nbsp;1986-87</li>\r\n\t<li>Clark Gillies: 1977-78 &ndash;&nbsp;1978-79</li>\r\n\t<li>Ed Westfall and Clark Gillies: 1976-77</li>\r\n\t<li>Ed Westfall: 1972-73 &ndash;&nbsp;1975-76</li>\r\n</ul>\r\n
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    data.coachingHistory
    ## 1 <ul class="striped-list">\r\n\t<li>Barry Trotz: Oct. 4, 2018 &ndash; Present</li>\r\n\t<li>Doug Weight: Jan. 19, 2017 &ndash; April 7, 2018</li>\r\n\t<li>Jack Capuano: Nov. 17, 2010 &ndash; Jan. 16, 2017</li>\r\n\t<li>Scott Gordon: Oct. 10, 2008 &ndash; Nov. 13, 2010</li>\r\n\t<li>Ted Nolan: Nov. 3, 2007 &ndash; April 4, 2008</li>\r\n\t<li>Al Arbour: Nov. 1, 2007</li>\r\n\t<li>Ted Nolan: Oct. 5, 2006 &ndash; Oct. 27, 2007</li>\r\n\t<li>Brad Shaw: Jan. 12&nbsp;&ndash; April 18, 2006</li>\r\n\t<li>Steve Stirling: Oct. 9, 2003 &ndash; Jan. 10, 2006</li>\r\n\t<li>Peter Laviolette: Oct. 5, 2001 &ndash; April 17, 2003</li>\r\n\t<li>Lorne Henning: March 5&nbsp;&ndash; April 7, 2001</li>\r\n\t<li>Butch Goring: Oct. 2, 1999 &ndash; March 3, 2001</li>\r\n\t<li>Bill Stewart: Jan. 21&nbsp;&ndash; April 17, 1999</li>\r\n\t<li>Mike Milbury: March 12, 1998 &ndash; Jan. 20, 1999</li>\r\n\t<li>Rick Bowness: Jan. 24, 1997 &ndash; March 10, 1998</li>\r\n\t<li>Mike Milbury: Oct. 7, 1995 &ndash; Jan. 22, 1997</li>\r\n\t<li>Lorne Henning: Jan. 21&nbsp;&ndash; May 2, 1995</li>\r\n\t<li>Al Arbour: Dec. 9, 1988 &ndash; April 24, 1994</li>\r\n\t<li>Terry Simpson: Oct. 9, 1986 &ndash; Dec. 6, 1988</li>\r\n\t<li>Al Arbour: Oct. 10, 1973 &ndash; April 12, 1986</li>\r\n\t<li>Earl Ingarfield: Jan. 31&nbsp;&ndash; April 1, 1973</li>\r\n\t<li>Phil Goyette: Oct. 7, 1972 &ndash; Jan. 26, 1973</li>\r\n\t<li>* <em>Date range indicates first and last games coached during tenure (regular season or playoffs)</em></li>\r\n</ul>\r\n
    ##      data.dateAwarded
    ## 1 1972-06-06T00:00:00
    ##                                       data.directoryUrl data.firstSeasonId
    ## 1 https://www.nhl.com/islanders/team/business-directory           19721973
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               data.generalManagerHistory
    ## 1 <ul class="striped-list">\r\n\t<li>Lou Lamoriello: June 5, 2018 &ndash; Present</li>\r\n\t<li>Garth Snow: July 18, 2006 &ndash; June 5, 2018</li>\r\n\t<li>Neil Smith: June 6&nbsp;&ndash; July 18, 2006</li>\r\n\t<li>Mike Milbury: Dec. 12, 1995 &ndash; June 6, 2006</li>\r\n\t<li>Darcy Regier: Dec. 2-12, 1995</li>\r\n\t<li>Don Maloney: Aug. 17, 1992 &ndash; Dec. 2, 1995</li>\r\n\t<li>Bill Torrey: Feb. 14, 1972 &ndash; Aug. 17, 1992</li>\r\n\t<li>* <em>Date range indicates first and last days of tenure</em></li>\r\n</ul>\r\n
    ##                                                              data.heroImageUrl
    ## 1 https://records.nhl.com/site/asset/public/ext/hero/Team Pages/NYI/Barzal.jpg
    ##   data.mostRecentTeamId
    ## 1                     2
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                              data.retiredNumbersSummary
    ## 1 <ul class="striped-list">\r\n\t<li>5 &ndash;&nbsp;Denis Potvin (1973-88)</li>\r\n\t<li>9 &ndash;&nbsp;Clark Gillies (1974-86)</li>\r\n\t<li>19 &ndash;&nbsp;Bryan Trottier (1975-90)</li>\r\n\t<li>22 &ndash;&nbsp;Mike Bossy (1977-87)</li>\r\n\t<li>23 &ndash;&nbsp;Bobby Nystrom (1972-86)</li>\r\n\t<li>27 &ndash;&nbsp;John Tonelli (1978-86)</li>\r\n\t<li>31 &ndash;&nbsp;Billy Smith (1972-89)</li>\r\n\t<li>91 &ndash;&nbsp;Butch Goring (1980-84)</li>\r\n</ul>\r\n
    ##   data.teamAbbrev  data.teamFullName total
    ## 1             NYI New York Islanders     1

### NHL stats API

``` r
stats <- function(id, ... ) {
  base_url2 <- "https://statsapi.web.nhl.com/api/v1/teams/" 
  if (is.numeric (id)) {
    apiurl <- paste0 (base_url2, id, "/?expand=team.stats")
  }
  if (!is.numeric (id)) {
      apiurl<- paste0 (base_url2, "?expand=team.stats")
    }
  team <- GET (apiurl)
  team_txt <- content (team,"text", encoding="UTF-8")
  stats_json <- fromJSON(team_txt, flatten=TRUE)
  team_df <- data.frame(stats_json) 
  return(team_df)
} 
   stats_data <- stats (2)
   stats_data
```

    ##                                                                                                                                                                            copyright
    ## 1 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ##   teams.id         teams.name      teams.link teams.abbreviation
    ## 1        2 New York Islanders /api/v1/teams/2                NYI
    ##   teams.teamName teams.locationName teams.firstYearOfPlay
    ## 1      Islanders           New York                  1972
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                          teams.teamStats
    ## 1 56, NA, 32, 12th, 17, 11th, 7, 11th, 71, 12th, 63.4, 12th, 2.714, 21st, 2.232, 2nd, 1.2418, 7th, 18.8, 20th, 27, 24th, 22, 2nd, 144, 28th, 83.7, 6th, 28.9821, 22nd, 28.3929, 10th, 0.821, 10th, 0.321, 16th, 0.833, 7th, 0.842, 20th, 0.69, 4th, 0.44, 4th, 2916, 31st, 1498, 25th, 1418, 2nd, 51.4, 9th, 9.4, NA, 0.921, NA, NA, 2nd, NA, 1st, NA, 17th, 2, 2, New York Islanders, New York Islanders, /api/v1/teams/2, /api/v1/teams/2, statsSingleSeason, R, Regular season, FALSE
    ##   teams.shortName            teams.officialSiteUrl teams.franchiseId
    ## 1    NY Islanders http://www.newyorkislanders.com/                22
    ##   teams.active                  teams.venue.name    teams.venue.link
    ## 1         TRUE Nassau Veterans Memorial Coliseum /api/v1/venues/null
    ##   teams.venue.city teams.venue.timeZone.id teams.venue.timeZone.offset
    ## 1        Uniondale        America/New_York                          -4
    ##   teams.venue.timeZone.tz teams.division.id teams.division.name
    ## 1                     EDT                25     MassMutual East
    ##    teams.division.link teams.conference.id teams.conference.name
    ## 1 /api/v1/divisions/25                   6               Eastern
    ##   teams.conference.link teams.franchise.franchiseId
    ## 1 /api/v1/conferences/6                          22
    ##   teams.franchise.teamName  teams.franchise.link
    ## 1                Islanders /api/v1/franchises/22

``` r
   stats_data2 <- stats ("teams")
   stats_data2
```

    ##                                                                                                                                                                             copyright
    ## 1  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 2  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 3  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 4  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 5  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 6  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 7  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 8  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 9  NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 10 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 11 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 12 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 13 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 14 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 15 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 16 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 17 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 18 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 19 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 20 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 21 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 22 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 23 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 24 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 25 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 26 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 27 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 28 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 29 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 30 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 31 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ## 32 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ##    teams.id            teams.name       teams.link teams.abbreviation
    ## 1         1     New Jersey Devils  /api/v1/teams/1                NJD
    ## 2         2    New York Islanders  /api/v1/teams/2                NYI
    ## 3         3      New York Rangers  /api/v1/teams/3                NYR
    ## 4         4   Philadelphia Flyers  /api/v1/teams/4                PHI
    ## 5         5   Pittsburgh Penguins  /api/v1/teams/5                PIT
    ## 6         6         Boston Bruins  /api/v1/teams/6                BOS
    ## 7         7        Buffalo Sabres  /api/v1/teams/7                BUF
    ## 8         8    Montréal Canadiens  /api/v1/teams/8                MTL
    ## 9         9       Ottawa Senators  /api/v1/teams/9                OTT
    ## 10       10   Toronto Maple Leafs /api/v1/teams/10                TOR
    ## 11       12   Carolina Hurricanes /api/v1/teams/12                CAR
    ## 12       13      Florida Panthers /api/v1/teams/13                FLA
    ## 13       14   Tampa Bay Lightning /api/v1/teams/14                TBL
    ## 14       15   Washington Capitals /api/v1/teams/15                WSH
    ## 15       16    Chicago Blackhawks /api/v1/teams/16                CHI
    ## 16       17     Detroit Red Wings /api/v1/teams/17                DET
    ## 17       18   Nashville Predators /api/v1/teams/18                NSH
    ## 18       19       St. Louis Blues /api/v1/teams/19                STL
    ## 19       20        Calgary Flames /api/v1/teams/20                CGY
    ## 20       21    Colorado Avalanche /api/v1/teams/21                COL
    ## 21       22       Edmonton Oilers /api/v1/teams/22                EDM
    ## 22       23     Vancouver Canucks /api/v1/teams/23                VAN
    ## 23       24         Anaheim Ducks /api/v1/teams/24                ANA
    ## 24       25          Dallas Stars /api/v1/teams/25                DAL
    ## 25       26     Los Angeles Kings /api/v1/teams/26                LAK
    ## 26       28       San Jose Sharks /api/v1/teams/28                SJS
    ## 27       29 Columbus Blue Jackets /api/v1/teams/29                CBJ
    ## 28       30        Minnesota Wild /api/v1/teams/30                MIN
    ## 29       52         Winnipeg Jets /api/v1/teams/52                WPG
    ## 30       53       Arizona Coyotes /api/v1/teams/53                ARI
    ## 31       54  Vegas Golden Knights /api/v1/teams/54                VGK
    ## 32       55        Seattle Kraken /api/v1/teams/55                SEA
    ##    teams.teamName teams.locationName teams.firstYearOfPlay
    ## 1          Devils         New Jersey                  1982
    ## 2       Islanders           New York                  1972
    ## 3         Rangers           New York                  1926
    ## 4          Flyers       Philadelphia                  1967
    ## 5        Penguins         Pittsburgh                  1967
    ## 6          Bruins             Boston                  1924
    ## 7          Sabres            Buffalo                  1970
    ## 8       Canadiens           Montréal                  1909
    ## 9        Senators             Ottawa                  1990
    ## 10    Maple Leafs            Toronto                  1917
    ## 11     Hurricanes           Carolina                  1979
    ## 12       Panthers            Florida                  1993
    ## 13      Lightning          Tampa Bay                  1991
    ## 14       Capitals         Washington                  1974
    ## 15     Blackhawks            Chicago                  1926
    ## 16      Red Wings            Detroit                  1926
    ## 17      Predators          Nashville                  1997
    ## 18          Blues          St. Louis                  1967
    ## 19         Flames            Calgary                  1980
    ## 20      Avalanche           Colorado                  1979
    ## 21         Oilers           Edmonton                  1979
    ## 22        Canucks          Vancouver                  1970
    ## 23          Ducks            Anaheim                  1993
    ## 24          Stars             Dallas                  1967
    ## 25          Kings        Los Angeles                  1967
    ## 26         Sharks           San Jose                  1990
    ## 27   Blue Jackets           Columbus                  1997
    ## 28           Wild          Minnesota                  1997
    ## 29           Jets           Winnipeg                  2011
    ## 30        Coyotes            Arizona                  1979
    ## 31 Golden Knights              Vegas                  2016
    ## 32         Kraken            Seattle                  <NA>
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              teams.teamStats
    ## 1              56, NA, 19, 28th, 30, 29th, 7, 15th, 45, 29th, 40.2, 29th, 2.589, 26th, 3.375, 28th, 0.8293, 21st, 14.2, 28th, 22, 28th, 43, 30th, 155, 23rd, 71.0, 31st, 28.7857, 24th, 31.0179, 22nd, 0.552, 22nd, 0.111, 31st, 0.737, 19th, 0.733, 28th, 0.211, 31st, 0.417, 31st, 3180, 8th, 1481, 27th, 1699, 30th, 46.6, 27th, 9, NA, 0.891, NA, NA, 6th, NA, 29th, NA, 24th, 1, 1, New Jersey Devils, New Jersey Devils, /api/v1/teams/1, /api/v1/teams/1, statsSingleSeason, R, Regular season, FALSE
    ## 2                     56, NA, 32, 12th, 17, 11th, 7, 11th, 71, 12th, 63.4, 12th, 2.714, 21st, 2.232, 2nd, 1.2418, 7th, 18.8, 20th, 27, 24th, 22, 2nd, 144, 28th, 83.7, 6th, 28.9821, 22nd, 28.3929, 10th, 0.821, 10th, 0.321, 16th, 0.833, 7th, 0.842, 20th, 0.69, 4th, 0.44, 4th, 2916, 31st, 1498, 25th, 1418, 2nd, 51.4, 9th, 9.4, NA, 0.921, NA, NA, 2nd, NA, 1st, NA, 17th, 2, 2, New York Islanders, New York Islanders, /api/v1/teams/2, /api/v1/teams/2, statsSingleSeason, R, Regular season, FALSE
    ## 3                 56, NA, 27, 16th, 23, 18th, 6, 17th, 60, 17th, 53.6, 17th, 3.143, 10th, 2.768, 14th, 1.0943, 13th, 20.7, 14th, 37, 10th, 30, 12th, 179, 4th, 82.2, 10th, 28.6964, 25th, 29.7143, 13th, 0.7, 12th, 0.231, 24th, 0.739, 16th, 0.808, 23rd, 0.545, 14th, 0.484, 14th, 3026, 26th, 1346, 31st, 1680, 29th, 44.5, 31st, 11, NA, 0.907, NA, NA, 20th, NA, 12th, NA, 4th, 3, 3, New York Rangers, New York Rangers, /api/v1/teams/3, /api/v1/teams/3, statsSingleSeason, R, Regular season, FALSE
    ## 4             56, NA, 25, 18th, 23, 19th, 8, 8th, 58, 19th, 51.8, 19th, 2.857, 15th, 3.518, 31st, 0.806, 23rd, 19.2, 18th, 32, 15th, 45, 31st, 167, 12th, 73.0, 30th, 30.2143, 10th, 29.2143, 12th, 0.609, 23rd, 0.333, 11th, 0.769, 10th, 0.833, 21st, 0.441, 22nd, 0.455, 22nd, 3217, 6th, 1738, 3rd, 1479, 8th, 54.0, 2nd, 9.5, NA, 0.88, NA, NA, 17th, NA, 31st, NA, 16th, 4, 4, Philadelphia Flyers, Philadelphia Flyers, /api/v1/teams/4, /api/v1/teams/4, statsSingleSeason, R, Regular season, FALSE
    ## 5                       56, NA, 37, 4th, 16, 7th, 3, 25th, 77, 7th, 68.8, 7th, 3.446, 2nd, 2.768, 13th, 1.27, 5th, 23.7, 4th, 36, 12th, 35, 22nd, 152, 25th, 77.4, 27th, 30.0714, 12th, 29.9821, 15th, 0.793, 7th, 0.519, 2nd, 0.846, 6th, 0.926, 7th, 0.655, 6th, 0.68, 6th, 3191, 7th, 1573, 11th, 1618, 23rd, 49.3, 21st, 11.5, NA, 0.908, NA, NA, 11th, NA, 10th, NA, 1st, 5, 5, Pittsburgh Penguins, Pittsburgh Penguins, /api/v1/teams/5, /api/v1/teams/5, statsSingleSeason, R, Regular season, FALSE
    ## 6                               56, NA, 33, 11th, 16, 9th, 7, 10th, 73, 10th, 65.2, 10th, 2.929, 14th, 2.393, 5th, 1.1383, 12th, 21.9, 10th, 35, 14th, 25, 3rd, 160, 16th, 86.0, 2nd, 33.3214, 3rd, 27.0714, 2nd, 0.735, 5th, 0.364, 17th, 0.909, 2nd, 0.885, 11th, 0.615, 10th, 0.467, 10th, 3169, 9th, 1751, 2nd, 1418, 1st, 55.2, 1st, 8.8, NA, 0.912, NA, NA, 28th, NA, 5th, NA, 26th, 6, 6, Boston Bruins, Boston Bruins, /api/v1/teams/6, /api/v1/teams/6, statsSingleSeason, R, Regular season, FALSE
    ## 7                    56, NA, 15, 31st, 34, 31st, 7, 16th, 37, 31st, 33.0, 31st, 2.393, 29th, 3.5, 30th, 0.635, 31st, 21.0, 12th, 30, 20th, 31, 15th, 143, 29th, 77.7, 26th, 28.4286, 26th, 33.7321, 31st, 0.421, 31st, 0.189, 23rd, 0.364, 31st, 0.667, 30th, 0.357, 27th, 0.244, 27th, 3053, 24th, 1514, 21st, 1539, 13th, 49.6, 19th, 8.4, NA, 0.896, NA, NA, 3rd, NA, 26th, NA, 28th, 7, 7, Buffalo Sabres, Buffalo Sabres, /api/v1/teams/7, /api/v1/teams/7, statsSingleSeason, R, Regular season, FALSE
    ## 8                   56, NA, 24, 19th, 21, 15th, 11, 3rd, 59, 18th, 52.7, 18th, 2.821, 17th, 2.946, 18th, 1.0291, 17th, 19.2, 17th, 29, 22nd, 37, 26th, 151, 26th, 78.5, 23rd, 31.1786, 7th, 28.1964, 7th, 0.613, 16th, 0.2, 27th, 0.7, 22nd, 0.9, 8th, 0.485, 16th, 0.318, 16th, 3114, 17th, 1507, 23rd, 1607, 22nd, 48.4, 25th, 9, NA, 0.896, NA, NA, 24th, NA, 27th, NA, 23rd, 8, 8, Montréal Canadiens, Montréal Canadiens, /api/v1/teams/8, /api/v1/teams/8, statsSingleSeason, R, Regular season, FALSE
    ## 9                  56, NA, 23, 23rd, 28, 25th, 5, 22nd, 51, 23rd, 45.5, 23rd, 2.768, 20th, 3.375, 27th, 0.803, 24th, 15.5, 26th, 27, 25th, 36, 24th, 174, 10th, 79.0, 20th, 29.6964, 16th, 32.125, 27th, 0.667, 21st, 0.219, 20th, 0.867, 4th, 0.85, 19th, 0.409, 23rd, 0.452, 23rd, 3100, 21st, 1469, 28th, 1631, 25th, 47.4, 26th, 9.3, NA, 0.895, NA, NA, 22nd, NA, 28th, NA, 18th, 9, 9, Ottawa Senators, Ottawa Senators, /api/v1/teams/9, /api/v1/teams/9, statsSingleSeason, R, Regular season, FALSE
    ## 10                  56, NA, 35, 8th, 14, 5th, 7, 9th, 77, 5th, 68.8, 5th, 3.321, 6th, 2.643, 7th, 1.375, 3rd, 20.0, 16th, 31, 19th, 31, 13th, 155, 20th, 78.5, 24th, 31.2679, 6th, 27.8214, 5th, 0.735, 4th, 0.455, 13th, 0.692, 24th, 0.8, 24th, 0.559, 13th, 0.75, 13th, 2981, 27th, 1523, 18th, 1458, 3rd, 51.1, 10th, 10.6, NA, 0.905, NA, NA, 5th, NA, 15th, NA, 6th, 10, 10, Toronto Maple Leafs, Toronto Maple Leafs, /api/v1/teams/10, /api/v1/teams/10, statsSingleSeason, R, Regular season, FALSE
    ## 11                          56, NA, 36, 5th, 12, 1st, 8, 7th, 80, 3rd, 71.4, 3rd, 3.125, 11th, 2.393, 4th, 1.3086, 4th, 25.6, 2nd, 42, 3rd, 26, 4th, 164, 14th, 85.2, 3rd, 32.0357, 5th, 28.2321, 8th, 0.735, 3rd, 0.5, 9th, 0.81, 9th, 0.862, 16th, 0.639, 8th, 0.632, 8th, 3425, 1st, 1845, 1st, 1580, 19th, 53.9, 3rd, 9.8, NA, 0.915, NA, NA, 26th, NA, 3rd, NA, 12th, 12, 12, Carolina Hurricanes, Carolina Hurricanes, /api/v1/teams/12, /api/v1/teams/12, statsSingleSeason, R, Regular season, FALSE
    ## 12                          56, NA, 37, 3rd, 14, 4th, 5, 19th, 79, 4th, 70.5, 4th, 3.357, 4th, 2.696, 9th, 1.2553, 6th, 20.5, 15th, 39, 5th, 34, 20th, 190, 2nd, 79.8, 18th, 34.8929, 1st, 30.0357, 16th, 0.714, 13th, 0.607, 1st, 0.737, 17th, 0.929, 5th, 0.622, 9th, 0.75, 9th, 3330, 2nd, 1671, 5th, 1659, 26th, 50.2, 15th, 9.6, NA, 0.91, NA, NA, 19th, NA, 8th, NA, 15th, 13, 13, Florida Panthers, Florida Panthers, /api/v1/teams/13, /api/v1/teams/13, statsSingleSeason, R, Regular season, FALSE
    ## 13                          56, NA, 36, 7th, 17, 10th, 3, 26th, 75, 9th, 67.0, 9th, 3.214, 9th, 2.589, 6th, 1.1443, 10th, 22.2, 9th, 40, 4th, 29, 9th, 180, 3rd, 84.2, 4th, 30.2143, 9th, 28.2679, 9th, 0.786, 11th, 0.5, 3rd, 0.909, 1st, 1, 1st, 0.655, 7th, 0.64, 7th, 3127, 15th, 1567, 12th, 1560, 16th, 50.1, 16th, 10.6, NA, 0.908, NA, NA, 29th, NA, 9th, NA, 7th, 14, 14, Tampa Bay Lightning, Tampa Bay Lightning, /api/v1/teams/14, /api/v1/teams/14, statsSingleSeason, R, Regular season, FALSE
    ## 14                     56, NA, 36, 6th, 15, 6th, 5, 20th, 77, 6th, 68.8, 6th, 3.357, 5th, 2.875, 17th, 1.2336, 8th, 24.8, 3rd, 38, 6th, 26, 5th, 153, 24th, 84.0, 5th, 29.4107, 18th, 28.7857, 11th, 0.75, 6th, 0.5, 6th, 0.727, 20th, 0.929, 6th, 0.677, 5th, 0.545, 5th, 3134, 14th, 1542, 13th, 1592, 21st, 49.2, 22nd, 11.4, NA, 0.9, NA, NA, 15th, NA, 19th, NA, 2nd, 15, 15, Washington Capitals, Washington Capitals, /api/v1/teams/15, /api/v1/teams/15, statsSingleSeason, R, Regular season, FALSE
    ## 15           56, NA, 24, 20th, 25, 20th, 7, 12th, 55, 20th, 49.1, 20th, 2.839, 16th, 3.286, 24th, 0.8, 25th, 21.7, 11th, 38, 7th, 35, 23rd, 175, 6th, 76.8, 28th, 29.1964, 19th, 33.7143, 30th, 0.63, 20th, 0.241, 19th, 0.647, 26th, 0.789, 26th, 0.45, 21st, 0.412, 21st, 3105, 19th, 1439, 29th, 1666, 27th, 46.3, 29th, 9.7, NA, 0.903, NA, NA, 8th, NA, 17th, NA, 14th, 16, 16, Chicago Blackhawks, Chicago Blackhawks, /api/v1/teams/16, /api/v1/teams/16, statsSingleSeason, R, Regular season, FALSE
    ## 16           56, NA, 19, 27th, 27, 24th, 10, 4th, 48, 28th, 42.9, 28th, 2.232, 30th, 3, 20th, 0.7768, 29th, 11.4, 30th, 17, 30th, 33, 18th, 149, 27th, 78.7, 22nd, 27.2857, 30th, 31.8929, 25th, 0.5, 29th, 0.219, 22nd, 0.643, 27th, 0.833, 22nd, 0.318, 28th, 0.353, 28th, 3041, 25th, 1523, 19th, 1518, 10th, 50.1, 17th, 8.2, NA, 0.906, NA, NA, 12th, NA, 14th, NA, 31st, 17, 17, Detroit Red Wings, Detroit Red Wings, /api/v1/teams/17, /api/v1/teams/17, statsSingleSeason, R, Regular season, FALSE
    ## 17          56, NA, 31, 13th, 23, 16th, 2, 31st, 64, 13th, 57.1, 13th, 2.696, 22nd, 2.75, 12th, 1.1429, 11th, 17.6, 23rd, 28, 23rd, 42, 29th, 159, 17th, 75.6, 29th, 29.9821, 14th, 31.3036, 24th, 0.72, 17th, 0.419, 5th, 0.75, 13th, 0.895, 10th, 0.571, 12th, 0.519, 12th, 3149, 12th, 1628, 9th, 1521, 11th, 51.7, 7th, 9, NA, 0.912, NA, NA, 23rd, NA, 4th, NA, 22nd, 18, 18, Nashville Predators, Nashville Predators, /api/v1/teams/18, /api/v1/teams/18, statsSingleSeason, R, Regular season, FALSE
    ## 18                     56, NA, 27, 15th, 20, 14th, 9, 5th, 63, 14th, 56.3, 14th, 2.982, 13th, 2.982, 19th, 0.9273, 19th, 23.2, 6th, 36, 13th, 38, 28th, 155, 21st, 77.8, 25th, 28.9643, 23rd, 29.8214, 14th, 0.565, 26th, 0.424, 4th, 0.696, 23rd, 0.8, 25th, 0.5, 15th, 0.467, 15th, 3145, 13th, 1677, 4th, 1468, 5th, 53.3, 4th, 10.3, NA, 0.9, NA, NA, 21st, NA, 21st, NA, 9th, 19, 19, St. Louis Blues, St. Louis Blues, /api/v1/teams/19, /api/v1/teams/19, statsSingleSeason, R, Regular season, FALSE
    ## 19              56, NA, 26, 17th, 27, 23rd, 3, 28th, 55, 21st, 49.1, 21st, 2.768, 19th, 2.857, 16th, 1.0667, 16th, 18.3, 21st, 32, 16th, 34, 21st, 175, 7th, 80.2, 15th, 30.1607, 11th, 28.1607, 6th, 0.741, 14th, 0.207, 26th, 0.737, 18th, 0.957, 2nd, 0.471, 19th, 0.429, 19th, 3085, 22nd, 1541, 14th, 1544, 14th, 50.0, 18th, 9.2, NA, 0.899, NA, NA, 25th, NA, 23rd, NA, 20th, 20, 20, Calgary Flames, Calgary Flames, /api/v1/teams/20, /api/v1/teams/20, statsSingleSeason, R, Regular season, FALSE
    ## 20                          56, NA, 39, 2nd, 13, 2nd, 4, 23rd, 82, 1st, 73.2, 1st, 3.518, 1st, 2.357, 3rd, 1.4886, 1st, 22.7, 8th, 47, 2nd, 30, 11th, 207, 1st, 83.0, 8th, 34.5893, 2nd, 25.4107, 1st, 0.806, 2nd, 0.5, 12th, 0.87, 3rd, 0.939, 3rd, 0.733, 2nd, 0.545, 2nd, 3235, 4th, 1670, 6th, 1565, 17th, 51.6, 8th, 10.2, NA, 0.907, NA, NA, 27th, NA, 11th, NA, 10th, 21, 21, Colorado Avalanche, Colorado Avalanche, /api/v1/teams/21, /api/v1/teams/21, statsSingleSeason, R, Regular season, FALSE
    ## 21                       56, NA, 35, 10th, 19, 12th, 2, 30th, 72, 11th, 64.3, 11th, 3.268, 7th, 2.75, 11th, 0.9914, 18th, 27.6, 1st, 48, 1st, 27, 7th, 174, 9th, 82.5, 9th, 29.8929, 15th, 30.6607, 21st, 0.852, 9th, 0.414, 8th, 0.826, 8th, 0.897, 9th, 0.583, 11th, 0.633, 11th, 2977, 29th, 1501, 24th, 1476, 7th, 50.4, 14th, 10.9, NA, 0.91, NA, NA, 10th, NA, 7th, NA, 5th, 22, 22, Edmonton Oilers, Edmonton Oilers, /api/v1/teams/22, /api/v1/teams/22, statsSingleSeason, R, Regular season, FALSE
    ## 22           56, NA, 23, 24th, 29, 28th, 4, 24th, 50, 24th, 44.6, 24th, 2.643, 24th, 3.339, 26th, 0.7939, 27th, 17.4, 25th, 27, 26th, 37, 27th, 155, 22nd, 79.8, 17th, 29.0893, 20th, 33.3929, 29th, 0.72, 18th, 0.161, 28th, 0.706, 21st, 0.882, 12th, 0.357, 26th, 0.429, 26th, 3162, 10th, 1655, 8th, 1507, 9th, 52.3, 5th, 9.1, NA, 0.9, NA, NA, 30th, NA, 20th, NA, 21st, 23, 23, Vancouver Canucks, Vancouver Canucks, /api/v1/teams/23, /api/v1/teams/23, statsSingleSeason, R, Regular season, FALSE
    ## 23               56, NA, 17, 30th, 30, 30th, 9, 6th, 43, 30th, 38.4, 30th, 2.214, 31st, 3.161, 23rd, 0.8197, 22nd, 8.9, 31st, 11, 31st, 33, 19th, 123, 30th, 79.9, 16th, 26.7857, 31st, 30.5714, 19th, 0.444, 30th, 0.172, 30th, 0.529, 29th, 0.643, 31st, 0.316, 29th, 0.306, 29th, 3118, 16th, 1591, 10th, 1527, 12th, 51.0, 11th, 8.3, NA, 0.897, NA, NA, 16th, NA, 25th, NA, 29th, 24, 24, Anaheim Ducks, Anaheim Ducks, /api/v1/teams/24, /api/v1/teams/24, statsSingleSeason, R, Regular season, FALSE
    ## 24                            56, NA, 23, 22nd, 19, 13th, 14, 1st, 60, 16th, 53.6, 16th, 2.786, 18th, 2.643, 8th, 1.086, 14th, 23.6, 5th, 37, 9th, 32, 16th, 157, 18th, 79.1, 19th, 30.3393, 8th, 27.1071, 3rd, 0.654, 19th, 0.2, 25th, 0.75, 14th, 0.857, 17th, 0.457, 20th, 0.333, 20th, 3218, 5th, 1668, 7th, 1550, 15th, 51.8, 6th, 9.2, NA, 0.903, NA, NA, 9th, NA, 18th, NA, 19th, 25, 25, Dallas Stars, Dallas Stars, /api/v1/teams/25, /api/v1/teams/25, statsSingleSeason, R, Regular season, FALSE
    ## 25          56, NA, 21, 25th, 28, 26th, 7, 13th, 49, 25th, 43.8, 25th, 2.536, 27th, 3.018, 21st, 0.7983, 26th, 18.9, 19th, 32, 17th, 26, 6th, 169, 11th, 83.7, 7th, 28.3393, 27th, 31.1786, 23rd, 0.667, 25th, 0.2, 21st, 0.769, 11th, 0.875, 14th, 0.375, 25th, 0.333, 25th, 2981, 28th, 1509, 22nd, 1472, 6th, 50.6, 12th, 8.9, NA, 0.903, NA, NA, 13th, NA, 16th, NA, 25th, 26, 26, Los Angeles Kings, Los Angeles Kings, /api/v1/teams/26, /api/v1/teams/26, statsSingleSeason, R, Regular season, FALSE
    ## 26           56, NA, 21, 26th, 28, 27th, 7, 14th, 49, 26th, 43.8, 26th, 2.607, 25th, 3.5, 29th, 0.7836, 28th, 14.1, 29th, 22, 27th, 36, 25th, 156, 19th, 80.4, 14th, 30.0357, 13th, 31.9821, 26th, 0.464, 27th, 0.286, 18th, 0.688, 25th, 0.688, 29th, 0.478, 18th, 0.333, 18th, 3156, 11th, 1528, 16th, 1628, 24th, 48.4, 24th, 8.7, NA, 0.891, NA, NA, 31st, NA, 30th, NA, 27th, 28, 28, San Jose Sharks, San Jose Sharks, /api/v1/teams/28, /api/v1/teams/28, statsSingleSeason, R, Regular season, FALSE
    ## 27 56, NA, 18, 29th, 26, 22nd, 12, 2nd, 48, 27th, 42.9, 27th, 2.393, 28th, 3.286, 25th, 0.7405, 30th, 15.4, 27th, 18, 29th, 28, 8th, 117, 31st, 79.0, 21st, 29.0179, 21st, 32.4107, 28th, 0.406, 28th, 0.208, 29th, 0.45, 30th, 0.75, 27th, 0.273, 30th, 0.357, 30th, 3061, 23rd, 1387, 30th, 1674, 28th, 45.3, 30th, 8.2, NA, 0.899, NA, NA, 1st, NA, 22nd, NA, 30th, 29, 29, Columbus Blue Jackets, Columbus Blue Jackets, /api/v1/teams/29, /api/v1/teams/29, statsSingleSeason, R, Regular season, FALSE
    ## 28                      56, NA, 35, 9th, 16, 8th, 5, 21st, 75, 8th, 67.0, 8th, 3.214, 8th, 2.839, 15th, 1.1667, 9th, 17.6, 24th, 29, 21st, 31, 14th, 165, 13th, 80.8, 12th, 28.3036, 28th, 30.4464, 17th, 0.767, 8th, 0.462, 7th, 0.739, 15th, 0.875, 13th, 0.714, 3rd, 0.517, 3rd, 3261, 3rd, 1517, 20th, 1744, 31st, 46.5, 28th, 11.4, NA, 0.907, NA, NA, 14th, NA, 13th, NA, 3rd, 30, 30, Minnesota Wild, Minnesota Wild, /api/v1/teams/30, /api/v1/teams/30, statsSingleSeason, R, Regular season, FALSE
    ## 29                 56, NA, 30, 14th, 23, 17th, 3, 27th, 63, 15th, 56.3, 15th, 3.036, 12th, 2.714, 10th, 1.0784, 15th, 23.0, 7th, 37, 8th, 29, 10th, 161, 15th, 80.5, 13th, 29.6607, 17th, 30.5893, 20th, 0.679, 15th, 0.393, 10th, 0.762, 12th, 0.864, 15th, 0.483, 17th, 0.625, 17th, 2957, 30th, 1492, 26th, 1465, 4th, 50.5, 13th, 10.2, NA, 0.911, NA, NA, 7th, NA, 6th, NA, 11th, 52, 52, Winnipeg Jets, Winnipeg Jets, /api/v1/teams/52, /api/v1/teams/52, statsSingleSeason, R, Regular season, FALSE
    ## 30            56, NA, 24, 21st, 26, 21st, 6, 18th, 54, 22nd, 48.2, 22nd, 2.679, 23rd, 3.107, 22nd, 0.8696, 20th, 20.8, 13th, 37, 11th, 32, 17th, 178, 5th, 80.8, 11th, 27.4643, 29th, 30.4643, 18th, 0.609, 24th, 0.303, 14th, 0.615, 28th, 0.857, 18th, 0.4, 24th, 0.441, 24th, 3108, 18th, 1525, 17th, 1583, 20th, 49.1, 23rd, 9.8, NA, 0.898, NA, NA, 18th, NA, 24th, NA, 13th, 53, 53, Arizona Coyotes, Arizona Coyotes, /api/v1/teams/53, /api/v1/teams/53, statsSingleSeason, R, Regular season, FALSE
    ## 31                    56, NA, 40, 1st, 14, 3rd, 2, 29th, 82, 2nd, 73.2, 2nd, 3.393, 3rd, 2.179, 1st, 1.3776, 2nd, 17.8, 22nd, 31, 18th, 19, 1st, 174, 8th, 86.8, 1st, 32.6607, 4th, 27.2679, 4th, 0.861, 1st, 0.45, 15th, 0.846, 5th, 0.931, 4th, 0.769, 1st, 0.667, 1st, 3102, 20th, 1536, 15th, 1566, 18th, 49.5, 20th, 10.4, NA, 0.92, NA, NA, 4th, NA, 2nd, NA, 8th, 54, 54, Vegas Golden Knights, Vegas Golden Knights, /api/v1/teams/54, /api/v1/teams/54, statsSingleSeason, R, Regular season, FALSE
    ## 32                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      NULL
    ##    teams.shortName              teams.officialSiteUrl teams.franchiseId
    ## 1       New Jersey    http://www.newjerseydevils.com/                23
    ## 2     NY Islanders   http://www.newyorkislanders.com/                22
    ## 3       NY Rangers     http://www.newyorkrangers.com/                10
    ## 4     Philadelphia http://www.philadelphiaflyers.com/                16
    ## 5       Pittsburgh     http://pittsburghpenguins.com/                17
    ## 6           Boston       http://www.bostonbruins.com/                 6
    ## 7          Buffalo             http://www.sabres.com/                19
    ## 8         Montréal          http://www.canadiens.com/                 1
    ## 9           Ottawa     http://www.ottawasenators.com/                30
    ## 10         Toronto         http://www.mapleleafs.com/                 5
    ## 11        Carolina http://www.carolinahurricanes.com/                26
    ## 12         Florida    http://www.floridapanthers.com/                33
    ## 13       Tampa Bay  http://www.tampabaylightning.com/                31
    ## 14      Washington http://www.washingtoncapitals.com/                24
    ## 15         Chicago  http://www.chicagoblackhawks.com/                11
    ## 16         Detroit    http://www.detroitredwings.com/                12
    ## 17       Nashville http://www.nashvillepredators.com/                34
    ## 18        St Louis       http://www.stlouisblues.com/                18
    ## 19         Calgary      http://www.calgaryflames.com/                21
    ## 20        Colorado  http://www.coloradoavalanche.com/                27
    ## 21        Edmonton     http://www.edmontonoilers.com/                25
    ## 22       Vancouver            http://www.canucks.com/                20
    ## 23         Anaheim       http://www.anaheimducks.com/                32
    ## 24          Dallas        http://www.dallasstars.com/                15
    ## 25     Los Angeles            http://www.lakings.com/                14
    ## 26        San Jose           http://www.sjsharks.com/                29
    ## 27        Columbus        http://www.bluejackets.com/                36
    ## 28       Minnesota               http://www.wild.com/                37
    ## 29        Winnipeg           http://winnipegjets.com/                35
    ## 30         Arizona     http://www.arizonacoyotes.com/                28
    ## 31           Vegas http://www.vegasgoldenknights.com/                38
    ## 32            <NA>        https://www.nhl.com/seattle                39
    ##    teams.active                  teams.venue.name    teams.venue.link
    ## 1          TRUE                 Prudential Center /api/v1/venues/null
    ## 2          TRUE Nassau Veterans Memorial Coliseum /api/v1/venues/null
    ## 3          TRUE             Madison Square Garden /api/v1/venues/5054
    ## 4          TRUE                Wells Fargo Center /api/v1/venues/5096
    ## 5          TRUE                  PPG Paints Arena /api/v1/venues/5034
    ## 6          TRUE                         TD Garden /api/v1/venues/5085
    ## 7          TRUE                    KeyBank Center /api/v1/venues/5039
    ## 8          TRUE                       Bell Centre /api/v1/venues/5028
    ## 9          TRUE              Canadian Tire Centre /api/v1/venues/5031
    ## 10         TRUE                  Scotiabank Arena /api/v1/venues/null
    ## 11         TRUE                         PNC Arena /api/v1/venues/5066
    ## 12         TRUE                       BB&T Center /api/v1/venues/5027
    ## 13         TRUE                      AMALIE Arena /api/v1/venues/null
    ## 14         TRUE                 Capital One Arena /api/v1/venues/5094
    ## 15         TRUE                     United Center /api/v1/venues/5092
    ## 16         TRUE              Little Caesars Arena /api/v1/venues/5145
    ## 17         TRUE                 Bridgestone Arena /api/v1/venues/5030
    ## 18         TRUE                 Enterprise Center /api/v1/venues/5076
    ## 19         TRUE             Scotiabank Saddledome /api/v1/venues/5075
    ## 20         TRUE                        Ball Arena /api/v1/venues/5064
    ## 21         TRUE                      Rogers Place /api/v1/venues/5100
    ## 22         TRUE                      Rogers Arena /api/v1/venues/5073
    ## 23         TRUE                      Honda Center /api/v1/venues/5046
    ## 24         TRUE          American Airlines Center /api/v1/venues/5019
    ## 25         TRUE                    STAPLES Center /api/v1/venues/5081
    ## 26         TRUE            SAP Center at San Jose /api/v1/venues/null
    ## 27         TRUE                  Nationwide Arena /api/v1/venues/5059
    ## 28         TRUE                Xcel Energy Center /api/v1/venues/5098
    ## 29         TRUE                    Bell MTS Place /api/v1/venues/5058
    ## 30         TRUE                  Gila River Arena /api/v1/venues/5043
    ## 31         TRUE                    T-Mobile Arena /api/v1/venues/5178
    ## 32        FALSE                              <NA>                <NA>
    ##    teams.venue.city teams.venue.id teams.venue.timeZone.id
    ## 1            Newark             NA        America/New_York
    ## 2         Uniondale             NA        America/New_York
    ## 3          New York           5054        America/New_York
    ## 4      Philadelphia           5096        America/New_York
    ## 5        Pittsburgh           5034        America/New_York
    ## 6            Boston           5085        America/New_York
    ## 7           Buffalo           5039        America/New_York
    ## 8          Montréal           5028        America/Montreal
    ## 9            Ottawa           5031        America/New_York
    ## 10          Toronto             NA         America/Toronto
    ## 11          Raleigh           5066        America/New_York
    ## 12          Sunrise           5027        America/New_York
    ## 13            Tampa             NA        America/New_York
    ## 14       Washington           5094        America/New_York
    ## 15          Chicago           5092         America/Chicago
    ## 16          Detroit           5145         America/Detroit
    ## 17        Nashville           5030         America/Chicago
    ## 18        St. Louis           5076         America/Chicago
    ## 19          Calgary           5075          America/Denver
    ## 20           Denver           5064          America/Denver
    ## 21         Edmonton           5100        America/Edmonton
    ## 22        Vancouver           5073       America/Vancouver
    ## 23          Anaheim           5046     America/Los_Angeles
    ## 24           Dallas           5019         America/Chicago
    ## 25      Los Angeles           5081     America/Los_Angeles
    ## 26         San Jose             NA     America/Los_Angeles
    ## 27         Columbus           5059        America/New_York
    ## 28         St. Paul           5098         America/Chicago
    ## 29         Winnipeg           5058        America/Winnipeg
    ## 30         Glendale           5043         America/Phoenix
    ## 31        Las Vegas           5178     America/Los_Angeles
    ## 32             <NA>             NA                    <NA>
    ##    teams.venue.timeZone.offset teams.venue.timeZone.tz teams.division.id
    ## 1                           -4                     EDT                25
    ## 2                           -4                     EDT                25
    ## 3                           -4                     EDT                25
    ## 4                           -4                     EDT                25
    ## 5                           -4                     EDT                25
    ## 6                           -4                     EDT                25
    ## 7                           -4                     EDT                25
    ## 8                           -4                     EDT                28
    ## 9                           -4                     EDT                28
    ## 10                          -4                     EDT                28
    ## 11                          -4                     EDT                26
    ## 12                          -4                     EDT                26
    ## 13                          -4                     EDT                26
    ## 14                          -4                     EDT                25
    ## 15                          -5                     CDT                26
    ## 16                          -4                     EDT                26
    ## 17                          -5                     CDT                26
    ## 18                          -5                     CDT                27
    ## 19                          -6                     MDT                28
    ## 20                          -6                     MDT                27
    ## 21                          -6                     MDT                28
    ## 22                          -7                     PDT                28
    ## 23                          -7                     PDT                27
    ## 24                          -5                     CDT                26
    ## 25                          -7                     PDT                27
    ## 26                          -7                     PDT                27
    ## 27                          -4                     EDT                26
    ## 28                          -5                     CDT                27
    ## 29                          -5                     CDT                28
    ## 30                          -7                     MST                27
    ## 31                          -7                     PDT                27
    ## 32                          NA                    <NA>                NA
    ##    teams.division.name    teams.division.link teams.conference.id
    ## 1      MassMutual East   /api/v1/divisions/25                   6
    ## 2      MassMutual East   /api/v1/divisions/25                   6
    ## 3      MassMutual East   /api/v1/divisions/25                   6
    ## 4      MassMutual East   /api/v1/divisions/25                   6
    ## 5      MassMutual East   /api/v1/divisions/25                   6
    ## 6      MassMutual East   /api/v1/divisions/25                   6
    ## 7      MassMutual East   /api/v1/divisions/25                   6
    ## 8         Scotia North   /api/v1/divisions/28                   6
    ## 9         Scotia North   /api/v1/divisions/28                   6
    ## 10        Scotia North   /api/v1/divisions/28                   6
    ## 11    Discover Central   /api/v1/divisions/26                   6
    ## 12    Discover Central   /api/v1/divisions/26                   6
    ## 13    Discover Central   /api/v1/divisions/26                   6
    ## 14     MassMutual East   /api/v1/divisions/25                   6
    ## 15    Discover Central   /api/v1/divisions/26                   5
    ## 16    Discover Central   /api/v1/divisions/26                   6
    ## 17    Discover Central   /api/v1/divisions/26                   5
    ## 18          Honda West   /api/v1/divisions/27                   5
    ## 19        Scotia North   /api/v1/divisions/28                   5
    ## 20          Honda West   /api/v1/divisions/27                   5
    ## 21        Scotia North   /api/v1/divisions/28                   5
    ## 22        Scotia North   /api/v1/divisions/28                   5
    ## 23          Honda West   /api/v1/divisions/27                   5
    ## 24    Discover Central   /api/v1/divisions/26                   5
    ## 25          Honda West   /api/v1/divisions/27                   5
    ## 26          Honda West   /api/v1/divisions/27                   5
    ## 27    Discover Central   /api/v1/divisions/26                   6
    ## 28          Honda West   /api/v1/divisions/27                   5
    ## 29        Scotia North   /api/v1/divisions/28                   5
    ## 30          Honda West   /api/v1/divisions/27                   5
    ## 31          Honda West   /api/v1/divisions/27                   5
    ## 32                <NA> /api/v1/divisions/null                  NA
    ##    teams.conference.name    teams.conference.link
    ## 1                Eastern    /api/v1/conferences/6
    ## 2                Eastern    /api/v1/conferences/6
    ## 3                Eastern    /api/v1/conferences/6
    ## 4                Eastern    /api/v1/conferences/6
    ## 5                Eastern    /api/v1/conferences/6
    ## 6                Eastern    /api/v1/conferences/6
    ## 7                Eastern    /api/v1/conferences/6
    ## 8                Eastern    /api/v1/conferences/6
    ## 9                Eastern    /api/v1/conferences/6
    ## 10               Eastern    /api/v1/conferences/6
    ## 11               Eastern    /api/v1/conferences/6
    ## 12               Eastern    /api/v1/conferences/6
    ## 13               Eastern    /api/v1/conferences/6
    ## 14               Eastern    /api/v1/conferences/6
    ## 15               Western    /api/v1/conferences/5
    ## 16               Eastern    /api/v1/conferences/6
    ## 17               Western    /api/v1/conferences/5
    ## 18               Western    /api/v1/conferences/5
    ## 19               Western    /api/v1/conferences/5
    ## 20               Western    /api/v1/conferences/5
    ## 21               Western    /api/v1/conferences/5
    ## 22               Western    /api/v1/conferences/5
    ## 23               Western    /api/v1/conferences/5
    ## 24               Western    /api/v1/conferences/5
    ## 25               Western    /api/v1/conferences/5
    ## 26               Western    /api/v1/conferences/5
    ## 27               Eastern    /api/v1/conferences/6
    ## 28               Western    /api/v1/conferences/5
    ## 29               Western    /api/v1/conferences/5
    ## 30               Western    /api/v1/conferences/5
    ## 31               Western    /api/v1/conferences/5
    ## 32                  <NA> /api/v1/conferences/null
    ##    teams.franchise.franchiseId teams.franchise.teamName
    ## 1                           23                   Devils
    ## 2                           22                Islanders
    ## 3                           10                  Rangers
    ## 4                           16                   Flyers
    ## 5                           17                 Penguins
    ## 6                            6                   Bruins
    ## 7                           19                   Sabres
    ## 8                            1                Canadiens
    ## 9                           30                 Senators
    ## 10                           5              Maple Leafs
    ## 11                          26               Hurricanes
    ## 12                          33                 Panthers
    ## 13                          31                Lightning
    ## 14                          24                 Capitals
    ## 15                          11               Blackhawks
    ## 16                          12                Red Wings
    ## 17                          34                Predators
    ## 18                          18                    Blues
    ## 19                          21                   Flames
    ## 20                          27                Avalanche
    ## 21                          25                   Oilers
    ## 22                          20                  Canucks
    ## 23                          32                    Ducks
    ## 24                          15                    Stars
    ## 25                          14                    Kings
    ## 26                          29                   Sharks
    ## 27                          36             Blue Jackets
    ## 28                          37                     Wild
    ## 29                          35                     Jets
    ## 30                          28                  Coyotes
    ## 31                          38           Golden Knights
    ## 32                          39                   Kraken
    ##     teams.franchise.link
    ## 1  /api/v1/franchises/23
    ## 2  /api/v1/franchises/22
    ## 3  /api/v1/franchises/10
    ## 4  /api/v1/franchises/16
    ## 5  /api/v1/franchises/17
    ## 6   /api/v1/franchises/6
    ## 7  /api/v1/franchises/19
    ## 8   /api/v1/franchises/1
    ## 9  /api/v1/franchises/30
    ## 10  /api/v1/franchises/5
    ## 11 /api/v1/franchises/26
    ## 12 /api/v1/franchises/33
    ## 13 /api/v1/franchises/31
    ## 14 /api/v1/franchises/24
    ## 15 /api/v1/franchises/11
    ## 16 /api/v1/franchises/12
    ## 17 /api/v1/franchises/34
    ## 18 /api/v1/franchises/18
    ## 19 /api/v1/franchises/21
    ## 20 /api/v1/franchises/27
    ## 21 /api/v1/franchises/25
    ## 22 /api/v1/franchises/20
    ## 23 /api/v1/franchises/32
    ## 24 /api/v1/franchises/15
    ## 25 /api/v1/franchises/14
    ## 26 /api/v1/franchises/29
    ## 27 /api/v1/franchises/36
    ## 28 /api/v1/franchises/37
    ## 29 /api/v1/franchises/35
    ## 30 /api/v1/franchises/28
    ## 31 /api/v1/franchises/38
    ## 32 /api/v1/franchises/39

## wrapper function

``` r
wrapper <- function (modifier, id,name,...) {
  if (modifier == "franchise") {
    franchise_Data <- franchise (id,name)
    franchise_Data
  }
    else if (modifier == "totals"){
     total_Data <- total (id,name)
     total_Data
    }
      else if (modifier == "season") {
       season_Data <- season(id)
       season_Data
      }
        else if (modifier == "goalie"){
         goalie_Data <- goalie (id)
         goalie_Data
        }
          else if (modifier == "skater"){
            skater_Data <- skater (id)
            skater_Data
          }
            else if (modifier == "detail") {
              detail_Data <-  detail (id)
              detail_Data
            }
              else if (modifier == "stats"){
                stas_Data <- stats (id)
                stas_Data
              }
}
result1 <- wrapper ("franchise", 2, "Montreal Wanderers")
result1
```

    ##   data.id data.firstSeasonId      data.fullName data.lastSeasonId
    ## 1       2           19171918 Montreal Wanderers          19171918
    ##   data.mostRecentTeamId data.teamAbbrev data.teamCommonName
    ## 1                    41             MWN           Wanderers
    ##   data.teamPlaceName total
    ## 1           Montreal    39

``` r
result2 <- wrapper ("stats", 2)
result2
```

    ##                                                                                                                                                                            copyright
    ## 1 NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2021. All Rights Reserved.
    ##   teams.id         teams.name      teams.link teams.abbreviation
    ## 1        2 New York Islanders /api/v1/teams/2                NYI
    ##   teams.teamName teams.locationName teams.firstYearOfPlay
    ## 1      Islanders           New York                  1972
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                          teams.teamStats
    ## 1 56, NA, 32, 12th, 17, 11th, 7, 11th, 71, 12th, 63.4, 12th, 2.714, 21st, 2.232, 2nd, 1.2418, 7th, 18.8, 20th, 27, 24th, 22, 2nd, 144, 28th, 83.7, 6th, 28.9821, 22nd, 28.3929, 10th, 0.821, 10th, 0.321, 16th, 0.833, 7th, 0.842, 20th, 0.69, 4th, 0.44, 4th, 2916, 31st, 1498, 25th, 1418, 2nd, 51.4, 9th, 9.4, NA, 0.921, NA, NA, 2nd, NA, 1st, NA, 17th, 2, 2, New York Islanders, New York Islanders, /api/v1/teams/2, /api/v1/teams/2, statsSingleSeason, R, Regular season, FALSE
    ##   teams.shortName            teams.officialSiteUrl teams.franchiseId
    ## 1    NY Islanders http://www.newyorkislanders.com/                22
    ##   teams.active                  teams.venue.name    teams.venue.link
    ## 1         TRUE Nassau Veterans Memorial Coliseum /api/v1/venues/null
    ##   teams.venue.city teams.venue.timeZone.id teams.venue.timeZone.offset
    ## 1        Uniondale        America/New_York                          -4
    ##   teams.venue.timeZone.tz teams.division.id teams.division.name
    ## 1                     EDT                25     MassMutual East
    ##    teams.division.link teams.conference.id teams.conference.name
    ## 1 /api/v1/divisions/25                   6               Eastern
    ##   teams.conference.link teams.franchise.franchiseId
    ## 1 /api/v1/conferences/6                          22
    ##   teams.franchise.teamName  teams.franchise.link
    ## 1                Islanders /api/v1/franchises/22

## Exploratory Data analysis

``` r
goalie_data <- wrapper ("goalie", 6)
goalie_data <- data.frame(goalie_data)
goalie_data <- goalie_data %>% select( data.franchiseId   , data.playerId,data.activePlayer, data.gameTypeId , data.gamesPlayed ,  data.losses, data.mostGoalsAgainstOneGame, data.seasons, data.mostSavesOneGame ,data.mostShotsAgainstOneGame, data.mostShutoutsOneSeason, data.mostWinsOneSeason ,  data.overtimeLosses,   data.positionCode    , data.seasons ,data.shutouts   , data.ties  , data.wins)   
goalie_data
```

    ##    data.franchiseId data.playerId data.activePlayer data.gameTypeId
    ## 1                 6       8445403             FALSE               2
    ## 2                 6       8445462             FALSE               2
    ## 3                 6       8445470             FALSE               2
    ## 4                 6       8446011             FALSE               2
    ## 5                 6       8446082             FALSE               2
    ## 6                 6       8447170             FALSE               2
    ## 7                 6       8447344             FALSE               2
    ## 8                 6       8447449             FALSE               2
    ## 9                 6       8448410             FALSE               2
    ## 10                6       8448759             FALSE               2
    ## 11                6       8449681             FALSE               2
    ## 12                6       8449823             FALSE               2
    ## 13                6       8449836             FALSE               2
    ## 14                6       8449851             FALSE               2
    ## 15                6       8449853             FALSE               2
    ## 16                6       8449861             FALSE               2
    ## 17                6       8449923             FALSE               2
    ## 18                6       8449960             FALSE               2
    ## 19                6       8449977             FALSE               2
    ## 20                6       8449983             FALSE               2
    ## 21                6       8449993             FALSE               2
    ## 22                6       8450005             FALSE               2
    ## 23                6       8450017             FALSE               2
    ## 24                6       8450019             FALSE               2
    ## 25                6       8450054             FALSE               2
    ## 26                6       8450066             FALSE               2
    ## 27                6       8450111             FALSE               2
    ## 28                6       8450127             FALSE               2
    ## 29                6       8450150             FALSE               2
    ## 30                6       8450343             FALSE               2
    ## 31                6       8450834             FALSE               2
    ## 32                6       8450835             FALSE               2
    ## 33                6       8450993             FALSE               2
    ## 34                6       8452122             FALSE               2
    ## 35                6       8452440             FALSE               2
    ## 36                6       8455633             FALSE               2
    ## 37                6       8455994             FALSE               2
    ## 38                6       8457714             FALSE               2
    ## 39                6       8458615             FALSE               2
    ## 40                6       8458968             FALSE               2
    ## 41                6       8458988             FALSE               2
    ## 42                6       8460612             FALSE               2
    ## 43                6       8460703             FALSE               2
    ## 44                6       8464715             FALSE               2
    ## 45                6       8467364             FALSE               2
    ## 46                6       8467913             FALSE               2
    ## 47                6       8469732             FALSE               2
    ## 48                6       8470860              TRUE               2
    ## 49                6       8471695              TRUE               2
    ## 50                6       8473434             FALSE               2
    ## 51                6       8475361             FALSE               2
    ##    data.gamesPlayed data.losses data.mostGoalsAgainstOneGame data.seasons
    ## 1                 8           0                            5            1
    ## 2                 8           4                            4            1
    ## 3                35          14                            6            2
    ## 4                57          15                            7            1
    ## 5                 2           1                            4            1
    ## 6               277          73                            9            7
    ## 7                40           6                            7            1
    ## 8                18           9                            5            1
    ## 9               154          46                            8            5
    ## 10              183          62                            8            6
    ## 11              261          75                            7            6
    ## 12               43          22                           11            2
    ## 13              444         144                           10            9
    ## 14                4           3                            9            1
    ## 15              416         103                            8           12
    ## 16                2           1                            6            1
    ## 17                1           1                            6            1
    ## 18               41          19                           13            1
    ## 19              141          62                           10            3
    ## 20                1           1                           10            1
    ## 21              237          99                           10            4
    ## 22              444         192                           11           11
    ## 23                2           2                            8            1
    ## 24               77          33                            8            3
    ## 25               23          11                           10            1
    ## 26                8           1                            5            1
    ## 27              102          43                            7            2
    ## 28              468         153                            9           11
    ## 29               67          22                            7            2
    ## 30              171          58                            7            4
    ## 31               29          12                            7            2
    ## 32               49          16                            6            2
    ## 33                1           1                            6            1
    ## 34               91          30                            7            2
    ## 35                5           2                            6            1
    ## 36               41          16                            7            2
    ## 37              283         104                            8            5
    ## 38               28           8                            6            1
    ## 39               36          13                            8            1
    ## 40               29          15                            6            2
    ## 41               32          10                            6            2
    ## 42                5           2                            5            1
    ## 43              378         121                            8            8
    ## 44               25          12                            6            1
    ## 45                2           0                            2            1
    ## 46               23           7                            4            1
    ## 47                7           2                            4            1
    ## 48               90          23                            7            3
    ## 49              560         163                            7           14
    ## 50               27           4                            5            1
    ## 51               24           9                            5            1
    ##    data.mostSavesOneGame data.mostShotsAgainstOneGame
    ## 1                     24                           27
    ## 2                     26                           28
    ## 3                     34                           36
    ## 4                     31                           36
    ## 5                     15                           19
    ## 6                     41                           44
    ## 7                     27                           31
    ## 8                     43                           47
    ## 9                     37                           40
    ## 10                    39                           42
    ## 11                    41                           44
    ## 12                    NA                           NA
    ## 13                    NA                           NA
    ## 14                    37                           43
    ## 15                    45                           48
    ## 16                    NA                           NA
    ## 17                    NA                           NA
    ## 18                    NA                           NA
    ## 19                    NA                           NA
    ## 20                    NA                           NA
    ## 21                    NA                           NA
    ## 22                    48                           56
    ## 23                    NA                           NA
    ## 24                    43                           48
    ## 25                    47                           57
    ## 26                    33                           34
    ## 27                    45                           49
    ## 28                    NA                           NA
    ## 29                    NA                           NA
    ## 30                    39                           44
    ## 31                    37                           39
    ## 32                    41                           42
    ## 33                    28                           34
    ## 34                    33                           37
    ## 35                    29                           33
    ## 36                    40                           42
    ## 37                    40                           44
    ## 38                    37                           38
    ## 39                    39                           44
    ## 40                    40                           43
    ## 41                    33                           38
    ## 42                    25                           27
    ## 43                    51                           55
    ## 44                    42                           46
    ## 45                    13                           15
    ## 46                    44                           45
    ## 47                    37                           40
    ## 48                    42                           44
    ## 49                    49                           52
    ## 50                    39                           41
    ## 51                    42                           42
    ##    data.mostShutoutsOneSeason data.mostWinsOneSeason data.overtimeLosses
    ## 1                           0                      2                  NA
    ## 2                           0                      1                  NA
    ## 3                           1                     10                  NA
    ## 4                           4                     30                  NA
    ## 5                           0                      0                  NA
    ## 6                           6                     34                  NA
    ## 7                           3                     26                  NA
    ## 8                           1                      8                  NA
    ## 9                           2                     19                  NA
    ## 10                          3                     24                  NA
    ## 11                          4                     37                  NA
    ## 12                          2                      8                  NA
    ## 13                         10                     33                  NA
    ## 14                          0                      0                  NA
    ## 15                          4                     30                  NA
    ## 16                          1                      1                  NA
    ## 17                          0                      0                  NA
    ## 18                          1                     17                  NA
    ## 19                          4                     22                  NA
    ## 20                          0                      0                  NA
    ## 21                          8                     32                  NA
    ## 22                          6                     30                  NA
    ## 23                          0                      0                  NA
    ## 24                          3                     16                  NA
    ## 25                          1                     10                  NA
    ## 26                          2                      7                  NA
    ## 27                          9                     22                  NA
    ## 28                         12                     38                  NA
    ## 29                         15                     20                  NA
    ## 30                          8                     40                  NA
    ## 31                          1                      7                  NA
    ## 32                          1                     17                  NA
    ## 33                          0                      0                  NA
    ## 34                          1                     25                  NA
    ## 35                          0                      1                  NA
    ## 36                          1                      9                  NA
    ## 37                         10                     35                  NA
    ## 38                          4                     12                  NA
    ## 39                          0                     12                  NA
    ## 40                          2                      5                  NA
    ## 41                          1                     16                   3
    ## 42                          0                      2                   0
    ## 43                          9                     36                  45
    ## 44                          0                      6                  NA
    ## 45                          0                      0                   0
    ## 46                          2                      9                   5
    ## 47                          0                      2                   1
    ## 48                          5                     22                  14
    ## 49                          8                     37                  66
    ## 50                          2                     17                   3
    ## 51                          1                     11                   1
    ##    data.positionCode data.shutouts data.ties data.wins
    ## 1                  G             0         3         2
    ## 2                  G             0         2         1
    ## 3                  G             1         3        15
    ## 4                  G             4         9        30
    ## 5                  G             0         0         0
    ## 6                  G            16        39       155
    ## 7                  G             3         7        26
    ## 8                  G             1         0         8
    ## 9                  G             4        13        83
    ## 10                 G             6        17        92
    ## 11                 G            13        36       136
    ## 12                 G             2         6        14
    ## 13                 G            35        70       230
    ## 14                 G             0         1         0
    ## 15                 G            26        76       226
    ## 16                 G             1         0         1
    ## 17                 G             0         0         0
    ## 18                 G             1         5        17
    ## 19                 G             7        33        46
    ## 20                 G             0         0         0
    ## 21                 G            24        44        93
    ## 22                 G            27        54       182
    ## 23                 G             0         0         0
    ## 24                 G             6         9        35
    ## 25                 G             1         2        10
    ## 26                 G             2         0         7
    ## 27                 G            11        19        40
    ## 28                 G            74        63       252
    ## 29                 G            19        13        32
    ## 30                 G             9        16        91
    ## 31                 G             1         2        10
    ## 32                 G             1         9        20
    ## 33                 G             0         0         0
    ## 34                 G             2        12        44
    ## 35                 G             0         0         1
    ## 36                 G             1         7        14
    ## 37                 G            25        40       132
    ## 38                 G             4         6        12
    ## 39                 G             0         9        12
    ## 40                 G             2         1         8
    ## 41                 G             2        NA        18
    ## 42                 G             0        NA         2
    ## 43                 G            31         0       196
    ## 44                 G             0         1         6
    ## 45                 G             0        NA         0
    ## 46                 G             2        NA         9
    ## 47                 G             0        NA         2
    ## 48                 G            10         0        49
    ## 49                 G            52         0       306
    ## 50                 G             2        NA        17
    ## 51                 G             1        NA        11

``` r
skater_data <- wrapper ("skater", 6)
skater_data <- data.frame (skater_data) %>% 
  select( data.franchiseId, data.playerId, data.activePlayer , data.assists   ,data.gameTypeId , data.goals,data.mostAssistsOneGame  ,  data.mostAssistsOneSeason  ,                data.gamesPlayed , data.mostGoalsOneGame, data.mostPenaltyMinutesOneSeason, data.mostPointsOneGame , data.mostPointsOneSeason  , data.penaltyMinutes , 
        data.points,data.positionCode, data.seasons    ) 
skater_data 
```

    ##    data.franchiseId data.playerId data.activePlayer data.assists
    ## 1                 6       8444899             FALSE            0
    ## 2                 6       8444966             FALSE            1
    ## 3                 6       8444980             FALSE            3
    ## 4                 6       8444984             FALSE            0
    ## 5                 6       8445005             FALSE            2
    ## 6                 6       8445009             FALSE            0
    ## 7                 6       8445061             FALSE            2
    ## 8                 6       8445068             FALSE            0
    ## 9                 6       8445074             FALSE            0
    ## 10                6       8445084             FALSE            5
    ## 11                6       8445152             FALSE            0
    ## 12                6       8445153             FALSE            0
    ## 13                6       8445239             FALSE            0
    ## 14                6       8445301             FALSE            1
    ## 15                6       8445306             FALSE            1
    ## 16                6       8445340             FALSE            0
    ## 17                6       8445348             FALSE            0
    ## 18                6       8445366             FALSE            0
    ## 19                6       8445377             FALSE            0
    ## 20                6       8445425             FALSE            0
    ## 21                6       8445488             FALSE            0
    ## 22                6       8445573             FALSE            0
    ## 23                6       8445591             FALSE            2
    ## 24                6       8445743             FALSE            0
    ## 25                6       8445747             FALSE            0
    ## 26                6       8445816             FALSE            0
    ## 27                6       8445848             FALSE            1
    ## 28                6       8445852             FALSE            0
    ## 29                6       8445863             FALSE            0
    ## 30                6       8445867             FALSE            0
    ## 31                6       8445901             FALSE            4
    ## 32                6       8445917             FALSE            0
    ## 33                6       8446095             FALSE            1
    ## 34                6       8446117             FALSE            4
    ## 35                6       8446234             FALSE            0
    ## 36                6       8446245             FALSE            0
    ## 37                6       8446297             FALSE            0
    ## 38                6       8446452             FALSE            1
    ## 39                6       8446504             FALSE            0
    ## 40                6       8446509             FALSE            0
    ## 41                6       8446514             FALSE            2
    ## 42                6       8446515             FALSE            0
    ## 43                6       8446574             FALSE            2
    ## 44                6       8446585             FALSE            1
    ## 45                6       8446643             FALSE            0
    ## 46                6       8446651             FALSE            2
    ## 47                6       8446654             FALSE            0
    ## 48                6       8446702             FALSE            0
    ## 49                6       8446835             FALSE            0
    ## 50                6       8446863             FALSE            0
    ## 51                6       8446869             FALSE            0
    ## 52                6       8446887             FALSE            0
    ## 53                6       8446909             FALSE            4
    ## 54                6       8447007             FALSE            1
    ## 55                6       8447021             FALSE            0
    ## 56                6       8447036             FALSE            0
    ## 57                6       8447054             FALSE            0
    ## 58                6       8447116             FALSE            0
    ##    data.gameTypeId data.goals data.mostAssistsOneGame
    ## 1                2          0                       0
    ## 2                2          0                       1
    ## 3                2          0                       1
    ## 4                2          0                       0
    ## 5                2          0                       1
    ## 6                2          0                       0
    ## 7                2          0                       1
    ## 8                2          0                       0
    ## 9                2          0                       0
    ## 10               2          0                       2
    ## 11               2          0                       0
    ## 12               2          0                       0
    ## 13               2          0                       0
    ## 14               2          0                       1
    ## 15               2          0                       1
    ## 16               2          0                       0
    ## 17               2          0                       0
    ## 18               2          0                       0
    ## 19               2          0                       0
    ## 20               2          0                       0
    ## 21               2          0                       0
    ## 22               2          0                       0
    ## 23               2          0                       2
    ## 24               2          0                       0
    ## 25               2          0                       0
    ## 26               2          0                       0
    ## 27               2          0                       1
    ## 28               2          0                       0
    ## 29               2          0                       0
    ## 30               2          0                       0
    ## 31               2          0                       1
    ## 32               2          0                       0
    ## 33               2          0                       1
    ## 34               2          0                       1
    ## 35               2          0                       0
    ## 36               2          0                       0
    ## 37               2          0                       0
    ## 38               2          0                       1
    ## 39               2          0                       0
    ## 40               2          0                       0
    ## 41               2          0                       1
    ## 42               2          0                       0
    ## 43               2          0                       1
    ## 44               2          0                       1
    ## 45               2          0                       0
    ## 46               2          0                       1
    ## 47               2          0                       0
    ## 48               2          0                       0
    ## 49               2          0                       0
    ## 50               2          0                       0
    ## 51               2          0                       0
    ## 52               2          0                       0
    ## 53               2          0                       1
    ## 54               2          0                       1
    ## 55               2          0                       0
    ## 56               2          0                       0
    ## 57               2          0                       0
    ## 58               2          0                       0
    ##    data.mostAssistsOneSeason data.gamesPlayed data.mostGoalsOneGame
    ## 1                          0                1                     0
    ## 2                          1                6                     0
    ## 3                          3               14                     0
    ## 4                          0                1                     0
    ## 5                          2               15                     0
    ## 6                          0                7                     0
    ## 7                          2                8                     0
    ## 8                          0                2                     0
    ## 9                          0                8                     0
    ## 10                         5                6                     0
    ## 11                         0                7                     0
    ## 12                         0                1                     0
    ## 13                         0                3                     0
    ## 14                         1                1                     0
    ## 15                         1               32                     0
    ## 16                         0               10                     0
    ## 17                         0                8                     0
    ## 18                         0                6                     0
    ## 19                         0                3                     0
    ## 20                         0               10                     0
    ## 21                         0                2                     0
    ## 22                         0                4                     0
    ## 23                         2               13                     0
    ## 24                         0                1                     0
    ## 25                         0               12                     0
    ## 26                         0                3                     0
    ## 27                         1               41                     0
    ## 28                         0                1                     0
    ## 29                         0                1                     0
    ## 30                         0                3                     0
    ## 31                         3               28                     0
    ## 32                         0                2                     0
    ## 33                         1                8                     0
    ## 34                         4               18                     0
    ## 35                         0                1                     0
    ## 36                         0                5                     0
    ## 37                         0                3                     0
    ## 38                         1               10                     0
    ## 39                         0                1                     0
    ## 40                         0                2                     0
    ## 41                         2                9                     0
    ## 42                         0                2                     0
    ## 43                         2               11                     0
    ## 44                         1                9                     0
    ## 45                         0               22                     0
    ## 46                         2               33                     0
    ## 47                         0                1                     0
    ## 48                         0                6                     0
    ## 49                         0                3                     0
    ## 50                         0                6                     0
    ## 51                         0                4                     0
    ## 52                         0                4                     0
    ## 53                         4               32                     0
    ## 54                         1                1                     0
    ## 55                         0                1                     0
    ## 56                         0                2                     0
    ## 57                         0                5                     0
    ## 58                         0                8                     0
    ##    data.mostPenaltyMinutesOneSeason data.mostPointsOneGame
    ## 1                                 0                      0
    ## 2                                11                      1
    ## 3                                14                      1
    ## 4                                 0                      0
    ## 5                                26                      1
    ## 6                                 2                      0
    ## 7                                 4                      1
    ## 8                                 0                      0
    ## 9                                 0                      0
    ## 10                                4                      2
    ## 11                                0                      0
    ## 12                                0                      0
    ## 13                                0                      0
    ## 14                                0                      1
    ## 15                                9                      1
    ## 16                                7                      0
    ## 17                                0                      0
    ## 18                                4                      0
    ## 19                                0                      0
    ## 20                                4                      0
    ## 21                                0                      0
    ## 22                                2                      0
    ## 23                                6                      2
    ## 24                                0                      0
    ## 25                                0                      0
    ## 26                                0                      0
    ## 27                               17                      1
    ## 28                                0                      0
    ## 29                                0                      0
    ## 30                                0                      0
    ## 31                               24                      1
    ## 32                                0                      0
    ## 33                                0                      1
    ## 34                               30                      1
    ## 35                                0                      0
    ## 36                                0                      0
    ## 37                                0                      0
    ## 38                               22                      1
    ## 39                                0                      0
    ## 40                                0                      0
    ## 41                                8                      1
    ## 42                                0                      0
    ## 43                                8                      1
    ## 44                                4                      1
    ## 45                               14                      0
    ## 46                                2                      1
    ## 47                                0                      0
    ## 48                                2                      0
    ## 49                               12                      0
    ## 50                               10                      0
    ## 51                                0                      0
    ## 52                                0                      0
    ## 53                               15                      1
    ## 54                                0                      1
    ## 55                                0                      0
    ## 56                                0                      0
    ## 57                                0                      0
    ## 58                                2                      0
    ##    data.mostPointsOneSeason data.penaltyMinutes data.points
    ## 1                         0                   0           0
    ## 2                         1                  11           1
    ## 3                         3                  14           3
    ## 4                         0                   0           0
    ## 5                         2                  26           2
    ## 6                         0                   2           0
    ## 7                         2                   4           2
    ## 8                         0                   0           0
    ## 9                         0                   0           0
    ## 10                        5                   4           5
    ## 11                        0                   0           0
    ## 12                        0                   0           0
    ## 13                        0                   0           0
    ## 14                        1                   0           1
    ## 15                        1                   9           1
    ## 16                        0                   7           0
    ## 17                        0                   0           0
    ## 18                        0                   4           0
    ## 19                        0                   0           0
    ## 20                        0                   4           0
    ## 21                        0                   0           0
    ## 22                        0                   2           0
    ## 23                        2                   6           2
    ## 24                        0                   0           0
    ## 25                        0                   0           0
    ## 26                        0                   0           0
    ## 27                        1                  25           1
    ## 28                        0                   0           0
    ## 29                        0                   0           0
    ## 30                        0                   0           0
    ## 31                        3                  60           4
    ## 32                        0                   0           0
    ## 33                        1                   0           1
    ## 34                        4                  30           4
    ## 35                        0                   0           0
    ## 36                        0                   0           0
    ## 37                        0                   0           0
    ## 38                        1                  22           1
    ## 39                        0                   0           0
    ## 40                        0                   0           0
    ## 41                        2                   8           2
    ## 42                        0                   0           0
    ## 43                        2                   8           2
    ## 44                        1                   4           1
    ## 45                        0                  14           0
    ## 46                        2                   2           2
    ## 47                        0                   0           0
    ## 48                        0                   2           0
    ## 49                        0                  12           0
    ## 50                        0                  10           0
    ## 51                        0                   0           0
    ## 52                        0                   0           0
    ## 53                        4                  15           4
    ## 54                        1                   0           1
    ## 55                        0                   0           0
    ## 56                        0                   0           0
    ## 57                        0                   0           0
    ## 58                        0                   2           0
    ##    data.positionCode data.seasons
    ## 1                  C            1
    ## 2                  D            2
    ## 3                  D            1
    ## 4                  R            1
    ## 5                  R            1
    ## 6                  C            1
    ## 7                  D            1
    ## 8                  R            1
    ## 9                  R            1
    ## 10                 L            1
    ## 11                 L            1
    ## 12                 C            1
    ## 13                 C            1
    ## 14                 D            1
    ## 15                 R            2
    ## 16                 D            1
    ## 17                 C            1
    ## 18                 D            1
    ## 19                 C            1
    ## 20                 C            2
    ## 21                 D            1
    ## 22                 C            3
    ## 23                 C            2
    ## 24                 C            1
    ## 25                 L            1
    ## 26                 C            1
    ## 27                 D            3
    ## 28                 D            1
    ## 29                 R            1
    ## 30                 D            1
    ## 31                 D            3
    ## 32                 D            1
    ## 33                 R            2
    ## 34                 D            1
    ## 35                 L            1
    ## 36                 R            2
    ## 37                 L            1
    ## 38                 L            1
    ## 39                 D            1
    ## 40                 R            1
    ## 41                 R            1
    ## 42                 L            1
    ## 43                 D            1
    ## 44                 C            2
    ## 45                 L            1
    ## 46                 L            1
    ## 47                 L            1
    ## 48                 L            1
    ## 49                 R            1
    ## 50                 D            1
    ## 51                 C            1
    ## 52                 R            1
    ## 53                 D            1
    ## 54                 D            1
    ## 55                 L            1
    ## 56                 L            1
    ## 57                 R            1
    ## 58                 D            2
    ##  [ reached 'max' / getOption("max.print") -- omitted 860 rows ]

``` r
gp_sk_comb <- inner_join(goalie_data, skater_data,copy=TRUE, by=("data.franchiseId"))
gp_sk_comb
```

    ##    data.franchiseId data.playerId.x data.activePlayer.x data.gameTypeId.x
    ## 1                 6         8445403               FALSE                 2
    ## 2                 6         8445403               FALSE                 2
    ## 3                 6         8445403               FALSE                 2
    ## 4                 6         8445403               FALSE                 2
    ## 5                 6         8445403               FALSE                 2
    ## 6                 6         8445403               FALSE                 2
    ## 7                 6         8445403               FALSE                 2
    ## 8                 6         8445403               FALSE                 2
    ## 9                 6         8445403               FALSE                 2
    ## 10                6         8445403               FALSE                 2
    ## 11                6         8445403               FALSE                 2
    ## 12                6         8445403               FALSE                 2
    ## 13                6         8445403               FALSE                 2
    ## 14                6         8445403               FALSE                 2
    ## 15                6         8445403               FALSE                 2
    ## 16                6         8445403               FALSE                 2
    ## 17                6         8445403               FALSE                 2
    ## 18                6         8445403               FALSE                 2
    ## 19                6         8445403               FALSE                 2
    ## 20                6         8445403               FALSE                 2
    ## 21                6         8445403               FALSE                 2
    ## 22                6         8445403               FALSE                 2
    ## 23                6         8445403               FALSE                 2
    ## 24                6         8445403               FALSE                 2
    ## 25                6         8445403               FALSE                 2
    ## 26                6         8445403               FALSE                 2
    ## 27                6         8445403               FALSE                 2
    ## 28                6         8445403               FALSE                 2
    ## 29                6         8445403               FALSE                 2
    ## 30                6         8445403               FALSE                 2
    ##    data.gamesPlayed.x data.losses data.mostGoalsAgainstOneGame
    ## 1                   8           0                            5
    ## 2                   8           0                            5
    ## 3                   8           0                            5
    ## 4                   8           0                            5
    ## 5                   8           0                            5
    ## 6                   8           0                            5
    ## 7                   8           0                            5
    ## 8                   8           0                            5
    ## 9                   8           0                            5
    ## 10                  8           0                            5
    ## 11                  8           0                            5
    ## 12                  8           0                            5
    ## 13                  8           0                            5
    ## 14                  8           0                            5
    ## 15                  8           0                            5
    ## 16                  8           0                            5
    ## 17                  8           0                            5
    ## 18                  8           0                            5
    ## 19                  8           0                            5
    ## 20                  8           0                            5
    ## 21                  8           0                            5
    ## 22                  8           0                            5
    ## 23                  8           0                            5
    ## 24                  8           0                            5
    ## 25                  8           0                            5
    ## 26                  8           0                            5
    ## 27                  8           0                            5
    ## 28                  8           0                            5
    ## 29                  8           0                            5
    ## 30                  8           0                            5
    ##    data.seasons.x data.mostSavesOneGame data.mostShotsAgainstOneGame
    ## 1               1                    24                           27
    ## 2               1                    24                           27
    ## 3               1                    24                           27
    ## 4               1                    24                           27
    ## 5               1                    24                           27
    ## 6               1                    24                           27
    ## 7               1                    24                           27
    ## 8               1                    24                           27
    ## 9               1                    24                           27
    ## 10              1                    24                           27
    ## 11              1                    24                           27
    ## 12              1                    24                           27
    ## 13              1                    24                           27
    ## 14              1                    24                           27
    ## 15              1                    24                           27
    ## 16              1                    24                           27
    ## 17              1                    24                           27
    ## 18              1                    24                           27
    ## 19              1                    24                           27
    ## 20              1                    24                           27
    ## 21              1                    24                           27
    ## 22              1                    24                           27
    ## 23              1                    24                           27
    ## 24              1                    24                           27
    ## 25              1                    24                           27
    ## 26              1                    24                           27
    ## 27              1                    24                           27
    ## 28              1                    24                           27
    ## 29              1                    24                           27
    ## 30              1                    24                           27
    ##    data.mostShutoutsOneSeason data.mostWinsOneSeason data.overtimeLosses
    ## 1                           0                      2                  NA
    ## 2                           0                      2                  NA
    ## 3                           0                      2                  NA
    ## 4                           0                      2                  NA
    ## 5                           0                      2                  NA
    ## 6                           0                      2                  NA
    ## 7                           0                      2                  NA
    ## 8                           0                      2                  NA
    ## 9                           0                      2                  NA
    ## 10                          0                      2                  NA
    ## 11                          0                      2                  NA
    ## 12                          0                      2                  NA
    ## 13                          0                      2                  NA
    ## 14                          0                      2                  NA
    ## 15                          0                      2                  NA
    ## 16                          0                      2                  NA
    ## 17                          0                      2                  NA
    ## 18                          0                      2                  NA
    ## 19                          0                      2                  NA
    ## 20                          0                      2                  NA
    ## 21                          0                      2                  NA
    ## 22                          0                      2                  NA
    ## 23                          0                      2                  NA
    ## 24                          0                      2                  NA
    ## 25                          0                      2                  NA
    ## 26                          0                      2                  NA
    ## 27                          0                      2                  NA
    ## 28                          0                      2                  NA
    ## 29                          0                      2                  NA
    ## 30                          0                      2                  NA
    ##    data.positionCode.x data.shutouts data.ties data.wins data.playerId.y
    ## 1                    G             0         3         2         8444899
    ## 2                    G             0         3         2         8444966
    ## 3                    G             0         3         2         8444980
    ## 4                    G             0         3         2         8444984
    ## 5                    G             0         3         2         8445005
    ## 6                    G             0         3         2         8445009
    ## 7                    G             0         3         2         8445061
    ## 8                    G             0         3         2         8445068
    ## 9                    G             0         3         2         8445074
    ## 10                   G             0         3         2         8445084
    ## 11                   G             0         3         2         8445152
    ## 12                   G             0         3         2         8445153
    ## 13                   G             0         3         2         8445239
    ## 14                   G             0         3         2         8445301
    ## 15                   G             0         3         2         8445306
    ## 16                   G             0         3         2         8445340
    ## 17                   G             0         3         2         8445348
    ## 18                   G             0         3         2         8445366
    ## 19                   G             0         3         2         8445377
    ## 20                   G             0         3         2         8445425
    ## 21                   G             0         3         2         8445488
    ## 22                   G             0         3         2         8445573
    ## 23                   G             0         3         2         8445591
    ## 24                   G             0         3         2         8445743
    ## 25                   G             0         3         2         8445747
    ## 26                   G             0         3         2         8445816
    ## 27                   G             0         3         2         8445848
    ## 28                   G             0         3         2         8445852
    ## 29                   G             0         3         2         8445863
    ## 30                   G             0         3         2         8445867
    ##    data.activePlayer.y data.assists data.gameTypeId.y data.goals
    ## 1                FALSE            0                 2          0
    ## 2                FALSE            1                 2          0
    ## 3                FALSE            3                 2          0
    ## 4                FALSE            0                 2          0
    ## 5                FALSE            2                 2          0
    ## 6                FALSE            0                 2          0
    ## 7                FALSE            2                 2          0
    ## 8                FALSE            0                 2          0
    ## 9                FALSE            0                 2          0
    ## 10               FALSE            5                 2          0
    ## 11               FALSE            0                 2          0
    ## 12               FALSE            0                 2          0
    ## 13               FALSE            0                 2          0
    ## 14               FALSE            1                 2          0
    ## 15               FALSE            1                 2          0
    ## 16               FALSE            0                 2          0
    ## 17               FALSE            0                 2          0
    ## 18               FALSE            0                 2          0
    ## 19               FALSE            0                 2          0
    ## 20               FALSE            0                 2          0
    ## 21               FALSE            0                 2          0
    ## 22               FALSE            0                 2          0
    ## 23               FALSE            2                 2          0
    ## 24               FALSE            0                 2          0
    ## 25               FALSE            0                 2          0
    ## 26               FALSE            0                 2          0
    ## 27               FALSE            1                 2          0
    ## 28               FALSE            0                 2          0
    ## 29               FALSE            0                 2          0
    ## 30               FALSE            0                 2          0
    ##    data.mostAssistsOneGame data.mostAssistsOneSeason data.gamesPlayed.y
    ## 1                        0                         0                  1
    ## 2                        1                         1                  6
    ## 3                        1                         3                 14
    ## 4                        0                         0                  1
    ## 5                        1                         2                 15
    ## 6                        0                         0                  7
    ## 7                        1                         2                  8
    ## 8                        0                         0                  2
    ## 9                        0                         0                  8
    ## 10                       2                         5                  6
    ## 11                       0                         0                  7
    ## 12                       0                         0                  1
    ## 13                       0                         0                  3
    ## 14                       1                         1                  1
    ## 15                       1                         1                 32
    ## 16                       0                         0                 10
    ## 17                       0                         0                  8
    ## 18                       0                         0                  6
    ## 19                       0                         0                  3
    ## 20                       0                         0                 10
    ## 21                       0                         0                  2
    ## 22                       0                         0                  4
    ## 23                       2                         2                 13
    ## 24                       0                         0                  1
    ## 25                       0                         0                 12
    ## 26                       0                         0                  3
    ## 27                       1                         1                 41
    ## 28                       0                         0                  1
    ## 29                       0                         0                  1
    ## 30                       0                         0                  3
    ##    data.mostGoalsOneGame data.mostPenaltyMinutesOneSeason
    ## 1                      0                                0
    ## 2                      0                               11
    ## 3                      0                               14
    ## 4                      0                                0
    ## 5                      0                               26
    ## 6                      0                                2
    ## 7                      0                                4
    ## 8                      0                                0
    ## 9                      0                                0
    ## 10                     0                                4
    ## 11                     0                                0
    ## 12                     0                                0
    ## 13                     0                                0
    ## 14                     0                                0
    ## 15                     0                                9
    ## 16                     0                                7
    ## 17                     0                                0
    ## 18                     0                                4
    ## 19                     0                                0
    ## 20                     0                                4
    ## 21                     0                                0
    ## 22                     0                                2
    ## 23                     0                                6
    ## 24                     0                                0
    ## 25                     0                                0
    ## 26                     0                                0
    ## 27                     0                               17
    ## 28                     0                                0
    ## 29                     0                                0
    ## 30                     0                                0
    ##    data.mostPointsOneGame data.mostPointsOneSeason data.penaltyMinutes
    ## 1                       0                        0                   0
    ## 2                       1                        1                  11
    ## 3                       1                        3                  14
    ## 4                       0                        0                   0
    ## 5                       1                        2                  26
    ## 6                       0                        0                   2
    ## 7                       1                        2                   4
    ## 8                       0                        0                   0
    ## 9                       0                        0                   0
    ## 10                      2                        5                   4
    ## 11                      0                        0                   0
    ## 12                      0                        0                   0
    ## 13                      0                        0                   0
    ## 14                      1                        1                   0
    ## 15                      1                        1                   9
    ## 16                      0                        0                   7
    ## 17                      0                        0                   0
    ## 18                      0                        0                   4
    ## 19                      0                        0                   0
    ## 20                      0                        0                   4
    ## 21                      0                        0                   0
    ## 22                      0                        0                   2
    ## 23                      2                        2                   6
    ## 24                      0                        0                   0
    ## 25                      0                        0                   0
    ## 26                      0                        0                   0
    ## 27                      1                        1                  25
    ## 28                      0                        0                   0
    ## 29                      0                        0                   0
    ## 30                      0                        0                   0
    ##    data.points data.positionCode.y data.seasons.y
    ## 1            0                   C              1
    ## 2            1                   D              2
    ## 3            3                   D              1
    ## 4            0                   R              1
    ## 5            2                   R              1
    ## 6            0                   C              1
    ## 7            2                   D              1
    ## 8            0                   R              1
    ## 9            0                   R              1
    ## 10           5                   L              1
    ## 11           0                   L              1
    ## 12           0                   C              1
    ## 13           0                   C              1
    ## 14           1                   D              1
    ## 15           1                   R              2
    ## 16           0                   D              1
    ## 17           0                   C              1
    ## 18           0                   D              1
    ## 19           0                   C              1
    ## 20           0                   C              2
    ## 21           0                   D              1
    ## 22           0                   C              3
    ## 23           2                   C              2
    ## 24           0                   C              1
    ## 25           0                   L              1
    ## 26           0                   C              1
    ## 27           1                   D              3
    ## 28           0                   D              1
    ## 29           0                   R              1
    ## 30           0                   D              1
    ##  [ reached 'max' / getOption("max.print") -- omitted 46788 rows ]

### Data from two endpoints

### New variables

### contingency tables

### Five plots

#### bar plot

#### histogram

#### box plot

#### scatter plot
