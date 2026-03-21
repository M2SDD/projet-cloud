# Analyse des comportements d'achat sur le dataset Black Friday avec Azure

Projet **Cloud** — Master 2 Science des Données (M2SDD)

---

## Objectif

Ce projet met en œuvre une chaîne complète de traitement de données dans l'écosystème **Microsoft Azure**, en partant d'un dataset public de transactions Black Friday jusqu'à la visualisation des insights dans **Power BI**. Il explore plusieurs services cloud : stockage blob, IA générative (Azure OpenAI), et analyse de texte (Azure Language / Text Analytics).

---

## Architecture du projet

```
Kaggle (dataset)
    └── Azure Blob Storage        ← ingestion & stockage
            └── Azure OpenAI      ← génération d'avis clients (GPT)
            └── Azure Text Analytics ← analyse de sentiment & opinions
                    └── Power BI  ← visualisation & publication
```

---

## Étapes

### 1. Exploration et ingestion (`1-exploration_ingestion.ipynb`)

- Téléchargement du dataset **Black Friday** (~550 000 lignes) via l'API Kaggle
- Exploration de la structure, types de variables, distributions
- Nettoyage et préparation des données
- Téléversement vers **Azure Blob Storage**

Screenshots : [pictures/1-ingestion/](pictures/1-ingestion/)

---

### 2. Enrichissement par IA Azure (`2-azure_ai.ipynb`)

Pour enrichir un dataset purement transactionnel avec de la donnée textuelle :

- Définition de **personas clients** et de descriptions de catégories de produits
- Génération d'**avis clients synthétiques** via **Azure OpenAI** (GPT)
- Analyse des avis générés avec **Azure Text Analytics** :
  - Analyse de sentiment (positif / négatif / neutre)
  - Extraction de mots-clés
- Export des données enrichies vers Blob Storage

> Stratégie adoptée : travailler sur des données agrégées (quelques dizaines de lignes) plutôt que sur l'intégralité du dataset afin de préserver les crédits API.

Screenshots : [pictures/2-azure_ai/](pictures/2-azure_ai/)

---

### 3. Visualisation Power BI (`3-dataviz.pbix`)

Tableau de bord en 3 pages connecté directement à Azure Blob Storage :

- **Page 1** — Vue générale des achats Black Friday (montants, segments, démographie)
- **Page 2** — Analyse par catégorie de produits
- **Page 3** — Insights IA : sentiments et mots-clés extraits des avis générés

Screenshots : [pictures/3-power_bi/](pictures/3-power_bi/)

---

## Structure du dépôt

```
ProjetCloud/
├── 1-exploration_ingestion.ipynb   # Exploration du dataset et ingestion Azure
├── 2-azure_ai.ipynb                # Enrichissement via Azure OpenAI & Text Analytics
├── 3-dataviz.pbix                  # Dashboard Power BI
├── data/
│   ├── blackfriday_clean.csv       # Dataset nettoyé (~550 000 lignes)
│   ├── reviews.csv                 # Avis générés par GPT
│   ├── reviews_enriched.csv        # Avis enrichis (sentiment, opinions)
│   ├── personas.json               # Profils clients utilisés pour la génération
│   ├── category_descriptions.json  # Descriptions des catégories produits
│   └── dict_encoding/decoding.*    # Mappings de décodage des variables encodées
├── pictures/
│   ├── 1-ingestion/                # Screenshots Azure Storage & téléversement
│   ├── 2-azure_ai/                 # Screenshots Azure OpenAI & Language
│   └── 3-power_bi/                 # Screenshots du dashboard Power BI
├── requirements.txt
├── .env.example                    # Template pour les variables d'environnement
└── Projet_data_M2_BDD.pdf          # Énoncé du projet
```

---

## Installation

### Prérequis

- Python 3.10+ (version utilisée 3.12.10)
- Un compte Azure avec les ressources suivantes provisionnées :
  - Azure Blob Storage (compte de stockage + conteneur)
  - Azure OpenAI (déploiement d'un modèle GPT)
  - Azure Language / Text Analytics
- Un compte Kaggle (pour le téléchargement du dataset)
- Power BI Desktop (pour ouvrir le fichier `.pbix`)

*Plus d'informations dans les notebooks.*

### Mise en place

```bash
# Cloner le dépôt
git clone <url-du-repo>
cd ProjetCloud

# Créer l'environnement virtuel
python -m venv .venv
source .venv/bin/activate  # ou .venv\Scripts\activate sous Windows

# Installer les dépendances
pip install -r requirements.txt

# Configurer les variables d'environnement
cp .env.example .env
# Remplir .env avec vos clés Azure et Kaggle
```

### Variables d'environnement (`.env`)

Se référer au fichier [.env.example](.env.example) pour la liste complète des variables nécessaires (clés Azure, endpoint OpenAI, connection string Blob Storage, credentials Kaggle).

*Plus d'informations dans les notebooks.*

---

## Services Azure utilisés

| Service | Usage |
|---|---|
| **Azure Blob Storage** | Stockage du dataset et des fichiers enrichis |
| **Azure OpenAI** | Génération d'avis clients synthétiques (GPT) |
| **Azure Text Analytics** | Analyse de sentiment et opinion mining |
| **Power BI** | Visualisation connectée au Blob Storage |

---

## Dataset

**Black Friday Sales** — disponible sur [Kaggle](https://www.kaggle.com/datasets/sdolezel/black-friday)

- ~550 000 transactions
- Variables :
    - `User_ID` : ID utilisateur
    - `Product_ID` : ID produit
    - `Gender` : sexe de l'utilisateur
    - `Age` : tranches d'âges
    - `Occupation` : occupation de l'utilisateur (anonymisé)
    - `City_Category` : catégorie de la ville (A, B, C)
    - `Stay_In_Current_City_Years` : nombre d'années passées dans la ville actuelle
    - `Marital_Status` : statut marital
    - `Product_Category_1` : catégorie principale du produit (anonymisé)
    - `Product_Category_2` : 2ème catégorie du produit (anonymisé)
    - `Product_Category_3` : 3ème catégorie du produit (anonymisé)
    - `Purchase` : Montant achat (pas d'information sur la devise monétaire)

*NB : un produit peut avoir plusieurs catégories*

