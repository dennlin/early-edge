# EarlyEdge: League of Legends Tempo vs. Value Objective Statistical Analysis

This project is a comprehensive data-driven exploration of objective prioritization in professional League of Legends matches in the 2024 meta. Inspired by the strategic shift from traditional dragon stacking to tempo-driven strategies, the analysis delves into the impact of objectives like void grubs, Rift Heralds, and dragons on game outcomes. Through detailed data analysis and hypothesis testing, the project investigates whether prioritizing tempo-based objectives correlates more strongly with game victories than value-based objectives. 

Author: Dennis Lin

## Introduction

#### **General Introduction**

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

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

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

<div style="overflow-x: auto;">

| gameid           | datacompleteness   | league   |   year | split   |   playoffs | date                |   game |   patch |   participantid | side   | position   | teamname          | teamid                                  | ban1      | ban2     | ban3      | ban4       | ban5     | pick1   | pick2   | pick3        | pick4    | pick5    |   gamelength | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   team kpm |   ckpm | firstdragon   |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   elders |   opp_elders | firstherald   |   heralds |   opp_heralds |   void_grubs |   opp_void_grubs | firstbaron   |   barons |   opp_barons | firsttower   |   towers |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   goldspent |        gspd |   gpr |   minionkills |   monsterkills |    cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |   goldat20 |   xpat20 |   csat20 |   opp_goldat20 |   opp_xpat20 |   opp_csat20 |   golddiffat20 |   xpdiffat20 |   csdiffat20 |   killsat20 |   assistsat20 |   deathsat20 |   opp_killsat20 |   opp_assistsat20 |   opp_deathsat20 |   goldat25 |   xpat25 |   csat25 |   opp_goldat25 |   opp_xpat25 |   opp_csat25 |   golddiffat25 |   xpdiffat25 |   csdiffat25 |   killsat25 |   assistsat25 |   deathsat25 |   opp_killsat25 |   opp_assistsat25 |   opp_deathsat25 |   gamelength_minutes | dragon_soul   | stole_elder_without_soul   |
|:-----------------|:-------------------|:---------|-------:|:--------|-----------:|:--------------------|-------:|--------:|----------------:|:-------|:-----------|:------------------|:----------------------------------------|:----------|:---------|:----------|:-----------|:---------|:--------|:--------|:-------------|:---------|:---------|-------------:|:---------|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------:|-------:|:--------------|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|---------:|-------------:|:--------------|----------:|--------------:|-------------:|-----------------:|:-------------|---------:|-------------:|:-------------|---------:|-------------:|----------------:|---------------------:|---------------:|-------------------:|-------------:|-----------------:|--------------------:|--------:|-----------------------:|---------------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------:|------------:|------:|--------------:|---------------:|--------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|---------------------:|:--------------|:---------------------------|
| LOLTMNT99_132542 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |             100 | Blue   | team       | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | Orianna   | Rell     | Viego     | Viktor     | Malphite | LeBlanc | Varus   | Renata Glasc | Renekton | Xin Zhao |         1446 | True     |      20 |        7 |        47 |          20 |            7 |             3 |             1 |             0 |            0 |            0 |     0.8299 | 1.1203 | True          |         2 |             1 |                 2 |                     1 |           0 |           0 |        1 |        0 |           0 |          1 |        0 |            0 | True          |         1 |             0 |            0 |                6 | True         |        1 |            0 | True         |        9 |            1 |               1 |                    1 |              8 |                  5 |            1 |                0 |               61477 | 2550.91 |                3214.65 |                    2552.07 |            64 | 2.6556 |            39 | 1.6183 |                   30 |           186 | 7.7178 |       52523 |        36396 |     1510.21  |       42725 |  0.166033   |  2.7  |           700 |            131 | 34.4813 |      17004 |    18250 |      328 |          15640 |        17693 |          311 |           1364 |          557 |           17 |           4 |             6 |            2 |               2 |                 3 |                4 |      27034 |    30021 |      530 |          24741 |        29072 |          507 |           2293 |          949 |           23 |           6 |            10 |            3 |               3 |                 5 |                6 |      38246 |    42428 |      718 |          33998 |        40290 |          668 |           4248 |         2138 |           50 |          10 |            21 |            5 |               5 |                10 |               10 |      52523 |    58329 |      831 |          39782 |        47502 |          752 |          12741 |        10827 |           79 |          20 |            47 |            7 |               7 |                14 |               20 |              24.1    | False         | False                      |
| LOLTMNT99_132542 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |             200 | Red    | team       | Dark Passage      | oe:team:a42600a99997fca0d35f2821662b997 | Rumble    | Ashe     | Nocturne  | Poppy      | Vi       | Kalista | Neeko   | Lee Sin      | Nautilus | Jax      |         1446 | False    |       7 |       20 |        14 |           7 |           20 |             0 |             0 |             0 |            0 |            1 |     0.2905 | 1.1203 | False         |         1 |             2 |                 1 |                     2 |           1 |           0 |        0 |        0 |           0 |          0 |        0 |            0 | False         |         0 |             1 |            6 |                0 | False        |        0 |            1 | False        |        1 |            9 |               0 |                    0 |              5 |                  8 |            0 |                1 |               42737 | 1773.32 |                3182.32 |                    2857.97 |            70 | 2.9046 |            22 | 0.9129 |                   23 |           141 | 5.8506 |       39782 |        23655 |      981.535 |       36175 | -0.166033   | -2.7  |           612 |            140 | 31.2033 |      15640 |    17693 |      311 |          17004 |        18250 |          328 |          -1364 |         -557 |          -17 |           2 |             3 |            4 |               4 |                 6 |                2 |      24741 |    29072 |      507 |          27034 |        30021 |          530 |          -2293 |         -949 |          -23 |           3 |             5 |            6 |               6 |                10 |                3 |      33998 |    40290 |      668 |          38246 |        42428 |          718 |          -4248 |        -2138 |          -50 |           5 |            10 |           10 |              10 |                21 |                5 |      39782 |    47502 |      752 |          52523 |        58329 |          831 |         -12741 |       -10827 |          -79 |           7 |            14 |           20 |              20 |                47 |                7 |              24.1    | False         | False                      |
| LOLTMNT99_132665 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 15:03:35 |      1 |   14.01 |             100 | Blue   | team       | unknown team      | oe:team:ce499dea30cfce118f4fe85da0227e8 | Jarvan IV | Nocturne | Orianna   | Cassiopeia | Darius   | Lucian  | Karthus | Jax          | Jhin     | Braum    |         2122 | True     |      31 |       20 |        60 |          31 |           20 |             3 |             1 |             0 |            0 |            0 |     0.8765 | 1.442  | False         |         2 |             3 |                 2 |                     3 |           1 |           0 |        0 |        0 |           1 |          0 |        0 |            0 | False         |         0 |             1 |            4 |                2 | False        |        1 |            1 | False        |        8 |            8 |               0 |                    0 |              1 |                  8 |            1 |                1 |               97999 | 2770.94 |                3672.53 |                    2580.25 |           115 | 3.2516 |            40 | 1.131  |                   38 |           251 | 7.0971 |       72355 |        49333 |     1394.9   |       64958 | -0.00448513 |  0.52 |           875 |            278 | 32.6013 |      15788 |    18825 |      335 |          15876 |        18200 |          318 |            -88 |          625 |           17 |           2 |             2 |            3 |               3 |                 3 |                2 |      25949 |    30435 |      505 |          26024 |        29343 |          493 |            -75 |         1092 |           12 |           8 |            12 |            6 |               6 |                 5 |                8 |      36104 |    43211 |      701 |          35327 |        40489 |          677 |            777 |         2722 |           24 |          11 |            17 |            8 |               8 |                11 |               11 |      45691 |    55221 |      850 |          44232 |        51828 |          825 |           1459 |         3393 |           25 |          17 |            28 |           11 |              11 |                15 |               17 |              35.3667 | False         | False                      |
| LOLTMNT99_132665 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 15:03:35 |      1 |   14.01 |             200 | Red    | team       | unknown team      | oe:team:ce499dea30cfce118f4fe85da0227e8 | Akshan    | Neeko    | Akali     | Alistar    | Ezreal   | Varus   | Rell    | Elise        | Syndra   | Jayce    |         2122 | False    |      20 |       31 |        23 |          20 |           31 |             0 |             0 |             0 |            0 |            1 |     0.5655 | 1.442  | True          |         3 |             2 |                 3 |                     2 |           2 |           0 |        0 |        1 |           0 |          0 |        0 |            0 | True          |         1 |             0 |            2 |                4 | True         |        1 |            1 | True         |        8 |            8 |               1 |                    1 |              8 |                  1 |            1 |                1 |               92816 | 2624.39 |                3562.2  |                    2552.69 |           100 | 2.8275 |            51 | 1.442  |                   22 |           251 | 7.0971 |       66965 |        43943 |     1242.5   |       65250 |  0.00448513 | -0.52 |           897 |            202 | 31.0745 |      15876 |    18200 |      318 |          15788 |        18825 |          335 |             88 |         -625 |          -17 |           3 |             3 |            2 |               2 |                 2 |                3 |      26024 |    29343 |      493 |          25949 |        30435 |          505 |             75 |        -1092 |          -12 |           6 |             5 |            8 |               8 |                12 |                6 |      35327 |    40489 |      677 |          36104 |        43211 |          701 |           -777 |        -2722 |          -24 |           8 |            11 |           11 |              11 |                17 |                8 |      44232 |    51828 |      825 |          45691 |        55221 |          850 |          -1459 |        -3393 |          -25 |          11 |            15 |           17 |              17 |                28 |               11 |              35.3667 | False         | False                      |
| LOLTMNT99_132755 | complete           | TSC      |   2024 | Winter  |          0 | 2024-01-05 16:10:07 |      1 |   14.01 |             100 | Blue   | team       | unknown team      | oe:team:ce499dea30cfce118f4fe85da0227e8 | Nocturne  | Kalista  | Jarvan IV | LeBlanc    | Jayce    | Varus   | Aatrox  | Renata Glasc | Lee Sin  | Akali    |         2099 | True     |      24 |        8 |        54 |          24 |            8 |             2 |             0 |             0 |            0 |            0 |     0.686  | 0.9147 | True          |         2 |             3 |                 2 |                     3 |           1 |           0 |        0 |        0 |           0 |          1 |        0 |            0 | True          |         1 |             0 |            2 |                4 | True         |        1 |            0 | False        |        9 |            3 |               0 |                    0 |              1 |                  3 |            1 |                0 |               72699 | 2078.1  |                3198.38 |                    3079.98 |           124 | 3.5445 |            51 | 1.4578 |                   38 |           261 | 7.4607 |       68226 |        45438 |     1298.85  |       60155 |  0.0857641  | -0.16 |           879 |            225 | 31.5579 |      14550 |    17416 |      291 |          17133 |        19134 |          331 |          -2583 |        -1718 |          -40 |           1 |             2 |            4 |               4 |                 7 |                1 |      24170 |    29638 |      472 |          24731 |        29228 |          508 |           -561 |          410 |          -36 |           4 |             6 |            4 |               4 |                 7 |                4 |      33386 |    42148 |      666 |          34914 |        42870 |          706 |          -1528 |         -722 |          -40 |           7 |             9 |            7 |               7 |                11 |                7 |      43051 |    53899 |      822 |          41959 |        51633 |          854 |           1092 |         2266 |          -32 |          10 |            19 |            7 |               7 |                11 |               10 |              34.9833 | False         | False                      |
</div>


