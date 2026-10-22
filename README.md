# Travaux Pratiques — Machine Learning Appliqué
### 3e Année DSI · Institut Supérieur des Études Technologiques

Ce dépôt regroupe l'ensemble des ateliers pratiques de Machine Learning Appliqué :

---

## 📂 Organisation du Dépôt

```text
├── guichet-ml/       # Atelier 1 — Inférence et API : Appeler un modèle et l'exposer derrière une API FastAPI
└── atelier2/         # Atelier 2 — Choisir un modèle sur preuves : Benchmark & sélection de LLMs (Groq)
```

---

## 🔹 Atelier 1 : Guichet ML — Inférence et API
- **Dossier** : [`guichet-ml/`](./guichet-ml/)
- **Objectif** : Développement d'une API FastAPI recevant un texte, identifiant sa langue et renvoyant une réponse normalisée (fournisseur simulé déterministe ou modèle Hugging Face distant).
- **Technologies** : Python, FastAPI, Pydantic, Pytest, Uvicorn, Hugging Face API.

---

## 🔹 Atelier 2 : Choisir un Modèle sur Preuves
- **Dossier** : [`atelier2/`](./atelier2/)
- **Objectif** : Évaluation rigoureuse et benchmark de 3 modèles de langage (`openai/gpt-oss-20b`, `openai/gpt-oss-120b`, `qwen/qwen3.8-27b`) sur 72 tickets clients avec comparaison des métriques (Accuracy, Macro-F1, matrice de confusion) et test d'une variante Few-shot.
- **Technologies** : Python, Groq API, métriques manuelles sans boîte noire, JSON proofs.
- **Résultats clés** : Le prompt engineering (Few-shot avec 4 exemples) surpasse l'impact de la taille du modèle en atteignant **0.970 de Macro-F1** (70/72 tickets corrects).
- **Livrables** : Code source, 4 fichiers de résultats JSON, [Fiche de décision](./atelier2/fiche_de_decision.md) et captures d'écran.
