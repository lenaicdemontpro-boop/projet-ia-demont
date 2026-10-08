# Assistant de comptes rendus — Basket Club du Gier

> Projet IA — BTS SIO 2 SLAM — Lénaïc Demont — octobre 2026

> **URL publique :** https://citation-foo-watching-patches.trycloudflare.com

## 1. Concevoir

### Sujet choisi

**Sujet 3 — Compte rendu → décisions et actions**

### Organisation et besoin

Le Basket Club du Gier souhaite faciliter l'analyse des comptes rendus de ses réunions.

Aujourd'hui, les informations importantes comme les décisions, les actions à réaliser, les responsables et les échéances peuvent être difficiles à retrouver dans des notes de réunion.

L'application utilise une IA afin d'extraire automatiquement les informations importantes d'un compte rendu.

### Trois cas d'usage

1. En tant que membre du Basket Club du Gier, je veux analyser les notes d'une réunion afin d'identifier les décisions et les actions importantes.

2. En tant que responsable du club, je veux connaître les actions à réaliser, les personnes responsables et les dates prévues afin de suivre les tâches.

3. En tant que membre du club, je veux identifier les éléments qui restent en suspens afin de savoir quels sujets doivent encore être traités.

### Ce que l'application ne fait pas

* Elle ne remplace pas la validation humaine des décisions et actions extraites.
* Elle ne prend pas automatiquement de décisions à la place des responsables du club.
* Elle ne garantit pas l'exactitude parfaite des dates, responsables ou interprétations produites par le modèle.
* Elle ne doit pas être utilisée comme seule source pour prendre une décision importante.

---

## 2. Architecture technique

L'application est composée de plusieurs éléments :

* **Application web :** Python / FastAPI
* **Modèle de langage :** Ollama
* **Modèles testés :**

  * `qwen2.5:1.5b`
  * `qwen2.5:3b`
* **Conteneurisation :** Docker
* **Publication :** Cloudflare Tunnel
* **Évaluation :** `evaluer.py`
* **Configuration :** `.env`

Le modèle de langage fonctionne localement sur la machine.

L'application communique avec Ollama afin d'envoyer les demandes au modèle.

---

## 3. Déploiement

L'application est lancée avec Docker Compose.

Les services utilisés sont :

* `app` : application web
* `tunnel` : Cloudflare Tunnel permettant l'accès HTTPS public

L'application est accessible à l'adresse :

https://citation-foo-watching-patches.trycloudflare.com

Le tunnel permet de publier l'application sans exposer directement le service Ollama.

Le port Ollama `11434` n'est pas exposé publiquement.

---

## 4. Mesures et comparaison

Deux modèles ont été évalués avec le même jeu de tests.

### Modèle `qwen2.5:1.5b`

Fichier de résultats :

`resultats-20261008-1550.csv`

Résultat :

**10 réussites sur 30, soit 33 %.**

Temps médian :

**1,1 seconde**

Temps minimum :

**0,3 seconde**

Temps maximum :

**1,9 seconde**

### Modèle `qwen2.5:3b`

Fichier de résultats :

`resultats-20261008-1557.csv`

Résultat :

**19 réussites sur 30, soit 63 %.**

Temps médian :

**1,7 seconde**

Temps minimum :

**0,4 seconde**

Temps maximum :

**6,4 secondes**

### Comparaison

| Modèle         |         Réussite | Temps médian | Temps min. | Temps max. |
| -------------- | ---------------: | -----------: | ---------: | ---------: |
| `qwen2.5:1.5b` | 10/30 — **33 %** |        1,1 s |      0,3 s |      1,9 s |
| `qwen2.5:3b`   | 19/30 — **63 %** |        1,7 s |      0,4 s |      6,4 s |

Le modèle `qwen2.5:3b` obtient un meilleur score que le modèle `qwen2.5:1.5b`, avec **30 points de réussite supplémentaires**.

En contrepartie, le modèle 3B est plus lent. Le temps médian passe de **1,1 s à 1,7 s**.

Le modèle 3B est donc retenu pour l'application car son niveau de réussite est nettement supérieur.

---

## 5. Analyse de deux échecs

### Échec 1 — Interprétation des dates

Lors du test 1, le modèle répond :

> « Vendredi prochain (réunion du jeudi 1er octobre) »

Le résultat attendu n'est pas correctement identifié.