### Univariate Analysis: Distribution of Key Objectives

#### **Visualization: Distribution of Dragons Secured**
Below is a histogram displaying the exact number of dragons secured across all games.

<iframe
  src="assets/drag_sec.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The histogram shows that most teams secure between 1 and 4 dragons per game, with securing 2 dragons being the most common. Teams securing more than 4 dragons are rare, and securing 6 or more dragons is almost negligible. This supports the notion that most games don't last long enough to secure excessive numbers of dragons, aligning with common competitive strategies focusing on early dragon stacking.

---

#### **Visualization: Distribution of Void Grubs Secured**
Below is a histogram displaying the exact number of void grubs collected across all games.

<iframe
  src="assets/void_sec.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that most teams collect 3 or 6 void grubs in a game. Collecting 3 void grubs often indicates partial control over early-game objectives, while securing 6 grubs may signify dominance in void grub-focused strategies. The frequency of securing 6 grubs supports theories favoring a tempo-based approach, where void grubs enable faster tower destruction and map control.

---

#### **Visualization: Proportion of Games with First Objectives Secured**
Below is a bar chart displaying the proportion of games in which each first objective (first dragon, first herald, first tower, first baron, and optionally first grub) was secured.

<iframe
  src="assets/first_obj.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The bar chart indicates that securing the first dragon, first herald, and first tower is equally likely in 50% of games. The first baron is slightly less common, appearing in 48% of games. These findings emphasize the critical role of early-game objectives in determining control and momentum.

