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

