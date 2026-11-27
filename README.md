# AI Finance Sentiment Tracker

### Quand les marchés bougent, les informations parlent. Ce dashboard les écoute.

**AI Finance Sentiment Tracker** transforme les actualités financières en signaux lisibles.
Le projet croise les cours de bourse, le sentiment du marché analysé par **FinBERT** et des indicateurs techniques dans une interface Streamlit sombre, rapide et interactive.

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-FF4B4B?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![FinBERT](https://img.shields.io/badge/Hugging%20Face-FinBERT-FFD21E?logo=huggingface&logoColor=black)](https://huggingface.co/ProsusAI/finbert)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

---

## Ce que vous pouvez découvrir

- **Lire le marché au-delà des chiffres** : chaque titre est classé comme positif, négatif ou neutre par FinBERT.
- **Relier informations et prix** : suivre l'évolution du sentiment autour d'une action et la comparer à son cours.
- **Analyser avec les bons indicateurs** : SMA 20/50, RSI 14, bandes de Bollinger 20-2 et MACD 12-26-9.
- **Obtenir une synthèse en langage naturel** : activer Claude pour transformer les données du ticker en commentaire de marché.
- **Gagner du temps** : les données de prix et d'actualités sont mises en cache afin de limiter les appels externes.
- **Personnaliser votre cockpit** : gérer une liste de favoris, filtrer les articles, sélectionner une période et forcer une actualisation.

> [!IMPORTANT]
> Les signaux affichés sont une aide à l'exploration et **ne constituent pas des conseils financiers**. Ils peuvent être incomplets, retardés ou erronés.

---

## Le pipeline en une image

```text
┌────────────────────┐
│  Yahoo Finance RSS │
│  Google News RSS   │──┐
│  yFinance News     │  │
└────────────────────┘  │
                        ▼
               ┌─────────────────┐
               │  Ingestion news │
               │  + déduplication │
               └────────┬────────┘
                        ▼
               ┌─────────────────┐
               │     FinBERT     │
               │  NLP financier  │
               └────────┬────────┘
                        ├───────────────┐
                        ▼               ▼
┌─────────────────┐  Scores         Prix historiques
│   Cache CSV     │  positifs/      via yFinance
│   24 h / 6 h    │  négatifs/neutres
└─────────────────┘        │               │
                           └───────┬───────┘
                                   ▼
                       ┌─────────────────────┐
                       │ Dashboard Streamlit │
                       │ Prix · Sentiment    │
                       │ RSI · MACD · Claude │
                       └─────────────────────┘
```

---

## Une interface pensée pour décider plus vite

| Module | Ce qu'il apporte |
|---|---|
| **Recherche intelligente** | Sélection d'un ticker populaire ou saisie libre avec validation |
| **Synthèse de marché** | Signal FinBERT + RSI, variation du cours et indicateurs clés |
| **Graphique de prix** | Courbes interactives avec moyennes mobiles, Bollinger et volume |
| **Analyse technique** | RSI et MACD affichés en panneaux optionnels |
| **Détails sentimentaux** | Score positif/négatif/neutre, jauges, répartition et tendance |
| **Fil d'actualités** | Articles filtrables avec source, date, lien et niveau de confiance |
| **Corrélation glissante** | Relation entre score de sentiment journalier et rendement du titre |
| **Résumé Claude** | Synthèse optionnelle fondée sur les résultats calculés dans l'application |

---

## Stack technique

- **Interface** : Streamlit
- **Visualisation** : Plotly
- **Données financières** : yFinance
- **Actualités** : Yahoo Finance RSS, Google News RSS, yFinance News
- **Modèle NLP** : FinBERT via Hugging Face Transformers
- **Calcul** : Pandas, NumPy, PyTorch
- **Synthèse optionnelle** : Anthropic Claude
- **Tests** : pytest

---

## Installation rapide

### Prérequis

- Python 3.10 ou supérieur
- `pip`
- Une connexion internet pour yFinance, les flux RSS et le téléchargement initial de FinBERT

### 1. Cloner le projet

```bash
git clone https://github.com/riim00/AI-Finance-Sentiment-Tracker.git
cd AI-Finance-Sentiment-Tracker
```

### 2. Créer un environnement virtuel

**macOS / Linux**

```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows**

```powershell
python -m venv venv
venv\Scripts\activate
```

### 3. Installer les dépendances

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Au premier lancement de l'analyse, Hugging Face télécharge automatiquement le modèle [ProsusAI/finbert](https://huggingface.co/ProsusAI/finbert).

---

## Lancer l'application

```bash
streamlit run app.py
```

Ouvrez ensuite votre navigateur à l'adresse :

```text
http://localhost:8501
```

---

## Configuration optionnelle : résumé Claude

Le résumé IA est désactivé par défaut. Pour l'activer, exportez une clé Anthropic avant de lancer Streamlit :

**macOS / Linux**

```bash
export ANTHROPIC_API_KEY="votre_clé"
streamlit run app.py
```

**Windows PowerShell**

```powershell
$env:ANTHROPIC_API_KEY="votre_clé"
streamlit run app.py
```

Sans clé, l'application reste pleinement utilisable : seule la synthèse Claude est masquée.

---

## Mode d'emploi

1. **Choisissez un ticker** dans la barre latérale ou saisissez-le manuellement.
2. **Définissez la période** souhaitée.
3. **Sélectionnez les indicateurs techniques** à afficher.
4. **Lancez l'analyse** : les cours, les actualités et les scores sont chargés.
5. **Explorez les vues** : prix, RSI, MACD, répartition du sentiment, actualités et corrélation.
6. **Ajoutez vos actions favorites** pour les retrouver rapidement dans la session.
7. **Utilisez le rafraîchissement forcé** lorsque vous avez besoin d'ignorer les fichiers locaux.

### Politique de cache

| Données | Durée de fraîcheur |
|---|---|
| Prix | 24 heures |
| Actualités | 6 heures |
| Prix affichés en temps réel | 60 secondes |

Le cache est enregistré dans `data/` sous forme de fichiers CSV. Utilisez l'option **Forcer le rafraîchissement** dans la barre latérale pour récupérer de nouvelles données.

---

## Tests

Les tests couvrent la récupération des cours et des actualités ainsi que le traitement de sentiment simulé.

```bash
pytest tests/ -v
```

---

## Structure du projet

```text
AI-Finance-Sentiment-Tracker/
├── app.py                  # Application Streamlit
├── src/
│   ├── ingestion.py        # Prix yFinance, flux RSS et cache CSV
│   └── processing.py       # Analyse NLP avec FinBERT
├── scripts/                # Utilitaires supplémentaires
├── tests/
│   ├── test_ingestion.py
│   └── test_processing.py
├── data/                   # Cache et résultats générés automatiquement
├── notebooks/              # Analyses exploratoires
├── requirements.txt
└── README.md
```

---

## Contribution

Les contributions sont les bienvenues :

1. Forkez le dépôt.
2. Créez une branche : `git checkout -b feature/ma-fonctionnalite`
3. Commitez vos changements : `git commit -m "Ajout de ma fonctionnalité"`
4. Poussez la branche : `git push origin feature/ma-fonctionnalite`
5. Ouvrez une Pull Request.

Avant de contribuer, lancez les tests et vérifiez que les nouvelles fonctionnalités ne ralentissent pas le chargement du modèle.

---

## Licence

Distribué sous licence MIT.