---

#### **How These Plots Answer the Question**
The dragons histogram underscores the importance of securing a manageable number of dragons (1-4) for most teams, suggesting that full dragon stacking is less common and harder to achieve. Meanwhile, the void grubs histogram highlights the potential value of tempo-based strategies, as securing 3 or 6 grubs is more common and impactful. The first objective proportions reinforce the significance of early-game control, supporting theories that prioritize tempo-focused objectives like void grubs and Rift Heralds over traditional dragon stacking.


### Bivariate Analysis: Relationships Between Objectives and Game Outcomes

#### **Visualization: Dragons Secured by Game Result**
The box plot below displays the distribution of dragons secured by game result (win/loss). The data highlights how successful teams tend to secure significantly more dragons on average compared to teams that lose.

<iframe
  src="assets/drag_sec_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The visualization shows a clear trend: winning teams (`True`) secure more dragons than losing teams (`False`). Winning teams secure a median of 3 dragons compared to 1 for losing teams. The mean number of dragons secured by winning teams (2.95) is nearly double that of losing teams (1.48). This strongly supports the idea that dragon control contributes significantly to late-game success.

---

#### **Visualization: Void Grubs Secured by Game Result**
The box plot below displays the number of void grubs secured by game result.

<iframe
  src="assets/void_sec_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Winning teams (`True`) secure more void grubs on average (3.08) than losing teams (2.42). This suggests that tempo advantages gained from void grubs may be a deciding factor in competitive matches. The narrower spread among losing teams suggests less frequent use of void grub strategies.

