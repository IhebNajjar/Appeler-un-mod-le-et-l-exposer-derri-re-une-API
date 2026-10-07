# Réponses au Quiz de sortie — Atelier 01

### 1. Quelle est la différence entre entraîner un modèle et faire de l'inférence ?
L'entraînement consiste à ajuster et optimiser les paramètres (poids) d'un modèle à partir d'un jeu de données (processus long, coûteux, nécessitant des GPU). L'inférence consiste à exécuter un modèle déjà entraîné sur une nouvelle donnée d'entrée pour obtenir une prédiction (rapide et peu coûteux par requête).

### 2. Un score de 0,98 garantit-il que la prédiction est correcte ?
Non. Un score élevé reflète uniquement le niveau de confiance statistique interne du modèle par rapport à ses données d'apprentissage. Le modèle peut avoir une confiance très élevée tout en faisant une prédiction fausse (ex. données hors domaine, surapprentissage ou hallucinations).

### 3. Pourquoi l'API ne renvoie-t-elle pas directement la réponse du fournisseur ?
Pour découpler l'application cliente de l'implémentation du fournisseur externe. Cela permet de normaliser le format de réponse, d'ajouter des métadonnées métiers (`latency_ms`, `requires_review`), de changer de fournisseur ou de version de modèle sans impacter les clients, et d'éviter toute fuite d'informations sensibles.

### 4. Pourquoi une clé d'API ne doit-elle jamais se trouver dans le code du navigateur ?
Tout code envoyé au navigateur (ex. JavaScript) est public, lisible et inspectable par n'importe quel utilisateur. Placer une clé côté client expose le compte à des vols, des abus et à l'épuisement de ses quotas/crédits.

### 5. Que doit faire l'application quand le meilleur score est inférieur au seuil ?
L'application doit positionner le champ `requires_review` à `true` afin d'avertir le système et orienter la requête vers une vérification ou une intervention humaine.

### 6. À quoi sert exactement le mode MOCK, et que ne prouve-t-il pas ?
Le mode MOCK sert à développer, tester et valider l'architecture et les contrats de données hors ligne, de façon rapide, reproductible et déterministe, sans dépendre du réseau ou de quotas. Il ne prouve en aucun cas la qualité, la pertinence ou la précision réelle du modèle de machine learning.
