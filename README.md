# Analyse de Statistiques CS:GO (Projet ETL)

## Equipe
* **Garcia Robin**
* **Boutet Thomas**

---

## Jeu de Données (Etape 1)
* **Source :** https://www.kaggle.com/datasets/mateusdmachado/csgo-professional-matches
* **Contenu :** Environ 50 000 matchs compétitifs.
* **Caracteristiques :** 4 fichiers CSV (results.csv, players.csv, picks.csv, economy.csv) contenant des statistiques in-game détaillées.

---

## Architecture Technique (Etape 2)

| Composant | Technologie | Rôle |
| :--- | :--- | :--- |
| **Extract/Load** | PostgreSQL 15 | Stockage des données brutes (RAW) et transformées. |
| **Transformation** | Python (Pandas & SQLAlchemy) | Nettoyage, typage et normalisation des donnees. |
| **Visualisation** | Metabase | Creation de dashboards analytiques. |
| **Orchestration** | Docker Compose | Conteneurisation de toute l'infrastructure. |

---

## Installation et Lancement

### 1. Prérequis
* Docker Desktop et Python 3.10+
* Extraire les fichiers CSV dans le dossier `./data/`

### 2. Démarrage
```bash
# Lancer l'infrastructure (Base de donnees + Metabase)
docker-compose up -d
```

### 3. Execution de l'ETL
Ouvrir et executer le notebook : ETL.ipynb. 
Ce script automatise l'extraction/importation (Extract-Load) et la restructuration des donnees (Transform).


## Modèle relationnel (Etape 3)
### Schéma écrit
country\
id_country(pk)
name(uk)
player\
id_player(pk)
id_country(fk)
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

### Schéma Mermaid
[![](https://mermaid.ink/img/pako:eNqVVMGO2jAQ_RXL5yyCZANJbojuqhKlRSv2sBVSNI0d4pLYke20pcC_b5zAQoipVJ_iN2_em5nY3uNEEIojTOUnBhsJxZqjei2_TN-eXtDh8PAg9mj27fXr6uUNRQjKEqRmlGsbr93Fq6fpIl5MV7PPdUbDT1hJ2wQTO9M7vJ-iOlFaxMZJBL_yXkyXLeuAzoQ1VrRRQqqSa3yxjDuitjrrVA1aIULPeeeu9-3WLMY1YiRORMW13KHl_BL6BTLJQCIOxamPY2dEfZUyhx2VVpE2FF-0LObP8w-bq9H2bTSFwmpiAo0Fer2VMqPtKxVQWoVq_K6OGa5NSSdZR4uApqiBY_Np9bZ2HN_zOPXtPM_v2ndiP4TIKXDEVPybcU5lN00lQtIb__45-sdfthXyvzWa2JbluepChILObjBQiil9BfKqoJIlSIJmfNPHgcg-uAWlb-wJYWnaxTIKRGVC35SQbvvUNAeVxZ3ijtjBG8kIjrSsqINr5wLMFjfTXGOd0foeYHNHCcituZ8mpwT-XYjinCZFtclwlEKu6l1VmmN0etM-KJQTKmfmAuHIc_1GA0d7_AdH7nA8mIShPwrC0B36nh84eIej8cCbDB-HjxPXCzxvHPhHB_9tXIeDMBiPatANR8EkmLgjB1PCtJCL9kltXtbjO09jlAU?type=png)](https://mermaid.live/edit#pako:eNqVVMGO2jAQ_RXL5yyCZANJbojuqhKlRSv2sBVSNI0d4pLYke20pcC_b5zAQoipVJ_iN2_em5nY3uNEEIojTOUnBhsJxZqjei2_TN-eXtDh8PAg9mj27fXr6uUNRQjKEqRmlGsbr93Fq6fpIl5MV7PPdUbDT1hJ2wQTO9M7vJ-iOlFaxMZJBL_yXkyXLeuAzoQ1VrRRQqqSa3yxjDuitjrrVA1aIULPeeeu9-3WLMY1YiRORMW13KHl_BL6BTLJQCIOxamPY2dEfZUyhx2VVpE2FF-0LObP8w-bq9H2bTSFwmpiAo0Fer2VMqPtKxVQWoVq_K6OGa5NSSdZR4uApqiBY_Np9bZ2HN_zOPXtPM_v2ndiP4TIKXDEVPybcU5lN00lQtIb__45-sdfthXyvzWa2JbluepChILObjBQiil9BfKqoJIlSIJmfNPHgcg-uAWlb-wJYWnaxTIKRGVC35SQbvvUNAeVxZ3ijtjBG8kIjrSsqINr5wLMFjfTXGOd0foeYHNHCcituZ8mpwT-XYjinCZFtclwlEKu6l1VmmN0etM-KJQTKmfmAuHIc_1GA0d7_AdH7nA8mIShPwrC0B36nh84eIej8cCbDB-HjxPXCzxvHPhHB_9tXIeDMBiPatANR8EkmLgjB1PCtJCL9kltXtbjO09jlAU)

## Modèle Dimensionnel
DimCoutry
nom

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
