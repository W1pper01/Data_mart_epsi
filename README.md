# Analyse de Statistiques CS:GO (Projet ELT)

## Equipe
* **Garcia Robin**
* **Boutet Thomas**

---

## Jeu de Donnees (Etape 1)
* **Source :** https://www.kaggle.com/datasets/mateusdmachado/csgo-professional-matches
* **Contenu :** Environ 50 000 matchs competitifs.
* **Caracteristiques :** 4 fichiers CSV (results.csv, players.csv, picks.csv, economy.csv) contenant des statistiques in-game detaillees.

---

## Architecture Technique (Etape 2)

| Composant | Technologie | Role |
| :--- | :--- | :--- |
| **Extract/Load** | PostgreSQL 15 | Stockage des donnees brutes (RAW) et transformees. |
| **Transformation** | Python (Pandas & SQLAlchemy) | Nettoyage, typage et normalisation des donnees. |
| **Visualisation** | Metabase | Creation de dashboards analytiques. |
| **Orchestration** | Docker Compose | Conteneurisation de toute l'infrastructure. |

---

## Installation et Lancement

### 1. Prerequis
* Docker Desktop et Python 3.10+
* Extraire les fichiers CSV dans le dossier `./data/`

### 2. Demarrage
```bash
# Lancer l'infrastructure (Base de donnees + Metabase)
docker-compose up -d
```

### 3. Execution de l'ETL
Ouvrir et executer le notebook : ETL.ipynb. 
Ce script automatise l'extraction/importation (Extract-Load) et la restructuration des donnees (Transform).


## Modèle relationnel (Etape 3)
### Schéma écrit
player\
player_team_match\
    id_player(pk;fk);
    id_team_match(pk;fk);
    stats_joueur_dans_match;

team\
team_match\
    id_team(pk;fk);
    id_match(pk;fk);
    winner: bool;
    stat_team_dans_match;

match\
id_match(pk)
id_map(fk)
map\
id_map(pk)

### Cardinalités

Un joueur peut participer à plusieurs matchs (1,N)\
Une équipe peut jouer plusieurs matchs (1,N)\
Un match se joue sur une carte (1,1)\
Un match implique deux équipes via team_match (2,2)\
Chaque participation de joueur est liée à une équipe et un match via team_match

