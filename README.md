# Guichet ML — Atelier 01 : Appeler un modèle et l'exposer derrière une API

Première brique du Guichet ML : une API FastAPI qui reçoit un texte, identifie sa langue et renvoie une réponse normalisée (fournisseur simulé déterministe ou modèle Hugging Face distant).

---

## Prérequis

- Python 3.11 ou version ultérieure
- Git

---

## Installation et démarrage

### 1. Cloner le projet et créer l'environnement virtuel

```bash
# Sous Windows PowerShell :
py -m venv .venv
.venv\Scripts\Activate.ps1
# Si l'activation est refusée :
# Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

# Sous Linux ou macOS :
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Installer les dépendances

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Configurer l'environnement

Créer le fichier `.env` à partir de `.env.example` :

```bash
# Windows PowerShell :
Copy-Item .env.example .env

# Linux / macOS :
cp .env.example .env
```

Par défaut, `INFERENCE_MODE=mock`, ce qui permet d'exécuter l'application sans compte ni connexion Internet.

### 4. Démarrer le serveur API

```bash
uvicorn app.main:app --reload
```

Le serveur est disponible sur :
- **Documentation Swagger interactive** : [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- **Point de contrôle de santé** : [http://127.0.0.1:8000/health](http://127.0.0.1:8000/health)

---

## Exécuter les tests

Les tests s'exécutent hors ligne avec :

```bash
pytest -q
```

---

## Structure du projet

```text
guichet-ml/
├── app/
│   ├── __init__.py
│   ├── config.py       # Chargement des variables d'environnement
│   ├── main.py         # Application FastAPI et routes (/health, /detect-language)
│   ├── providers.py    # Fournisseur simulé (MockProvider) et Hugging Face (HfProvider)
│   └── schemas.py      # Contrats de données Pydantic (Request, Response, Prediction)
├── tests/
│   └── test_api.py     # Tests unitaires automatisés
├── .env.example        # Modèle des variables d'environnement (versionné)
├── .gitignore          # Fichiers et dossiers ignorés (.venv, .env, __pycache__)
├── requirements.txt    # Dépendances aux versions figées
└── README.md           # Instructions d'installation et d'exécution
```