---

#### **Visualization: Dragon Soul Presence by Game Result**
The bar plot below shows the relationship between dragon soul presence and game result.

<iframe
  src="assets/soul_sec.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Teams that secure dragon soul are significantly more likely to win (2937 wins with dragon soul compared to 379 losses). However, there are many games where teams win without securing a dragon soul (5365), emphasizing that other factors, such as map control and tempo, can also lead to victory.

---

#### **Visualization: Game Length and Dragons/Void Grubs Secured**
Scatter plots reveal how game length correlates with the number of dragons and void grubs secured.

<iframe
  src="assets/drag_sec_len.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/void_sec_len.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| Correlation | Dragons Secured | Void Grubs Secured |
|-------------|-----------------|--------------------|
| Value       | 0.37            | -0.01             |

**Dragons Secured**: A moderate positive correlation (0.37) indicates that longer games provide more opportunities to secure dragons.  
**Void Grubs Secured**: The near-zero correlation (-0.01) suggests that void grubs are consistently prioritized regardless of game length, supporting their value in shorter games for early tempo control.

---

#### **Game Length vs. Dragon Soul Presence**
The table below summarizes dragon soul presence by game length:

<iframe
  src="assets/soul_sec_len.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| Game Length        | Dragon Soul Presence | Count |
|--------------------|-----------------------|-------|
| Short              | False                |    34 |
| Short              | True                 |     0 |
| Moderately Short   | False                |  4444 |
| Moderately Short   | True                 |   534 |
| Average            | False                |  7564 |
| Average            | True                 |  2032 |
| Moderately Long    | False                |  1162 |
| Moderately Long    | True                 |   700 |
| Long               | False                |    86 |
| Long               | True                 |    50 |

Shorter games (under 20 minutes) rarely result in dragon soul acquisition, while average and moderately long games (20–40 minutes) show a higher frequency of dragon soul presence among winning teams. Longer games (over 40 minutes) appear to result in fewer dragon souls, suggesting the importance of tempo-focused objectives like void grubs in those cases.

---

### **How These Plots Answer the Question**

1. **Game Result and Objectives**:
   - Winning teams consistently secure more dragons, void grubs, and dragon souls compared to losing teams. These findings validate the significance of both tempo (grubs) and value (dragons) in determining game outcomes.

2. **Game Length and Objective Dynamics**:
   - Dragons correlate positively with game length, while void grubs do not, emphasizing that dragon stacking is only effective in longer games.
   - Dragon soul is a decisive factor in longer matches but less relevant in shorter, fast-paced games.

**Conclusion**:
The data suggests a dual strategy: in shorter games, focus on tempo-driven objectives like void grubs and heralds, while in longer games, dragon stacking and soul acquisition become pivotal. This nuanced understanding aligns with evolving strategies in the 2024 professional League of Legends meta.


### Interesting Aggregates

#### Visualization: Aggregated Statistics by Objective Type and Game Result
The graph and table below summarizes the average number of objectives secured (tempo-based: void grubs and heralds; value-based: dragons and elders) and their correlation with game results.

<iframe
  src="assets/av_obj.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| **Game Result** | **Dragon Soul** | **Avg Void Grubs** | **Avg Heralds** | **Avg Towers** | **Avg Dragons** | **Avg Elders** |
|------------------|-----------------|---------------------|------------------|----------------|-----------------|----------------|
| False            | False          | 2.43               | 0.36            | 2.85           | 1.35            | 0.02           |
| False            | True           | 2.17               | 0.50            | 4.99           | 4.12            | 0.12           |
| True             | False          | 3.43               | 0.64            | 9.31           | 2.27            | 0.03           |
| True             | True           | 2.44               | 0.61            | 9.38           | 4.19            | 0.19           |

