# Travaux Pratiques — Machine Learning Appliqué
### 3e Année DSI · Institut Supérieur des Études Technologiques

Ce dépôt regroupe l'ensemble des ateliers pratiques de Machine Learning Appliqué :

---

## 📂 Organisation du Dépôt

```text
├── guichet-ml/       # Atelier 1 — Inférence et API : FastAPI & détection de langue
├── atelier2/         # Atelier 2 — Choisir un modèle sur preuves : Benchmark & sélection de LLMs (Groq)
└── atelier3/         # Atelier 3 — La voix et l'image : Whisper (Audio), Vision (Qwen 3.8) et comparaison multimodale
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
- **Livrables** : Code source, 4 fichiers de résultats JSON, [Fiche de décision](./atelier2/fiche_de_decision.md).

---

## 🔹 Atelier 3 : La Voix et l'Image (Multimodal)
- **Dossier** : [`atelier3/`](./atelier3/)
- **Objectif** : Extension du guichet aux canaux audio (messages vocaux `.mp3`) et visuel (captures d'écran `.png`).
- **Chaîne vocale** : Audio $\rightarrow$ Whisper ASR (mesuré par le WER) $\rightarrow$ Classifieur LLM (mesuré par le Macro-F1).
- **Chaîne image** : Capture $\rightarrow$ Modèle de vision `qwen/qwen3.8-27b` end-to-end.
- **Résultats obtenus** :
  - **Image** : Macro-F1 = 1.000 ($n=20$)
  - **Texte** : Macro-F1 = 0.959 ($n=72$)
  - **Voix** : WER = 0.091, Macro-F1 = 0.904 ($n=72$)
- **Livrables** : Scripts complétés (`transcription.py`, `wer.py`, `propagation.py`, `vision.py`), preuves JSON dans `resultats/`, et [Bilan & Réponses](./atelier3/reponses_bilan.md).
