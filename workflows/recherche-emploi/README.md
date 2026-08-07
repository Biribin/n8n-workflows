# Recherche d'emploi automatisée

Chaîne complète de candidature : découverte d'offres, rédaction adaptée, envoi, relance, lecture des réponses. Projet personnel, donc publiable en entier.

## 01-decouverte-offres

Découverte d'offres d'emploi : plan de recherche déclaratif en YAML, interrogation de l'API France Travail, tri des offres par un service de scoring externe. Zéro token consommé sur la phase de collecte.

- Fichier : [`01-decouverte-offres.json`](01-decouverte-offres.json)
- Déclencheur : webhook HTTP, planification (cron)
- 11 nœuds
- Briques : webhook, Set, HTTP, Code (JS), boucle par lots, scheduleTrigger

## 02-generation-lettre-et-cv

Génération d'une lettre et d'un CV adaptés à une offre validée : appel d'un service de rédaction, rendu PDF par une GitHub Action sur une branche jetable, récupération des fichiers.

- Fichier : [`02-generation-lettre-et-cv.json`](02-generation-lettre-et-cv.json)
- Déclencheur : webhook HTTP
- 37 nœuds
- Briques : webhook, Set, Code (JS), HTTP, Wait, IF, PostgreSQL, Gmail, extractFromFile

## 03-envoi-et-relances

Envoi au recruteur ou dépôt sur portail selon la décision humaine, archivage, puis relance planifiée si pas de réponse. Montre un point d'arrêt humain au milieu d'une chaîne automatique.

- Fichier : [`03-envoi-et-relances.json`](03-envoi-et-relances.json)
- Déclencheur : webhook HTTP, planification (cron)
- 46 nœuds
- Briques : webhook, Set, scheduleTrigger, Code (JS), HTTP, PostgreSQL, Switch, IF, Gmail, boucle par lots

## 04-ecoute-gmail-des-reponses

Écoute d'une boîte Gmail, rattachement d'une réponse à la candidature d'origine et classement du verdict. Un accusé de réception automatique n'est pas compté comme une réponse.

- Fichier : [`04-ecoute-gmail-des-reponses.json`](04-ecoute-gmail-des-reponses.json)
- Déclencheur : arrivée de courriel Gmail, webhook HTTP
- 24 nœuds
- Briques : gmailTrigger, Set, Code (JS), PostgreSQL, IF, HTTP, Gmail, boucle par lots, webhook

## 05-decouverte-ats

Balayage quotidien de dizaines de connecteurs ATS (Greenhouse, Lever, Workday...) via un service d'agrégation, pour ne pas dépendre d'un seul portail d'emploi.

- Fichier : [`05-decouverte-ats.json`](05-decouverte-ats.json)
- Déclencheur : planification (cron)
- 6 nœuds
- Briques : scheduleTrigger, Set, HTTP, Code (JS), IF, Gmail

## harnais-de-test-agent-llm

Harnais de non-régression pour un agent LLM : rejoue le vrai prompt de production et applique une série de contrôles automatiques sur ses règles absolues. Répond à « comment savoir qu'un prompt n'a pas régressé ».

- Fichier : [`harnais-de-test-agent-llm.json`](harnais-de-test-agent-llm.json)
- Déclencheur : déclenchement manuel
- 6 nœuds
- Briques : manualTrigger, Code (JS)

## veille-forum-discourse

Veille sur un forum Discourse : lecture d'une catégorie en JSON, scoring de pertinence par un LLM contre un profil, déduplication par data table n8n, alerte seulement sur ce qui compte. Deuxième branche : alerte si on cite votre nom. Le marquage « vu » n'est écrit qu'après réussite de la poussée, sinon le sujet repasse au tour suivant.

- Fichier : [`veille-forum-discourse.json`](veille-forum-discourse.json)
- Déclencheur : planification (cron)
- 20 nœuds
- Briques : scheduleTrigger, HTTP, Code (JS), data table n8n, IF

---

Retour au [sommaire](../../README.md).