**Key Insights**:
- Teams that win without securing dragon soul have the highest average tempo-based objectives (**3.43 void grubs** and **0.64 heralds**) and destroy significantly more towers (**9.31**).
- Dragon soul correlates with increased tower destruction and elder dragon control, but tempo objectives remain impactful even without dragon soul.
- Teams securing tempo objectives (void grubs and heralds) without relying on dragon soul demonstrate a flexible approach to game success.

---

#### Visualization: Median Towers by Tempo Objectives
This scatter plot illustrates the median number of towers destroyed based on the number of void grubs and heralds secured.

<iframe
  src="assets/towers_by_top.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insights**:
- Teams with **1 herald** consistently destroy more towers (median of **8** or **9**), regardless of the number of void grubs secured.
- Without heralds, void grubs alone result in fewer towers destroyed (median of **3-4**), emphasizing the combined importance of tempo objectives.
- A higher number of void grubs combined with herald control correlates with increasingly dominant map control.

---

#### Visualization: Pivot Table - Objectives and Win Rates
The following pivot table displays win rates by combinations of objectives secured. The table highlights void grubs, heralds, dragon soul, and total dragons.

<iframe
  src="assets/winrate_by_obj.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| **Void Grubs** | **Heralds** | **Dragon Soul** | **Dragons** | **Win Rate** |
|----------------|-------------|-----------------|-------------|--------------|
| 0.0            | 0.0         | False           | 2.0         | 21%          |
| 0.0            | 1.0         | True            | 4.0         | 88%          |
| 3.0            | 1.0         | False           | 4.0         | 83%          |
| 6.0            | 1.0         | True            | 5.0         | 100%         |

**Key Insights**:
- Win rates peak when both tempo and value objectives are secured. For example, teams with **6 void grubs, 1 herald, and dragon soul** achieve a **100% win rate**.
- Teams that only secure tempo objectives (e.g., **3 void grubs and 1 herald**) can still achieve high win rates (**83%**) without dragon soul.
- Value objectives (like dragon soul) amplify the effectiveness of tempo objectives, underscoring their complementary nature.

---

#### Correlation Matrix: Tempo vs. Value Objectives
The correlation matrix quantifies relationships between tempo objectives (void grubs, heralds), value objectives (dragon soul, dragons, elders), and outcomes (towers, win rates).

<iframe
  src="assets/tempo_vs_value_obj.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| **Metric**       | **Void Grubs** | **Heralds** | **Dragon Soul** | **Dragons** | **Towers** | **Elders** |
|-------------------|----------------|-------------|-----------------|-------------|------------|------------|
| **Void Grubs**    | 1.00           | 0.21        | -0.08           | -0.07       | 0.22       | -0.03      |
| **Heralds**       | 0.21           | 1.00        | 0.10            | 0.16        | 0.32       | 0.01       |
| **Dragon Soul**   | -0.08          | 0.10        | 1.00            | 0.71        | 0.38       | 0.25       |

**Key Insights**:
- **Heralds** correlate more strongly with tower destruction (**0.32**) than void grubs (**0.22**), emphasizing the importance of herald control for tempo strategies.
- Dragon soul correlates strongly with total dragons (**0.71**) but has a weaker link to towers (**0.38**), suggesting that while impactful, it supports sustained rather than immediate control.
- Elders show limited direct correlation with tempo-based objectives, highlighting their role as a late-game value amplifier.

---

### How This Answers the Question
These analyses support the hypothesis that tempo-based objectives (void grubs and heralds) are highly impactful in professional matches. 
- **Tempo Objectives**: Teams that excel in securing tempo objectives frequently outperform opponents, even without dragon soul, by leveraging map control and tower destruction.
- **Value Objectives**: Dragon soul and elders contribute significantly in prolonged matches but are most effective when combined with tempo strategies.
This interplay emphasizes the adaptability required in professional play, showcasing how tempo and value objectives synergize to optimize win conditions.


### Imputation

#### Description of Data
The `split` column represents categorical data corresponding to competitive splits and events, such as `Winter`, `Spring`, `Summer`, etc.

#### Imputation Technique
- **Approach**: Missing values in the `split` column were imputed using the **mode** (the most frequent category), which was `Summer`.
- **Rationale**: 
  - Using the mode preserves the natural distribution of the data by filling missing values with the most common category.
  - This method avoids introducing arbitrary or unrelated values into the dataset.
  - It’s particularly suitable for categorical columns, as it prevents distortion in trends.

