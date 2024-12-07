## Introduction and Question Identification

#### **Introduction**

League of Legends, a dynamic and evolving game, has seen its win conditions and strategies shift drastically over the years. Objectives such as dragons, Rift Heralds, and the newly introduced void grubs each contribute uniquely to a team’s path to victory. By 2024, the professional meta began emphasizing tempo-driven strategies, prioritizing topside objectives like Heralds and void grubs over the traditionally valued dragon stacking.

This shift reflects a deeper strategic debate: **tempo vs. value**. 
- **Tempo Objectives**: Securing void grubs and Heralds generates a permanent tempo advantage, enabling teams to destroy towers faster, dictate the pace of the game, and pressure opponents into unfavorable positions.
- **Value Objectives**: Stacking dragons provides long-term team-wide buffs that scale into the late game but may require a longer duration to yield tangible advantages.

This dataset contains professional League of Legends match data collected from Oracle’s Elixir, with detailed information about team and player performance, objective control, and game outcomes. Each row represents either a player or a team-level summary of a single game. The data allows us to explore relationships between key objectives and winning strategies in professional matches.

As a member of a competitive League of Legends team, I’ve often encountered debates about optimal strategies for securing victories. Our head coach has developed many unconventional theories, one of which challenges the traditional emphasis on dragon stacking as the primary path to late-game success. Instead, he strongly believes that prioritizing void grub collection, Rift Herald control, and using them to destroy towers is a superior strategy. While this theory may sound counterintuitive to most players who were taught that dragon stacking is essential for late-game dominance, it opens the door for interesting analysis.

The goal of this project is to investigate whether prioritizing objectives like void grubs and Heralds over dragon stacking correlates more strongly with winning games. The findings could have significant implications for competitive strategies, especially for teams looking to gain an edge with unconventional tactics.

---

#### **Question**

This analysis seeks to answer the following question:  
**"In the 2024 professional meta, does prioritizing tempo-based objectives (void grub collection and Rift Herald control) over value-based objectives (dragon stacking and Elder fights) correlate more strongly with game victories?"**

---

#### **Dataset Overview**

- **Number of Rows**: 16,606 (team-level summaries of complete matches).  
- **Relevant Columns**:  
  - **`dragons`**: Total number of dragons secured by a team.  
  - **`firstdragon`**: Whether the team secured the first dragon.  
  - **`heralds`**: Total number of Rift Heralds secured by a team.  
  - **`firstherald`**: Whether the team secured the first Herald.  
  - **`void_grubs`**: Total number of void grubs collected.  
  - **`result`**: Whether the team won the game (1 = win, 0 = loss).  
  - **`towers`**: Total number of towers destroyed by a team.  
  - **`gamelength_minutes`**: Duration of the game in minutes.  

By focusing on these columns, we can explore the relationships between these objectives and game outcomes to evaluate the validity of our coach’s strategy.

## Data Cleaning

#### **Steps Taken**

1. **Filtered Complete Games**:  
   Rows where the column `datacompleteness` was labeled as `partial` were removed to ensure only fully recorded games were included in the analysis. This reduced the dataset size from **116,328 rows to 99,636 rows**.

2. **Filtered Team-Level Rows**:  
   Rows where the column `position` was labeled as `'team'` were retained, as they provide team-level summaries crucial for objective-based analysis. This further reduced the dataset size to **16,606 rows**.

3. **Dropped Columns with >50% Missing Data**:  
   Columns such as `monsterkillsownjungle`, `playername`, and `firstbloodkill` were removed due to excessive missing values. This step reduced the total columns from **163 to 150**.

4. **Imputed Missing Values**:  
   - **Numeric Columns**: Missing values in columns like `dragons`, `heralds`, and `towers` were imputed with their respective means to preserve the dataset's distribution.
   - **Categorical Columns**: Missing values in boolean columns like `firstdragon` and `firstherald` were imputed with their most frequent value (mode).

5. **Adjusted Derived Columns**:  
   - **`dragon_soul`**: Created a column indicating whether a team secured at least four dragons and had dragon soul.
   - **`stole_elder_without_soul`**: Added a flag for cases where a team with fewer than four dragons secured Elder Dragon, indicating potential comeback scenarios.

6. **Converted Boolean Columns**:  
   Columns such as `result`, `firstdragon`, `firstherald`, and `firsttower` were converted to boolean types for improved readability and easier analysis.

7. **Calculated Game Duration in Minutes**:  
   The column `gamelength` was standardized to `gamelength_minutes` by converting seconds into minutes for easier interpretability in the context of game strategy and outcomes.