Le modèle comprend qu'une information temporelle est présente mais interprète incorrectement la date relative.

**Cause probable :**

Le modèle de langage ne réalise pas toujours correctement les calculs et interprétations de dates relatives.

**Amélioration possible :**

Effectuer le traitement des dates avec Python plutôt que de laisser entièrement cette tâche au modèle.

Python pourrait convertir les expressions comme « vendredi prochain » en une date précise.

---

### Échec 2 — Numéro de téléphone et date

Lors du test 5, le modèle répond :

> « Piège : numéro de téléphone + fin de la semaine »

Le modèle identifie certains éléments du texte mais ne restitue pas correctement toutes les informations attendues.

**Cause probable :**

Le modèle peut mélanger plusieurs informations présentes dans le compte rendu, notamment les informations de contact et les expressions temporelles.

**Amélioration possible :**

Utiliser une sortie structurée avec des champs séparés :

* type d'information
* responsable
* action
* date
* contact

Cela permettrait de limiter les mélanges entre les différentes informations.

---

## 6. Sécurité

### Port Ollama

Ollama utilise le port `11434`.

Ce port n'est pas exposé publiquement.

L'accès à Ollama reste limité à l'environnement local.

### Secrets

Aucun secret ou mot de passe ne doit être stocké dans le dépôt Git.

Les informations sensibles sont placées dans le fichier `.env` et celui-ci ne doit pas être envoyé dans le dépôt.

### Détournement du modèle

L'application a été testée contre plusieurs tentatives de détournement.

Pour le modèle `qwen2.5:3b` :

* 10.1 : ✓
* 10.2 : ✓
* 10.3 : ✓

Les trois tentatives de détournement ont été correctement refusées.

---

## 7. Tableau des risques

| Risque                             | Preuve / constat                                | Protection                           |
| ---------------------------------- | ----------------------------------------------- | ------------------------------------ |
| Accès direct à Ollama              | Le port `11434` n'est pas exposé publiquement   | Ollama reste accessible localement   |
| Fuite de secrets                   | Aucun secret ne doit être présent dans le dépôt | Utilisation du `.env`                |
| Détournement du modèle             | Tests 10.1, 10.2 et 10.3 réussis                | Refus des tentatives de détournement |
| Mauvaise interprétation des dates  | Échec du test 1                                 | Traitement des dates en Python       |
| Mauvaise extraction d'informations | Échec du test 5                                 | Sortie structurée et validation      |
| Erreur du modèle                   | 19/30 au test avec le 3B                        | Validation humaine des résultats     |

---

## 8. Limites

L'application repose sur un modèle de langage local. Les réponses peuvent donc contenir des erreurs.

Les principales limites observées concernent :

* l'interprétation des dates ;
* l'extraction de plusieurs informations dans une même phrase ;
* la distinction entre certaines informations ;
* la compréhension du contexte.

Les résultats générés par l'IA doivent donc être vérifiés par un utilisateur avant utilisation.

---

## 9. Améliorations possibles

Plusieurs améliorations pourraient être mises en place :

1. Traiter les dates avec Python plutôt qu'avec le modèle.
2. Utiliser une sortie JSON structurée.
3. Ajouter une validation automatique des informations extraites.
4. Comparer davantage de modèles locaux.
5. Ajouter davantage de tests d'évaluation.
6. Améliorer la protection contre les injections de prompt.
7. Ajouter une authentification pour limiter l'accès à l'application.

---

## 10. Conclusion

Le projet permet de mettre en place une application web utilisant un modèle de langage local pour analyser des comptes rendus de réunion.

Deux modèles ont été testés : `qwen2.5:1.5b` et `qwen2.5:3b`.

Le modèle `qwen2.5:1.5b` obtient **10/30, soit 33 % de réussite**, avec un temps médian de **1,1 seconde**.

Le modèle `qwen2.5:3b` obtient **19/30, soit 63 % de réussite**, avec un temps médian de **1,7 seconde**.

Le modèle 3B offre donc une amélioration importante de la qualité des résultats, au prix d'un temps de réponse légèrement supérieur.

Les principales difficultés concernent l'interprétation précise des dates et l'extraction de plusieurs informations présentes dans une même phrase.

Une amélioration pertinente serait de confier les traitements déterministes, notamment les dates, à Python et de laisser au modèle les tâches nécessitant une compréhension du langage naturel.