#### Visualizations of Distribution

Below are visualizations of the `split` column distributions before and after imputation:

1. **Before Imputation**:
   - Displays the frequencies of each category with missing data excluded.

2. **After Imputation**:
   - Shows the updated frequencies with missing values filled using the mode (`Summer`).

<iframe
  src="assets/before_clean.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/after_clean.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The increase in the `Summer` category reflects the imputed values.

---

### Answering The Question

#### Summary of Findings
This analysis of professional League of Legends matches in the 2024 meta reveals key insights into how tempo-based and value-based objectives influence game outcomes. By examining aggregated statistics, bivariate relationships, and pivot tables, we identified trends that highlight the interplay between these objective types:

1. **Tempo-Based Objectives (Void Grubs and Heralds)**:
   - Tempo objectives, particularly void grubs and heralds, are strongly correlated with immediate map control and tower destruction.
   - Teams that secure heralds alongside void grubs achieve significant advantages in terms of map pressure, enabling faster game progression and higher win rates.

2. **Value-Based Objectives (Dragon Soul and Elders)**:
   - Dragon soul provides impactful late-game scaling but is most effective when combined with tempo-based objectives.
   - Elders appear less correlated with tempo metrics, serving as a pivotal factor in extended games where dragon control becomes critical.

3. **Objective Interplay**:
   - The combination of tempo and value objectives creates a powerful synergy. Teams that excel in both domains achieve peak performance, as seen in cases where high void grub counts, herald control, and dragon soul align to yield near-perfect win rates.
   - Tempo objectives alone can drive strong win rates in the absence of dragon soul, underscoring their versatility.

#### Implications for Strategy
1. **Adaptability is Key**: Teams should prioritize tempo-based objectives for early-game dominance while remaining flexible to incorporate value objectives like dragon soul in prolonged games.
2. **Tempo as a Foundation**: Success often begins with effective void grub and herald control, which lays the groundwork for subsequent value-based objectives.
3. **Holistic Approach**: The most successful teams balance tempo and value objectives, tailoring their strategies to game duration and opponent strengths.

#### Limitations

While the analysis provides valuable insights into the 2024 professional meta, there are several limitations to consider that may influence the findings:

1. **Human Error and Execution**:
   - Even at the professional level, players and teams are not immune to mistakes, particularly during the mid-game. Early-game compositions that rely on precise execution to maintain their lead may falter if a team fails to capitalize on their tempo advantage or makes critical errors in transitioning from early to mid-game.
   - For instance, securing tempo objectives such as void grubs and heralds offers a strong early advantage, but if the team lacks coordination or misplays key fights, this advantage can be squandered.

2. **Scaling vs. Tempo Trade-Off**:
   - Scaling compositions inherently carry a higher level of risk ("coinflipping") in the current meta. Teams that opt for scaling objectives like dragon soul or elder dragons may find themselves outpaced by tempo-driven teams if they fail to stall the game long enough for their strategy to come online.
   - This risk-reward trade-off underscores the importance of execution, as scaling teams must survive the early onslaught of tempo-driven aggression to reach their win condition.

3. **Equal Skill Assumption**:
   - The analysis assumes relatively equal skill levels across teams, but in reality, disparities in skill, teamwork, and experience can heavily influence outcomes. Even with a tempo advantage, a weaker team may struggle to leverage their objectives effectively against a more skilled opponent.

4. **Dataset-Specific Constraints**:
   - The dataset focuses exclusively on professional matches, which may not fully represent broader trends in amateur or ranked gameplay.
   - Additionally, the analysis is limited to objective-based metrics available in the dataset and does not account for other factors like draft strategy, individual player skill, or real-time in-game adaptations.

Despite these limitations, the findings highlight the growing importance of tempo-based strategies in the 2024 meta. Teams must weigh the trade-offs between tempo and scaling objectives, acknowledging that success hinges not only on strategic choices but also on flawless execution and adaptation during matches.


#### Conclusion

The analysis provides a clear answer to the question: **"In the 2024 professional meta, does prioritizing tempo-based objectives (void grub collection and Rift Herald control) over value-based objectives (dragon stacking and Elder fights) correlate more strongly with game victories?"**

The findings strongly support the hypothesis that tempo-based objectives are more consistently correlated with game victories in the 2024 meta. Teams that prioritized void grubs and heralds demonstrated substantial advantages in tower destruction and map control, with some achieving high win rates even in the absence of dragon soul. However, the interplay between tempo and value objectives is critical, as the highest win rates occurred when both types of objectives were secured.