---

#### **Impact on Analysis**

The data cleaning process ensured the dataset was free from missing values, irrelevant rows, and inconsistent columns, making it ready for exploratory and inferential analysis. By focusing on team-level summaries and imputing missing values where necessary, the integrity of the dataset was preserved, and potential biases were minimized. Additionally:
- The **derived columns** like `dragon_soul` and `stole_elder_without_soul` provide critical insights into objective-based strategies.
- Calculating game duration in minutes allows for more intuitive comparisons between objectives and game outcomes.

---

#### **Cleaned DataFrame (Head)**

| gameid           | datacompleteness   | league   |   year | split   |   playoffs | date                |   game |   patch |   participantid | side   | position   | teamname          | teamid                                  | ban1      | ban2     | ban3      | ban4       | ban5     | pick1   | pick2   | pick3        | pick4    | pick5    |   gamelength | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   team kpm |   ckpm | firstdragon   |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   elders |   opp_elders | firstherald   |   heralds |   opp_heralds |   void_grubs |   opp_void_grubs | firstbaron   |   barons |   opp_barons | firsttower   |   towers |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   goldspent |        gspd |   gpr |   minionkills |   monsterkills |    cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |   goldat20 |   xpat20 |   csat20 |   opp_goldat20 |   opp_xpat20 |   opp_csat20 |   golddiffat20 |   xpdiffat20 |   csdiffat20 |   killsat20 |   assistsat20 |   deathsat20 |   opp_killsat20 |   opp_assistsat20 |   opp_deathsat20 |   goldat25 |   xpat25 |   csat25 |   opp_goldat25 |   opp_xpat25 |   opp_csat25 |   golddiffat25 |   xpdiffat25 |   csdiffat25 |   killsat25 |   assistsat25 |   deathsat25 |   opp_killsat25 |   opp_assistsat25 |   opp_deathsat25 |   gamelength_minutes | dragon_soul   | stole_elder_without_soul   |
|:-----------------|:-------------------|:---------|-------:|:--------|-----------:|:--------------------|-------:|--------:|----------------:|:-------|:-----------|:------------------|:----------------------------------------|:----------|:---------|:----------|:-----------|:---------|:--------|:--------|:-------------|:---------|:---------|-------------:|:---------|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------:|-------:|:--------------|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|---------:|-------------:|:--------------|----------:|--------------:|-------------:|-----------------:|:-------------|---------:|-------------:|:-------------|---------:|-------------:|----------------:|---------------------:|---------------:|-------------------:|-------------:|-----------------:|--------------------:|--------:|-----------------------:|---------------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------:|------------:|------:|--------------:|---------------:|--------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|---------------------:|:--------------|:---------------------------|
| LOLTMNT99_132542 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |             100 | Blue   | team       | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | Orianna   | Rell     | Viego     | Viktor     | Malphite | LeBlanc | Varus   | Renata Glasc | Renekton | Xin Zhao |         1446 | True     |      20 |        7 |        47 |          20 |            7 |             3 |             1 |             0 |            0 |            0 |     0.8299 | 1.1203 | True          |         2 |             1 |                 2 |                     1 |           0 |           0 |        1 |        0 |           0 |          1 |        0 |            0 | True          |         1 |             0 |            0 |                6 | True         |        1 |            0 | True         |        9 |            1 |               1 |                    1 |              8 |                  5 |            1 |                0 |               61477 | 2550.91 |                3214.65 |                    2552.07 |            64 | 2.6556 |            39 | 1.6183 |                   30 |           186 | 7.7178 |       52523 |        36396 |     1510.21  |       42725 |  0.166033   |  2.7  |           700 |            131 | 34.4813 |      17004 |    18250 |      328 |          15640 |        17693 |          311 |           1364 |          557 |           17 |           4 |             6 |            2 |               2 |                 3 |                4 |      27034 |    30021 |      530 |          24741 |        29072 |          507 |           2293 |          949 |           23 |           6 |            10 |            3 |               3 |                 5 |                6 |      38246 |    42428 |      718 |          33998 |        40290 |          668 |           4248 |         2138 |           50 |          10 |            21 |            5 |               5 |                10 |               10 |      52523 |    58329 |      831 |          39782 |        47502 |          752 |          12741 |        10827 |           79 |          20 |            47 |            7 |               7 |                14 |               20 |              24.1    | False         | False                      |
| LOLTMNT99_132542 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |             200 | Red    | team       | Dark Passage      | oe:team:a42600a99997fca0d35f2821662b997 | Rumble    | Ashe     | Nocturne  | Poppy      | Vi       | Kalista | Neeko   | Lee Sin      | Nautilus | Jax      |         1446 | False    |       7 |       20 |        14 |           7 |           20 |             0 |             0 |             0 |            0 |            1 |     0.2905 | 1.1203 | False         |         1 |             2 |                 1 |                     2 |           1 |           0 |        0 |        0 |           0 |          0 |        0 |            0 | False         |         0 |             1 |            6 |                0 | False        |        0 |            1 | False        |        1 |            9 |               0 |                    0 |              5 |                  8 |            0 |                1 |               42737 | 1773.32 |                3182.32 |                    2857.97 |            70 | 2.9046 |            22 | 0.9129 |                   23 |           141 | 5.8506 |       39782 |        23655 |      981.535 |       36175 | -0.166033   | -2.7  |           612 |            140 | 31.2033 |      15640 |    17693 |      311 |          17004 |        18250 |          328 |          -1364 |         -557 |          -17 |           2 |             3 |            4 |               4 |                 6 |                2 |      24741 |    29072 |      507 |          27034 |        30021 |          530 |          -2293 |         -949 |          -23 |           3 |             5 |            6 |               6 |                10 |                3 |      33998 |    40290 |      668 |          38246 |        42428 |          718 |          -4248 |        -2138 |          -50 |           5 |            10 |           10 |              10 |                21 |                5 |      39782 |    47502 |      752 |          52523 |        58329 |          831 |         -12741 |       -10827 |          -79 |           7 |            14 |           20 |              20 |                47 |                7 |              24.1    | False         | False                      |
| LOLTMNT99_132665 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 15:03:35 |      1 |   14.01 |             100 | Blue   | team       | unknown team      | oe:team:ce499dea30cfce118f4fe85da0227e8 | Jarvan IV | Nocturne | Orianna   | Cassiopeia | Darius   | Lucian  | Karthus | Jax          | Jhin     | Braum    |         2122 | True     |      31 |       20 |        60 |          31 |           20 |             3 |             1 |             0 |            0 |            0 |     0.8765 | 1.442  | False         |         2 |             3 |                 2 |                     3 |           1 |           0 |        0 |        0 |           1 |          0 |        0 |            0 | False         |         0 |             1 |            4 |                2 | False        |        1 |            1 | False        |        8 |            8 |               0 |                    0 |              1 |                  8 |            1 |                1 |               97999 | 2770.94 |                3672.53 |                    2580.25 |           115 | 3.2516 |            40 | 1.131  |                   38 |           251 | 7.0971 |       72355 |        49333 |     1394.9   |       64958 | -0.00448513 |  0.52 |           875 |            278 | 32.6013 |      15788 |    18825 |      335 |          15876 |        18200 |          318 |            -88 |          625 |           17 |           2 |             2 |            3 |               3 |                 3 |                2 |      25949 |    30435 |      505 |          26024 |        29343 |          493 |            -75 |         1092 |           12 |           8 |            12 |            6 |               6 |                 5 |                8 |      36104 |    43211 |      701 |          35327 |        40489 |          677 |            777 |         2722 |           24 |          11 |            17 |            8 |               8 |                11 |               11 |      45691 |    55221 |      850 |          44232 |        51828 |          825 |           1459 |         3393 |           25 |          17 |            28 |           11 |              11 |                15 |               17 |              35.3667 | False         | False                      |
| LOLTMNT99_132665 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 15:03:35 |      1 |   14.01 |             200 | Red    | team       | unknown team      | oe:team:ce499dea30cfce118f4fe85da0227e8 | Akshan    | Neeko    | Akali     | Alistar    | Ezreal   | Varus   | Rell    | Elise        | Syndra   | Jayce    |         2122 | False    |      20 |       31 |        23 |          20 |           31 |             0 |             0 |             0 |            0 |            1 |     0.5655 | 1.442  | True          |         3 |             2 |                 3 |                     2 |           2 |           0 |        0 |        1 |           0 |          0 |        0 |            0 | True          |         1 |             0 |            2 |                4 | True         |        1 |            1 | True         |        8 |            8 |               1 |                    1 |              8 |                  1 |            1 |                1 |               92816 | 2624.39 |                3562.2  |                    2552.69 |           100 | 2.8275 |            51 | 1.442  |                   22 |           251 | 7.0971 |       66965 |        43943 |     1242.5   |       65250 |  0.00448513 | -0.52 |           897 |            202 | 31.0745 |      15876 |    18200 |      318 |          15788 |        18825 |          335 |             88 |         -625 |          -17 |           3 |             3 |            2 |               2 |                 2 |                3 |      26024 |    29343 |      493 |          25949 |        30435 |          505 |             75 |        -1092 |          -12 |           6 |             5 |            8 |               8 |                12 |                6 |      35327 |    40489 |      677 |          36104 |        43211 |          701 |           -777 |        -2722 |          -24 |           8 |            11 |           11 |              11 |                17 |                8 |      44232 |    51828 |      825 |          45691 |        55221 |          850 |          -1459 |        -3393 |          -25 |          11 |            15 |           17 |              17 |                28 |               11 |              35.3667 | False         | False                      |
| LOLTMNT99_132755 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 16:10:07 |      1 |   14.01 |             100 | Blue   | team       | unknown team      | oe:team:ce499dea30cfce118f4fe85da0227e8 | Nocturne  | Kalista  | Jarvan IV | LeBlanc    | Jayce    | Varus   | Aatrox  | Renata Glasc | Lee Sin  | Akali    |         2099 | True     |      24 |        8 |        54 |          24 |            8 |             2 |             0 |             0 |            0 |            0 |     0.686  | 0.9147 | True          |         2 |             3 |                 2 |                     3 |           1 |           0 |        0 |        0 |           0 |          1 |        0 |            0 | True          |         1 |             0 |            2 |                4 | True         |        1 |            0 | False        |        9 |            3 |               0 |                    0 |              1 |                  3 |            1 |                0 |               72699 | 2078.1  |                3198.38 |                    3079.98 |           124 | 3.5445 |            51 | 1.4578 |                   38 |           261 | 7.4607 |       68226 |        45438 |     1298.85  |       60155 |  0.0857641  | -0.16 |           879 |            225 | 31.5579 |      14550 |    17416 |      291 |          17133 |        19134 |          331 |          -2583 |        -1718 |          -40 |           1 |             2 |            4 |               4 |                 7 |                1 |      24170 |    29638 |      472 |          24731 |        29228 |          508 |           -561 |          410 |          -36 |           4 |             6 |            4 |               4 |                 7 |                4 |      33386 |    42148 |      666 |          34914 |        42870 |          706 |          -1528 |         -722 |          -40 |           7 |             9 |            7 |               7 |                11 |                7 |      43051 |    53899 |      822 |          41959 |        51633 |          854 |           1092 |         2266 |          -32 |          10 |            19 |            7 |               7 |                11 |               10 |              34.9833 | False         | False                      |


