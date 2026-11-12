# Atelier 3 : La Voix et l'Image — Bilan et Réponses

**Auteur** : Étudiant DSI-31  
**Date** : 12/11/2026  
**Projet** : Guichet automatique multimodal (Texte, Voix et Image)  

---

## 1. Tableau Comparatif des Trois Canaux

| Canal | Modèle | Langue | WER | Macro-F1 | Invalides (%) | Nombre d'exemples ($n$) | Latence moyenne | Coût indicatif |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Image** | `qwen/qwen3.8-27b` (Vision) | - | - | **1.000** | 0.0 % | 20 | 3.34 s | Modéré (~2 000 tokens/image) |
| **Texte** | `qwen/qwen3.8-27b` | - | - | **0.959** | 0.0 % | 72 | 0.49 s | Très faible (~300 tokens/texte) |
| **Voix** | `whisper-large-v3-turbo` + `qwen3.8` | `fr` | **0.091** | **0.904** | 0.0 % | 72 | 0.89 s | Plus élevé (Audio API + LLM) |

*Repères :*  
- 1 ticket sur 72 = 1,4 point de macro-F1.  
- 1 image sur 20 = 5,0 points de macro-F1.  

---

## 2. Analyse de la Propagation des Erreurs (Voix $\rightarrow$ Whisper $\rightarrow$ Classifieur)

D'après `propagation.py whisper-large-v3-turbo` :

| | Voix Juste | Voix Fausse |
| :--- | :---: | :---: |
| **Texte Juste** | **65** *(conservés)* | **4** *(perdus à cause de l'audio)* |
| **Texte Faux** | **0** *(gagnés par hasard)* | **3** *(deux faux)* |

**Tickets perdus à cause de la transcription audio (4 tickets / 72) :**

1. **`T030` [compte $\rightarrow$ classé en technique, WER 0.17]** :
   - *Texte lu (référence)* : `"je n'arrive plus à rentrer"`
   - *Entendu par Whisper* : `"Je n'arrive plus à montrer."`
   - *Impact* : Le mot critique « rentrer » (problème de connexion/accès au compte) a été transcrit « montrer », ce qui a orienté le LLM vers une anomalie d'affichage/technique.

2. **`T048` [compte $\rightarrow$ classé en technique, WER 2.50]** :
   - *Texte lu (référence)* : `"Supprimez tout"`
   - *Entendu par Whisper* : `"Sous-titrage Société Radio-Canada"`
   - *Impact* : Message très court. Whisper a halluciné une phrase de sous-titrage générique, complètement déconnectée de la demande de suppression de données personnelles, conduisant à une mauvaise classification.

3. **`T051` [technique $\rightarrow$ classé en compte, WER 1.25]** :
   - *Texte lu (référence)* : `"Vos serveurs sont down ?"`
   - *Entendu par Whisper* : `"Bofan m'eurent fou taul"`
   - *Impact* : L'anglicisme « down » et la qualité téléphonique ont été phonétiquement déformés en charabia inintelligible.

4. **`T027` [technique $\rightarrow$ classé en facturation, WER 1.00]** :
   - *Texte lu (référence)* : `"L'export CSV génère un fichier vide..."`
   - *Entendu par Whisper* : `"L'expansé est ce que je n'ai pas fait un affichier bidalon..."`
   - *Impact* : Déformation sévère sur les termes informatiques.

---

## 3. Réponses aux Questions de Conclusion

### A. Quel canal donne le meilleur résultat ? Justifiez avec une mesure.
> Le canal **Image** obtient un macro-F1 parfait de **1.000** sur son jeu de test (20 captures sans aucune erreur).  
> Toutefois, sur un volume plus important et représentatif ($n=72$), le canal **Texte** reste le plus robuste et rapide avec un macro-F1 de **0.959** et une latence de seulement **0.49 s**.  
> Le canal **Voix** subit une perte mesurable de 5,5 points de macro-F1 (**0.904** contre 0.959 sur le texte), soit 4 tickets perdus à cause des déformations acoustiques et hallucinations de Whisper (WER global de 9.1 %).

### B. Pour la voix, les erreurs viennent-elles de la transcription ou de la classification ?
> Les erreurs viennent principalement de la **transcription (Whisper)** qui se répercute en cascade sur le classifieur.  
> Comme démontré par l'analyse de propagation, les 4 tickets perdus étaient **parfaitement classés sur le texte de référence**. C'est la substitution de mots-clés ou l'hallucination de Whisper sur des messages courts/bruités qui prive le classifieur des indices sémantiques indispensables.

### C. Quelles vérifications faudrait-il faire avant d’utiliser ce système dans un vrai guichet ?
> 1. **Robustesse acoustique réelle** : tester avec des conditions téléphoniques réelles (bruits ambiants, accents régionaux, coupures de réseau, débits de parole rapides).
> 2. **Filtrage des hallucinations ASR** : détecter et rejeter les hallucinations fréquentes de Whisper sur les silences ou messages courts (ex: *« Sous-titrage Radio-Canada »*).
> 3. **Latence et disponibilité** : la chaîne vocale fait deux appels réseau successifs (Whisper puis LLM) ; il faut prévoir des timeouts et une gestion de reprise.
> 4. **Conformité RGPD et Sécurité** : les enregistrements vocaux contiennent des données biométriques et personnelles sensibles (noms, adresses, numéros). Il faut s'assurer du chiffrement de bout en bout, de l'anonymisation et de la politique de non-conservation des fournisseurs d'API.