By focusing on tempo objectives for early dominance and leveraging value objectives when opportunities arise, teams can adapt to the 2024 professional meta and optimize their chances of success.


## Framing a Prediction Problem

#### Problem Statement
The prediction problem is:

**"Can we predict the likelihood of a team winning a game based on their early-game control of tempo and value objectives?"**

#### Problem Type
This is a **binary classification problem**, where the response variable (`result`) indicates whether a team wins (`1`) or loses (`0`) a game.

#### Response Variable
- **Response Variable**: `result` (1 = Win, 0 = Loss)
- **Justification**: The `result` variable directly measures the outcome of the game, which aligns with the goal of understanding the impact of early-game objective control on game success.

#### Evaluation Metric
- **Metric**: F1-Score
- **Reason for Choosing F1-Score**:
  - League of Legends match data often involves class imbalance (e.g., more losses than wins in certain subsets of the data).
  - F1-Score balances precision (minimizing false positives) and recall (minimizing false negatives), making it ideal for evaluating predictive accuracy in imbalanced datasets.
  - F1-Score is particularly suitable when both types of errors (predicting a win when it is a loss, or vice versa) are equally critical to the prediction problem.

#### Features Used for Prediction
The following features will be used, ensuring they represent information available **at the time of prediction** (i.e., early to mid-game):

- **Tempo-Based Objectives**:
  - `void_grubs`: Number of void grubs secured.
  - `heralds`: Number of Rift Heralds secured.
  - `firstherald`: Whether the team secured the first herald.
  - `firsttower`: Whether the team secured the first tower.
  - `towers_destroyed`: Total number of towers destroyed.

- **Value-Based Objectives**:
  - `dragons`: Number of dragons secured.
  - `dragon_soul`: Whether the team has secured the dragon soul.
  - `elders`: Number of elder dragons secured.
  - `firstdragon`: Whether the team secured the first dragon.

- **Game Context**:
  - `gamelength_minutes`: Duration of the game (in minutes).
  - `gold_diff`: Team's gold difference at the time of prediction (if available).

#### Justification for Features
These features focus on **early-game control and mid-game transitions**, ensuring they represent information observable at the time of prediction. Features such as `dragon_soul` are included because securing the soul reflects decisions and outcomes in earlier phases of the game. Late-game outcomes (e.g., Baron Nashor control) are excluded as they occur after the model's prediction point.

#### Coherence with the Question
This prediction problem complements the overarching analysis question by providing a quantitative framework to test the relative impact of tempo and value objectives on win probabilities. The classification model will help verify if teams prioritizing tempo objectives are more likely to win, particularly in shorter games.


## Baseline Model

#### Model Description
The baseline model is a **Random Forest Classifier**, chosen for its robustness and ability to handle both categorical and numerical data without extensive preprocessing. The model predicts the **game result** (`Win` or `Loss`) based on selected game features, reflecting key dynamics in competitive League of Legends gameplay.

#### Features Used in the Model
1. **Quantitative Features**:
   - `void_grubs`: Number of void grubs secured.
   - `heralds`: Number of Rift Heralds secured.
   - `dragons`: Number of dragons secured.
   - `gamelength_minutes`: Duration of the game in minutes.

2. **Nominal Features**:
   - `firstdragon`: Whether the team secured the first dragon (True/False).
   - `firstherald`: Whether the team secured the first herald (True/False).
   - `firsttower`: Whether the team secured the first tower (True/False).

3. **Encodings**:
   - **Numerical Features**: Standardized using `StandardScaler` for consistent scaling.
   - **Nominal Features**: Encoded using **OneHotEncoder** to allow the model to learn from binary and categorical data effectively.

#### Model Performance
1. **Training Data Performance**:
   - **Precision**: 1.00
   - **Recall**: 1.00
   - **F1-Score**: 0.9998
   - **Accuracy**: 100%

2. **Test Data Performance**:
   - **Precision**: 0.97
   - **Recall**: 0.97
   - **F1-Score**: 0.9683
   - **Accuracy**: 97%

#### Assessment of Model Quality
- **Strengths**:
  - The baseline model achieves **exceptional performance** on both training and test data, with a test F1-score of 0.9683 indicating strong generalization.
  - The inclusion of both tempo and value-based objectives reflects strategic insights, aligning the model with competitive game dynamics.
  - The **OneHotEncoder** effectively integrates nominal features like `firstherald` and `firstdragon`, preserving their categorical nature while enabling the model to learn from them.

- **Potential Concerns**:
  - The significant performance improvement from previous iterations suggests the addition of features like `towers` might have introduced **data leakage**. If the `towers` feature includes information available only after the game's outcome is determined, this could artificially inflate the model's performance.
  - The high training accuracy may also indicate potential **overfitting**, despite the strong generalization observed on the test set.

