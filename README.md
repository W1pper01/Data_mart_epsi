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

Table de Faits (fact_player_stats) : Le cœur du projet. Stocke les chiffres (Kills, Deaths, Rating). Chaque ligne est une performance unique d'un joueur dans un match.

Table de Faits (fact_matches) : Regroupe le contexte des matchs (Map, Date, Score final, Vainqueur).

Table de Dimension (dim_players) : Contient l'identité des joueurs (Pseudo, Pays). On ne stocke ces infos qu'une seule fois pour éviter de s'encombrer.

Si le schéma suivant ne fonctionne pas une capture d'écran est présente avec ce Readme.

[![](https://mermaid.ink/img/pako:eNqFU8tugzAQ_BW0ZxIBSQhwQ3moVRopanJphYSs4IIVbNBi1KY0_15eiVqZpnuxPLNj747XFRyziIIHFJeMxEh4ILQ6lo_bcPfkv6ye99rX12iUVdraXxx6LNwf_MNe87QA8pScKYYsCqBTtmlb_7B4WP0j5UQek145oK06rIlCIhOxdhVou43CRURSBQw5yRVQUsJDcxi2FBhpUaZyIL8nrD-qfGdCUOzIi-qp2tzNyKHuelIQrjZ5zEoh8fzrKtXwO26uN3eKGSBPLE0L9QEokYkKk6JghVRxJLJe-qJBhxhZBJ7EkurAKXLSbKGtOgCZ0LpxaIYmInhqBqbR5ES8Zhm_yjAr4wS8N5IW9a7Mm5Hoh_qWQkVEcdE4Bp5ptEeAV8EHeBPTHjvuzHKNuWNYhm3qcK7R6Xgyn7qOZbnmzLZd66LDZ3unMXZq4mfoQCMmM9x2P6r9WJdvxHYEhQ?type=png)](https://mermaid.live/edit#pako:eNqFU8tugzAQ_BW0ZxIBSQhwQ3moVRopanJphYSs4IIVbNBi1KY0_15eiVqZpnuxPLNj747XFRyziIIHFJeMxEh4ILQ6lo_bcPfkv6ye99rX12iUVdraXxx6LNwf_MNe87QA8pScKYYsCqBTtmlb_7B4WP0j5UQek145oK06rIlCIhOxdhVou43CRURSBQw5yRVQUsJDcxi2FBhpUaZyIL8nrD-qfGdCUOzIi-qp2tzNyKHuelIQrjZ5zEoh8fzrKtXwO26uN3eKGSBPLE0L9QEokYkKk6JghVRxJLJe-qJBhxhZBJ7EkurAKXLSbKGtOgCZ0LpxaIYmInhqBqbR5ES8Zhm_yjAr4wS8N5IW9a7Mm5Hoh_qWQkVEcdE4Bp5ptEeAV8EHeBPTHjvuzHKNuWNYhm3qcK7R6Xgyn7qOZbnmzLZd66LDZ3unMXZq4mfoQCMmM9x2P6r9WJdvxHYEhQ)