## Univariate Analysis: Distribution of Key Objectives

#### **Visualization: Distribution of Dragons Secured**
Below is a histogram displaying the exact number of dragons secured across all games.

<iframe
  src="../assets/drag_sec.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The histogram shows that most teams secure between 1 and 4 dragons per game, with securing 2 dragons being the most common. Teams securing more than 4 dragons are rare, and securing 6 or more dragons is almost negligible. This supports the notion that most games don't last long enough to secure excessive numbers of dragons, aligning with common competitive strategies focusing on early dragon stacking.

---

#### **Visualization: Distribution of Void Grubs Secured**
Below is a histogram displaying the exact number of void grubs collected across all games.

<iframe
  src="../assets/void_sec.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that most teams collect 3 or 6 void grubs in a game. Collecting 3 void grubs often indicates partial control over early-game objectives, while securing 6 grubs may signify dominance in void grub-focused strategies. The frequency of securing 6 grubs supports theories favoring a tempo-based approach, where void grubs enable faster tower destruction and map control.

---

#### **Visualization: Proportion of Games with First Objectives Secured**
Below is a bar chart displaying the proportion of games in which each first objective (first dragon, first herald, first tower, first baron, and optionally first grub) was secured.

<iframe
  src="../assets/first_obj.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The bar chart indicates that securing the first dragon, first herald, and first tower is equally likely in 50% of games. The first baron is slightly less common, appearing in 48% of games. These findings emphasize the critical role of early-game objectives in determining control and momentum.

---

#### **How These Plots Answer the Question**
The dragons histogram underscores the importance of securing a manageable number of dragons (1-4) for most teams, suggesting that full dragon stacking is less common and harder to achieve. Meanwhile, the void grubs histogram highlights the potential value of tempo-based strategies, as securing 3 or 6 grubs is more common and impactful. The first objective proportions reinforce the significance of early-game control, supporting theories that prioritize tempo-focused objectives like void grubs and Rift Heralds over traditional dragon stacking.