#### Is the Current Model “Good”?
- **Yes, with reservations.**
  - The model’s performance on both training and test data is **outstanding**, with near-perfect F1-scores indicating it effectively distinguishes between wins and losses.
  - However, further validation is needed to rule out data leakage and ensure the `towers` feature is appropriate for prediction at the time it is used.

<!-- #### Next Steps
To ensure the model remains robust and its performance is valid:
1. **Validate Feature Appropriateness**:
   - Confirm that features like `towers` are available and relevant at the time of prediction.
2. **Expand Feature Engineering**:
   - Introduce new derived features, such as interactions between tempo and value objectives (e.g., `grubs_per_minute`, `dragons_per_herald`).
3. **Hyperparameter Tuning**:
   - Optimize the Random Forest’s parameters using techniques like GridSearchCV to ensure the model’s architecture is balanced and not overly complex.
4. **Perform Cross-Validation**:
   - Validate the model on multiple data splits to confirm its generalization across different subsets of the dataset.

This baseline model establishes a strong foundation for refining predictions of game results in professional League of Legends matches. -->

## Final Model

#### Features Added
1. **`dragons_per_herald`**:
   - This feature captures the ratio of dragons secured to heralds secured, reflecting the balance between value (dragons) and tempo (heralds) objectives.
   - **Reason for inclusion**: Teams often have to choose between prioritizing dragons or heralds early in the game. This ratio can provide insights into how effectively teams allocate resources for objectives and how it impacts game outcomes.

2. **`grubs_per_minute`**:
   - This feature measures the rate of void grubs secured relative to game duration.
   - **Reason for inclusion**: Void grubs represent a critical tempo-based objective in the early game, and their rate of collection could indicate a team’s dominance in map control and tempo.

3. **`grubs_towers_interaction`**:
   - This interaction feature multiplies the number of void grubs secured by the number of towers destroyed.
   - **Reason for inclusion**: Void grubs contribute to map control, which facilitates tower destruction. This interaction captures the synergy between these two tempo-based objectives.

#### Modeling Algorithm
- **Algorithm Chosen**: Random Forest Classifier
  - Random Forest was selected due to its robustness, ability to handle both numerical and categorical features, and effectiveness in capturing complex interactions and nonlinear relationships in the data.

- **Hyperparameter Tuning**:
  - The following hyperparameters were tuned using `GridSearchCV`:
    - `n_estimators`: Number of trees in the forest.
    - `max_depth`: Maximum depth of the trees to prevent overfitting.
    - `max_features`: Number of features considered for each split.
  - **Best Hyperparameters**:
    - `n_estimators`: 50
    - `max_depth`: 20
    - `max_features`: `sqrt`
  - A 5-fold cross-validation was performed on the training set to identify the best hyperparameters, optimizing for the F1-score to balance precision and recall.

#### Performance Comparison
| **Metric**         | **Baseline Model** | **Final Model** |
|---------------------|--------------------|-----------------|
| **Training Accuracy** | 100%               | 95%             |
| **Test Accuracy**    | 97%                | 92%             |
| **Training F1-Score** | 1.00               | 0.95            |
| **Test F1-Score**    | 0.97               | 0.92            |

**Key Observations**:
- The Baseline Model achieved **excellent generalization** with a test F1-score of **0.97** and minimal overfitting, given the balanced performance across training and testing data.
- The Final Model introduced new engineered features that aimed to capture nuanced relationships between tempo- and value-based objectives.
- Despite the Baseline Model’s stronger raw performance, the Final Model still offers value in exploring feature engineering and its alignment with the game dynamics. However, further iterations may focus on refining the engineered features or exploring alternative models.

#### Confusion Matrix for Final Model
Below is the confusion matrix for the Final Model on the test set:

|             | Predicted Negative | Predicted Positive |
|-------------|--------------------:|--------------------:|
| **Actual Negative** | 1525               | 136                |
| **Actual Positive** | 122                | 1539               |

- The model shows balanced precision and recall for both classes, with minimal misclassifications, indicating robust generalization.

#### Conclusion
While the Final Model introduced new features and used hyperparameter tuning to optimize its structure, the Baseline Model already provided exceptional predictive performance. The Baseline Model’s incorporation of critical features like `towers` and `dragon_soul` likely captured much of the predictive power needed for game outcomes, reducing the marginal benefits of the additional features in the Final Model. Further improvements could involve deeper exploration of interaction terms or alternative machine learning algorithms.