### Schéma Merdmaid 
[![](https://mermaid.ink/img/pako:eNqVVF2P2jAQ_CuWnwMKJATIG7reqRJFQtX1oRVStI03xCWxI9tpS4H_3jghveaDSvVTdnZ2Zr2xfaGxZEhDiuodh6OC_CBItfYfNp-fP5LrdTKRl3sUvT5vdtFu8_r0noSkAGV4zAtsCmyupXd432R5pzTIGCeWwnAUpuXtG9aVtIQD1VgrEV2qA32zjDqiY31WpQaMJgxtXWd3lyayiwtDOIuKDM6oyH77lvkOKk5BkSYVCchxmIxlKYw6N4lba1PPZGhiEPJRC5uoDcinbU_KzmSolEMxKlThD3XsVMaUTJx2tBgYJDUc2c9R75ftyI6jRx73fTsv24f2ndxXKTMEQbiOfnAhUHXLdCwV9vyHB-Af_3iskf_t0eZOPMt0F2IIJu1hoDXX5i9QlDkqHhMFhovjEAemhuAJtOnZM8aTpIulCEyn0vRaSE5DapKBTqNOczfq0KPijIZGlejQyjkHG9J6mgdqUqxuAbWXi4E62YtlawoQX6TM2zIly2NKwwQyXUVlYY_R_ZH5Q0HBUD3Z20NDbxXUGjS80J80nCzX08D150Hgrb3A9YKlQ88VvHanrh8Eq_nKD-YLb7G8OfRXbTubzvz1yl3OZ_7SC9beauFQZNxItWseufqtu_0GguNzug?type=png)](https://mermaid.live/edit#pako:eNqVVF2P2jAQ_CuWnwMKJATIG7reqRJFQtX1oRVStI03xCWxI9tpS4H_3jghveaDSvVTdnZ2Zr2xfaGxZEhDiuodh6OC_CBItfYfNp-fP5LrdTKRl3sUvT5vdtFu8_r0noSkAGV4zAtsCmyupXd432R5pzTIGCeWwnAUpuXtG9aVtIQD1VgrEV2qA32zjDqiY31WpQaMJgxtXWd3lyayiwtDOIuKDM6oyH77lvkOKk5BkSYVCchxmIxlKYw6N4lba1PPZGhiEPJRC5uoDcinbU_KzmSolEMxKlThD3XsVMaUTJx2tBgYJDUc2c9R75ftyI6jRx73fTsv24f2ndxXKTMEQbiOfnAhUHXLdCwV9vyHB-Af_3iskf_t0eZOPMt0F2IIJu1hoDXX5i9QlDkqHhMFhovjEAemhuAJtOnZM8aTpIulCEyn0vRaSE5DapKBTqNOczfq0KPijIZGlejQyjkHG9J6mgdqUqxuAbWXi4E62YtlawoQX6TM2zIly2NKwwQyXUVlYY_R_ZH5Q0HBUD3Z20NDbxXUGjS80J80nCzX08D150Hgrb3A9YKlQ88VvHanrh8Eq_nKD-YLb7G8OfRXbTubzvz1yl3OZ_7SC9beauFQZNxItWseufqtu_0GguNzug)

## Modèle Dimensionnel
DimPlayer
nom
age

DimMap
nom

DimTeam
nom

DimMatch


DimDate
jourssemaine
joursmois
mois
annee
trimester
semestre

Fait_participationJoueur
fk_dim_player
fk_dim_match
fk_dim_team
fk_dim_map
fk_dim_date
stats

Fait_ParticipationTeam
fk_dim_match
fk_dim_team
fk_dim_map
fk_dim_date
stats

Table de Faits (fact_player_stats) : Le cœur du projet. Stocke les chiffres (Kills, Deaths, Rating). Chaque ligne est une performance unique d'un joueur dans un match.

Table de Faits (fact_matches) : Regroupe le contexte des matchs (Map, Date, Score final, Vainqueur).

Table de Dimension (dim_players) : Contient l'identité des joueurs (Pseudo, Pays). On ne stocke ces infos qu'une seule fois pour éviter de s'encombrer.

Si le schéma suivant ne fonctionne pas une capture d'écran est présente avec ce Readme.

[![](https://mermaid.ink/img/pako:eNqFU8tugzAQ_BW0ZxIBSQhwQ3moVRopanJphYSs4IIVbNBi1KY0_15eiVqZpnuxPLNj747XFRyziIIHFJeMxEh4ILQ6lo_bcPfkv6ye99rX12iUVdraXxx6LNwf_MNe87QA8pScKYYsCqBTtmlb_7B4WP0j5UQek145oK06rIlCIhOxdhVou43CRURSBQw5yRVQUsJDcxi2FBhpUaZyIL8nrD-qfGdCUOzIi-qp2tzNyKHuelIQrjZ5zEoh8fzrKtXwO26uN3eKGSBPLE0L9QEokYkKk6JghVRxJLJe-qJBhxhZBJ7EkurAKXLSbKGtOgCZ0LpxaIYmInhqBqbR5ES8Zhm_yjAr4wS8N5IW9a7Mm5Hoh_qWQkVEcdE4Bp5ptEeAV8EHeBPTHjvuzHKNuWNYhm3qcK7R6Xgyn7qOZbnmzLZd66LDZ3unMXZq4mfoQCMmM9x2P6r9WJdvxHYEhQ?type=png)](https://mermaid.live/edit#pako:eNqFU8tugzAQ_BW0ZxIBSQhwQ3moVRopanJphYSs4IIVbNBi1KY0_15eiVqZpnuxPLNj747XFRyziIIHFJeMxEh4ILQ6lo_bcPfkv6ye99rX12iUVdraXxx6LNwf_MNe87QA8pScKYYsCqBTtmlb_7B4WP0j5UQek145oK06rIlCIhOxdhVou43CRURSBQw5yRVQUsJDcxi2FBhpUaZyIL8nrD-qfGdCUOzIi-qp2tzNyKHuelIQrjZ5zEoh8fzrKtXwO26uN3eKGSBPLE0L9QEokYkKk6JghVRxJLJe-qJBhxhZBJ7EkurAKXLSbKGtOgCZ0LpxaIYmInhqBqbR5ES8Zhm_yjAr4wS8N5IW9a7Mm5Hoh_qWQkVEcdE4Bp5ptEeAV8EHeBPTHjvuzHKNuWNYhm3qcK7R6Xgyn7qOZbnmzLZd66LDZ3unMXZq4mfoQCMmM9x2P6r9WJdvxHYEhQ)