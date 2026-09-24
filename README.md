# NBA_player_clustering


Readme · MD
# NBA Player Clustering & Playstyle Similarity Model
 
Groups NBA players into playstyle archetypes using unsupervised learning on shot location data and per game player statistics, then test whether those archetypes can be predicted.
 
## Key Results
 
- Clustered over 500 players into 4 playstyle archetypes (e.g. perimeter scorer, interior scorer, isolation scorer) using K-Means on 232,000+ shot records merged with per game statistics
- Validated the relationship between shot location features and playstyle with a Chi-square test of independence (Cramér's V = 0.449) before using them in the model
- Trained a Random Forest classifier (GridSearchCV-tuned) to predict cluster from statistical features, reaching 95% cross-validated accuracy
- Built a cosine similarity function that returns the most playstyle similar players to any given player
## Data
 
- **Shot data**: `NBA_2023_24_shot_data.csv` — 232,541 individual shot attempts for the 2023-24 NBA season, including shot zone, shot distance, and action type
- **Season stats**: `nba_player_stats_2023_24.csv` — per-player box score stats (rebounds, assists, shooting splits, etc.) for the same season

Data source for shot data: *(https://www.kaggle.com/datasets/jackchen019/nba-2023-24-player-shooting-datainclude-playoffs/data)*

Data source for player per game data: (https://www.basketball-reference.com/leagues/NBA_2024_per_game.html) 


 
The raw CSVs aren't included in this repo due to file size. To run the notebook, download the data from the source above and place both files in the project root.
 
## Methodology
 
1. **Data prep** — merged shot level and season stat datasets on player name, engineered shot-zone proportion features, converted counting stats to per game rates
2. **Feature validation** — used a Chi-square test + Cramér's V to confirm shot-location features meaningfully relate to playstyle before including them
3. **Clustering** — MinMax-scaled features (to weight shot selection over raw production), used the elbow method to select k=4, then interpreted each cluster's stats to label playstyle archetypes
4. **Classification** — trained a Random Forest (GridSearchCV for hyperparameters) to predict cluster membership from stats
5. **Similarity search** — cosine similarity over player feature vectors to recommend playstyle-similar players
## Limitations & Future Work
 
The classifier in step 4 is trained on the same features used to generate the clusters in step 3, so its accuracy mainly confirms the clusters are internally consistent rather than proving predictive power on unseen data. A more accurate version would predict cluster membership from a reduced feature set (box-score stats only, no shot-location data) to test whether playstyle can be inferred without shot chart data.
 
## Tech Stack
 
Python · pandas · scikit-learn · SciPy · seaborn
 
## Setup
 
```bash
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Mac/Linux
 
pip install -r requirements.txt
```
 
Then open `nba_player_clustering.ipynb` in Jupyter or VS Code and run all cells (data files must be present in the project root — see Data section above).
 
## Repository Structure
 
```
NBA_player_clustering/
├── nba_player_clustering.ipynb   # main analysis notebook
├── requirements.txt              # package dependencies
└── README.md
```
