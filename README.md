# Assistant de comptes rendus — Basket Club du Gier

> Projet IA — BTS SIO 2 SLAM — [Prénom NOM] — octobre 2026
> **URL publique** : à compléter mardi — code d'accès envoyé à l'enseignant par e-mail

## 1. Concevoir

**Sujet choisi** : 3 — Compte rendu → décisions et actions, Basket Club du Gier

**L'organisation et son besoin :**

Le Basket Club du Gier souhaite faciliter l'analyse des comptes rendus de ses réunions.
Aujourd'hui, les informations importantes comme les décisions et les actions à réaliser peuvent être difficiles à retrouver dans des notes de réunion.
L'application utilise une IA pour extraire automatiquement les décisions, les actions, les personnes responsables, les dates prévues et les éléments restant en suspens.

**Trois cas d'usage :**

1. En tant que membre du Basket Club du Gier, je veux analyser les notes d'une réunion, afin d'identifier les décisions prises.

2. En tant que responsable du club, je veux connaître les actions à réaliser, les personnes responsables et les dates prévues, afin de suivre les tâches après une réunion.

3. En tant que membre du club, je veux identifier les éléments qui restent en suspens, afin de savoir quels sujets doivent encore être traités.

**Ce que l'application ne fait pas :**

- L'application n'invente pas de décision, d'action, de personne responsable ou de date qui ne figurent pas dans les notes fournies.
- L'application ne remplace pas la vérification humaine du compte rendu.
- L'application peut produire une erreur si les notes de réunion sont ambiguës, incomplètes ou difficiles à interpréter.

## 2. Le modèle et la machine

| | |
|---|---|
| Carte graphique et mémoire vidéo (VRAM) | Aucune carte graphique dédiée — GPU intégré à la puce Apple M4 |
| Mémoire vive | 16 Go |
| Modèle retenu | `qwen2.5:1.5b` |
| Pourquoi celui-là | Le modèle correspond à la configuration de la machine selon les consignes du projet. Il a été testé localement avec Ollama. |
| Modèle comparé | À compléter mardi |

## 3. Piloter — le journal

| Séance | Ce qui est fait | Ce qui a bloqué, et comment c'est réglé |
|---|---|---|
| Lundi 05/10 | Choix et test du modèle `qwen2.5:1.5b` avec Ollama. Création du dépôt GitHub. Conception du prompt dans `prompt.txt`. Mise en place et test de l'application en local. Création d'au moins 10 cas dans `cas.json`, dont des cas hors sujet et une tentative de détournement des consignes. Première évaluation avec `evaluer.py`. | … |
| Mardi 06/10 | À compléter | À compléter |

## 4. Mesurer

Jeu de **10 cas au moins** dans `cas.json`, dont au moins deux hors sujet et un qui tente de détourner les consignes.

| | Modèle retenu | Modèle comparé |
|---|---|---|
| Réussite (sur N cas × 3 essais) | À compléter mardi | À compléter mardi |
| Temps de réponse médian | À compléter mardi | À compléter mardi |

**Ce que les échecs montrent** :

À compléter mardi après les tests.

## 5. Sécuriser

| Risque | Ce qui pourrait arriver | Mesure prise dans le projet |
|---|---|---|
| L'URL est publique | N'importe qui pourrait utiliser l'application et les ressources de la machine. | Code d'accès et limite de requêtes. |
| Ollama exposé | Une personne pourrait accéder directement au serveur Ollama. | Ollama n'est pas exposé publiquement. |
| Détournement des consignes | Un utilisateur pourrait essayer de faire ignorer les instructions du modèle. | Le prompt doit demander au modèle de respecter les consignes et de refuser les demandes qui tentent de les détourner. |
| Données personnelles | Des informations personnelles pourraient être transmises au modèle. | Utiliser uniquement les données nécessaires et éviter les données personnelles réelles. |
| Secrets dans le dépôt | Le code d'accès pourrait être récupéré sur GitHub. | Le code d'accès est stocké dans `.env` et le fichier contenant le secret n'est pas envoyé sur GitHub. |

## 6. Mettre en production — comment refaire

```bash
cp .env.example .env
# puis remplir les variables nécessaires

docker compose up -d --build
docker compose ps
docker compose logs tunnel
```
## 7. Usage de l'IA pendant le projet

Ce que vous avez demandé à un assistant, et ce que vous avez gardé, modifié ou refusé.
