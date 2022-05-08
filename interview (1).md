    This juptyer notebook depicts a query that I wrote to practice multipl joins. This problem was a more advanced join than I have done in the past and I wanted to make this a part of my portfolio becasue I learned some specific skills that I would not of learned otherwise. SOURCE: This problem was complete from HackerRank.com.  
    
    PROBLEM STATEMENT: Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are 0.

    TABLES: 
        1. contests (contest_id integer, hacker_id integer, name text)
        2. colleges (college_id integer, contest_id integer)
        3. challenges (challenge_id integer, college_id integer
        4. view_stats (challenge_id integer, total_views integer, total_unique_views integer)
        5. submission_stats (challenge_id integer, total_submissions, total_accepted_submissions)
    

    WHAT I LEARNED: 
        I had some diffifulty with getting the correct output after joining the tables together. In the first submission that I had the join statements looked like this: 
        


```python
INNER JOIN colleges on contests.contest_id = colleges.contest_id
INNER JOIN challenges on colleges.college_id = challenges.college_id
LEFT JOIN view_stats ON challenges.challenge_id = view_stats.challenge_id
LEFT JOIN submission_ stats ON challenges.challenge_id = submission_stats.challenge_id
```

    And the output from the query with these join statements showed this: 


```python
import sqlite3
import pandas as pd 

con = sqlite3.connect("interview.db3")

ff = pd.read_sql_query("""SELECT contests.contest_id AS contest_id, 
        contests.hacker_id AS hacker_id, 
        contests.name as name,
        SUM(submission_stats.total_submissions) AS total_subs, 
        SUM(submission_stats.total_accepted_submissions) AS total_accepted_subs,
        SUM(view_stats.total_views) AS total_views,
        SUM(view_stats.total_unique_views) AS total_unique_views
FROM contests
INNER JOIN colleges on contests.contest_id = colleges.contest_id
INNER JOIN challenges on colleges.college_id = challenges.college_id
LEFT JOIN view_stats ON challenges.challenge_id = view_stats.challenge_id
LEFT JOIN submission_stats ON challenges.challenge_id = submission_stats.challenge_id
    
GROUP BY contests.contest_id, contests.hacker_id, contests.name
HAVING  SUM(submission_stats.total_submissions) != 0 AND
        SUM(submission_stats.total_accepted_submissions)!= 0 AND
        SUM(view_stats.total_views)!= 0 AND
        SUM(view_stats.total_unique_views) != 0
ORDER BY contests.contest_id""", con)

print(ff)

```

       contest_id hacker_id       name  total_subs  total_accepted_subs  \
    0       10568      8802      Kelly        5363                 1826   
    1       11100      8809      Robin        3701                 1216   
    2       12742      9203      Ralph        3443                  866   
    3       12861      9644     Gloria        2947                  964   
    4       12865     10108     Victor        3364                  966   
    5       13503     10803      David        1109                  314   
    6       13537     11390      Joyce        3079                 1133   
    7       13612     12592      Donna        3698                 1045   
    8       14502     12923   Michelle        3673                 1148   
    9       14867     13017  Stephanie        4609                 1351   
    10      15164     13256     Gerald        4772                 1603   
    11      15804     13421     Walter        3317                 1046   
    12      15891     13569  Christina        4779                 1570   
    13      16063     14287    Brandon        4839                 1535   
    14      16415     14311  Elizabeth        9793                 2856   
    15       1793      2655    Patrick        2666                  742   
    16      18477     14440     Joseph        2213                  728   
    17      18855     16973   Lawrence        7693                 2397   
    18      19097     17123    Marilyn        7020                 2134   
    19      19575     17562       Lori        5222                 1625   
    20       2374      2765       Lisa        6373                 1947   
    21       2963      2845   Kimberly        9471                 2630   
    22       3584      2873     Bonnie        6841                 1720   
    23       4044      3067    Michael        2859                 1059   
    24       4249      3116       Todd        2418                  616   
    25       4269      3256        Joe        3115                 1042   
    26       4483      3386       Earl        3988                 1026   
    27       4541      3608     Robert        3670                 1010   
    28       4601      3868        Amy        3991                 1427   
    29       4710      4255     Pamela        4402                 1039   
    30       4982      5639      Maria        5188                 1394   
    31       5913      5669        Joe        4518                 1415   
    32       5994      5713      Linda        6928                 1928   
    33       6939      6550    Melissa        6490                 1917   
    34       7266      6947      Carol        5969                 1452   
    35       7280      7030      Paula        3754                 1135   
    36       7484      7033    Marilyn        7588                 2216   
    37       7734      7386   Jennifer        8214                 2207   
    38       7831      7787      Harry        7223                 1805   
    39       7862      8029      David        2748                  813   
    40        858      1053     Angela        1953                  448   
    41       8812      8147      Julia        1916                  494   
    42       8825      8438      Kevin        5541                 1671   
    43        883      1055      Frank        2689                  734   
    44       9136      8727       Paul        8279                 2886   
    45       9613      8762      James        8137                 2217   
    
        total_views  total_unique_views  
    0          6165                1886  
    1          3847                1271  
    2          3399                 972  
    3          2981                 977  
    4          2365                 852  
    5           963                 259  
    6          3592                1115  
    7          3501                1016  
    8          3158                1066  
    9          5189                1187  
    10         4942                1487  
    11         3546                1080  
    12         4180                1531  
    13         5161                1674  
    14         9736                2924  
    15         2269                 769  
    16         2352                 777  
    17         7966                2527  
    18         6420                1890  
    19         5692                1695  
    20         7933                2154  
    21         7528                2516  
    22         6424                2051  
    23         3188                 970  
    24         2146                 600  
    25         3363                1004  
    26         3355                 911  
    27         3532                 975  
    28         4500                1367  
    29         3979                1140  
    30         4877                1359  
    31         5827                1510  
    32         6151                1879  
    33         7375                2082  
    34         5449                1647  
    35         2636                 858  
    36         8142                2210  
    37         8853                2529  
    38         6007                2074  
    39         2864                 903  
    40         1740                 636  
    41         1865                 572  
    42         5495                1627  
    43         2153                 667  
    44         8131                2459  
    45         8478                2539  


Now, the output looks like it should be correct but I was not getting the appropriate output. I double and triple checked my join statements for accuaracy and I started thinking about how I have never compiled aggregations at the same time I was making multiple joins to this extent. I found some helpful information on stack overflow that stated that I need to "pre-aggregate" the data before joining the data to the new table for a further aggregation. If I do not pre-aggregate, essentially  the data is being "counted" twice and the values will be higher than they should be. 

For the correct query below, I utilized subquires within my join statements to "pre-aggregate" and this led to the correct output that you see below. 


```python
SELECT contests.contest_id, 
        contests.hacker_id, 
        contests.name,
        SUM(submission_stats.total_submissions), 
        SUM(submission_stats.total_accepted_submissions),
        SUM(view_stats.total_views),
        SUM(view_stats.total_unique_views)
FROM contests

INNER JOIN colleges on contests.contest_id = colleges.contest_id
INNER JOIN challenges on colleges.college_id = challenges.college_id
LEFT JOIN (SELECT challenge_id, 
                    SUM(total_views) AS total_views, 
                    SUM(total_unique_views) AS total_unique_views 
            FROM view_stats 
            GROUP BY challenge_id) AS view_stats
    ON challenges.challenge_id = view_stats.challenge_id
LEFT JOIN (SELECT challenge_id, 
            SUM(total_submissions) AS total_submissions, 
            SUM(total_accepted_submissions) AS total_accepted_submissions 
            FROM Submission_Stats 
            GROUP BY challenge_id) AS submission_stats 
    ON challenges.challenge_id = submission_stats.challenge_id


GROUP BY contests.contest_id, contests.hacker_id, contests.name
HAVING  SUM(submission_stats.total_submissions) != 0 AND
        SUM(submission_stats.total_accepted_submissions)!= 0 AND
        SUM(view_stats.total_views)!= 0 AND
        SUM(view_stats.total_unique_views) != 0
ORDER BY contests.contest_id
```


```python
import sqlite3
import pandas as pd 

con = sqlite3.connect("interview.db3")

df = pd.read_sql_query("""SELECT contests.contest_id AS contest_id, 
        contests.hacker_id AS hacker_id, 
        contests.name as name,
        SUM(submission_stats.total_submissions) AS total_subs, 
        SUM(submission_stats.total_accepted_submissions) AS total_accepted_subs,
        SUM(view_stats.total_views) AS total_views,
        SUM(view_stats.total_unique_views) AS total_unique_views
FROM contests
INNER JOIN colleges on contests.contest_id = colleges.contest_id
INNER JOIN challenges on colleges.college_id = challenges.college_id
LEFT JOIN (SELECT challenge_id, 
                    SUM(total_views) AS total_views, 
                    SUM(total_unique_views) AS total_unique_views 
            FROM view_stats 
            GROUP BY challenge_id) AS view_stats
    ON challenges.challenge_id = view_stats.challenge_id
LEFT JOIN (SELECT challenge_id, 
            SUM(total_submissions) AS total_submissions, 
            SUM(total_accepted_submissions) AS total_accepted_submissions 
            FROM Submission_Stats 
            GROUP BY challenge_id) AS submission_stats 
    ON challenges.challenge_id = submission_stats.challenge_id
GROUP BY contests.contest_id, contests.hacker_id, contests.name
HAVING  SUM(submission_stats.total_submissions) != 0 AND
        SUM(submission_stats.total_accepted_submissions)!= 0 AND
        SUM(view_stats.total_views)!= 0 AND
        SUM(view_stats.total_unique_views) != 0
ORDER BY contests.contest_id""", con)

print(df)


```

       contest_id hacker_id       name  total_subs  total_accepted_subs  \
    0       10568      8802      Kelly        1907                  620   
    1       11100      8809      Robin        1929                  613   
    2       12742      9203      Ralph        1523                  413   
    3       12861      9644     Gloria        1596                  536   
    4       12865     10108     Victor        2076                  597   
    5       13503     10803      David         924                  251   
    6       13537     11390      Joyce        1381                  497   
    7       13612     12592      Donna        1981                  550   
    8       14502     12923   Michelle        1510                  463   
    9       14867     13017  Stephanie        2471                  676   
    10      15164     13256     Gerald        2570                  820   
    11      15804     13421     Walter        1454                  459   
    12      15891     13569  Christina        2188                  710   
    13      16063     14287    Brandon        1804                  580   
    14      16415     14311  Elizabeth        4535                 1366   
    15       1793      2655    Patrick        1337                  360   
    16      18477     14440     Joseph        1320                  391   
    17      18855     16973   Lawrence        2967                 1020   
    18      19097     17123    Marilyn        2956                  807   
    19      19575     17562       Lori        2590                  863   
    20       2374      2765       Lisa        2733                  815   
    21       2963      2845   Kimberly        4306                 1221   
    22       3584      2873     Bonnie        2492                  652   
    23       4044      3067    Michael        1323                  449   
    24       4249      3116       Todd        1452                  376   
    25       4269      3256        Joe        1018                  372   
    26       4483      3386       Earl        1911                  572   
    27       4541      3608     Robert        1886                  516   
    28       4601      3868        Amy        1900                  639   
    29       4710      4255     Pamela        2752                  639   
    30       4982      5639      Maria        2705                  759   
    31       5913      5669        Joe        2646                  790   
    32       5994      5713      Linda        3369                  967   
    33       6939      6550    Melissa        2842                  859   
    34       7266      6947      Carol        2758                  665   
    35       7280      7030      Paula        1963                  554   
    36       7484      7033    Marilyn        3217                  934   
    37       7734      7386   Jennifer        3780                 1015   
    38       7831      7787      Harry        3190                  883   
    39       7862      8029      David        1738                  476   
    40        858      1053     Angela         703                  160   
    41       8812      8147      Julia        1044                  302   
    42       8825      8438      Kevin        2624                  772   
    43        883      1055      Frank        1121                  319   
    44       9136      8727       Paul        4205                 1359   
    45       9613      8762      James        3438                  943   
    
        total_views  total_unique_views  
    0          2577                 798  
    1          1883                 619  
    2          1344                 383  
    3          2089                 623  
    4          1259                 418  
    5           584                 167  
    6          1784                 538  
    7          1487                 465  
    8          1830                 545  
    9          2291                 574  
    10         2085                 607  
    11         1396                 476  
    12         2266                 786  
    13         1621                 521  
    14         3631                1071  
    15         1216                 412  
    16         1419                 428  
    17         3371                1011  
    18         2554                 750  
    19         2627                 760  
    20         3368                 904  
    21         3603                1184  
    22         3019                 954  
    23         1722                 528  
    24         1767                 463  
    25         1766                 530  
    26         1644                 477  
    27         1694                 504  
    28         1738                 548  
    29         2378                 705  
    30         2558                 711  
    31         3181                 835  
    32         3048                 954  
    33         3574                1004  
    34         3044                 835  
    35          886                 259  
    36         3795                1061  
    37         3637                1099  
    38         2933                1012  
    39         1475                 472  
    40         1002                 384  
    41          819                 266  
    42         2187                 689  
    43         1217                 338  
    44         3125                 954  
    45         3620                1046  

