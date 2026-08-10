# Prospection et génération de leads

Identification de prospects, enrichissement, déduplication et rédaction de messages personnalisés. Toutes les fiches contact ont été retirées : ce qui reste est la mécanique.

## apollo-enrichissement-par-lots

Enrichissement d'une liste de contacts via l'API Apollo, par lots, avec reprise là où la boucle s'est arrêtée. La liste d'entrée a été retirée : branchez la vôtre.

- Fichier : [`apollo-enrichissement-par-lots.json`](apollo-enrichissement-par-lots.json)
- Déclencheur : déclenchement manuel
- 7 nœuds
- Briques : manualTrigger, Code (JS), Set, HTTP, boucle par lots

## apollo-pipeline-de-leads

Pipeline de génération de leads sur l'API Apollo : recherche d'organisations, recherche de personnes dans ces organisations, révélation de l'adresse professionnelle, puis écriture en feuille. Traitement par lots pour tenir le quota, et reprise là où la boucle s'est arrêtée.

- Fichier : [`apollo-pipeline-de-leads.json`](apollo-pipeline-de-leads.json)
- Déclencheur : formulaire n8n
- 48 nœuds
- Briques : HTTP, formTrigger, Code (JS), IF, boucle par lots, Google Sheets, Merge

## dedoublonnage-multi-feuilles

Construction d'une liste de dédoublonnage à partir de plusieurs feuilles à la fois, exposée en webhook pour être interrogée par les autres workflows. Le motif « une seule source de vérité pour savoir si on a déjà contacté quelqu'un ».

- Fichier : [`dedoublonnage-multi-feuilles.json`](dedoublonnage-multi-feuilles.json)
- Déclencheur : webhook HTTP
- 12 nœuds
- Briques : webhook, microsoftExcel, Code (JS)

## dedoublonnage-par-segment-a

Déclinaison du dédoublonnage sur un segment précis. Publié pour montrer comment décliner le motif sans le dupliquer.

- Fichier : [`dedoublonnage-par-segment-a.json`](dedoublonnage-par-segment-a.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : webhook, microsoftExcel, Code (JS)

## dedoublonnage-par-segment-b

Deuxième déclinaison du dédoublonnage par segment.

- Fichier : [`dedoublonnage-par-segment-b.json`](dedoublonnage-par-segment-b.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : webhook, microsoftExcel, Code (JS)

## dedoublonnage-par-segment-c

Troisième déclinaison du dédoublonnage par segment.

- Fichier : [`dedoublonnage-par-segment-c.json`](dedoublonnage-par-segment-c.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : webhook, microsoftExcel, Code (JS)

## ecriture-en-feuille-par-segment-a

Réception d'une liste en webhook, éclatement en lignes, écriture dans la feuille du segment. Petit mais c'est la moitié utile d'un pipeline : séparer la collecte de l'écriture.

- Fichier : [`ecriture-en-feuille-par-segment-a.json`](ecriture-en-feuille-par-segment-a.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : webhook, splitOut, microsoftExcel

## ecriture-en-feuille-par-segment-b

Même motif d'écriture, autre segment.

- Fichier : [`ecriture-en-feuille-par-segment-b.json`](ecriture-en-feuille-par-segment-b.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : webhook, splitOut, microsoftExcel

## ecriture-en-feuille-par-segment-c

Même motif d'écriture, autre segment.

- Fichier : [`ecriture-en-feuille-par-segment-c.json`](ecriture-en-feuille-par-segment-c.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : microsoftExcel, webhook, splitOut

## ecriture-en-feuille-par-segment-d

Même motif d'écriture, autre segment, avec une étape d'éclatement supplémentaire.

- Fichier : [`ecriture-en-feuille-par-segment-d.json`](ecriture-en-feuille-par-segment-d.json)
- Déclencheur : webhook HTTP
- 4 nœuds
- Briques : splitOut, webhook, microsoftExcel

## ecriture-en-feuille-par-segment-e

Même motif d'écriture, autre segment.

- Fichier : [`ecriture-en-feuille-par-segment-e.json`](ecriture-en-feuille-par-segment-e.json)
- Déclencheur : webhook HTTP
- 3 nœuds
- Briques : webhook, splitOut, microsoftExcel

## linkedin-post-hebdo-depuis-une-veille

Publication hebdomadaire sur LinkedIn alimentée par une veille RSS : lecture des flux, rédaction par LLM, envoi pour relecture avant publication. Rien ne part sans validation humaine.

- Fichier : [`linkedin-post-hebdo-depuis-une-veille.json`](linkedin-post-hebdo-depuis-une-veille.json)
- Déclencheur : planification (cron)
- 8 nœuds
- Briques : scheduleTrigger, Code (JS), rssFeedRead, HTTP, Outlook

## linkedin-publication-assistee

Chaîne de publication LinkedIn : sujet tiré d'une feuille, rédaction par LLM, envoi du brouillon sur Telegram pour relecture, publication après accord.

- Fichier : [`linkedin-publication-assistee.json`](linkedin-publication-assistee.json)
- Déclencheur : planification (cron)
- 7 nœuds
- Briques : linkedIn, scheduleTrigger, Google Sheets, telegram, openAi

## linkedin-publication-sous-workflow

Version sous-workflow de la publication LinkedIn, appelable depuis une autre chaîne. Utile pour ne garder qu'un seul endroit où la rédaction est définie.

- Fichier : [`linkedin-publication-sous-workflow.json`](linkedin-publication-sous-workflow.json)
- Déclencheur : appel depuis un autre workflow
- 7 nœuds
- Briques : linkedIn, openAi, Google Sheets, telegram, executeWorkflowTrigger

## radar-de-veille-commerciale

Radar quotidien sur des sources publiques : collecte, filtrage, dédoublonnage contre l'historique, puis alerte sur les seules nouveautés. 46 nœuds, c'est le plus complet de la famille.

- Fichier : [`radar-de-veille-commerciale.json`](radar-de-veille-commerciale.json)
- Déclencheur : planification (cron), déclenchement manuel
- 46 nœuds
- Briques : scheduleTrigger, Code (JS), boucle par lots, filter, manualTrigger, HTTP, IF, Wait

## recherche-de-prospects-sur-formulaire

Recherche de prospects déclenchée par un formulaire : un nom d'entreprise en entrée, recherche d'organisation, recherche de personnes, enrichissement, écriture en feuille. Le parcours complet du besoin à la liste exploitable.

- Fichier : [`recherche-de-prospects-sur-formulaire.json`](recherche-de-prospects-sur-formulaire.json)
- Déclencheur : formulaire n8n
- 52 nœuds
- Briques : HTTP, formTrigger, Code (JS), IF, boucle par lots, Merge, microsoftExcel, chainLlm, lmChatAnthropic

## scraper-offres-france-travail

Scraping de l'API France Travail par mots-clés, avec pagination, temporisation de quota et écriture incrémentale. Utile bien au-delà de la prospection.

- Fichier : [`scraper-offres-france-travail.json`](scraper-offres-france-travail.json)
- Déclencheur : planification (cron)
- 49 nœuds
- Briques : HTTP, boucle par lots, microsoftExcel, Code (JS), IF, Merge, Wait, scheduleTrigger, filter

## scraper-offres-indeed

Scraping d'un agrégateur d'offres : boucle par mots-clés, pagination, dédoublonnage, écriture des retenues et des rejetées. 60 nœuds, avec toute la gestion d'erreur.

- Fichier : [`scraper-offres-indeed.json`](scraper-offres-indeed.json)
- Déclencheur : planification (cron)
- 60 nœuds
- Briques : scheduleTrigger, microsoftExcel, HTTP, boucle par lots, IF, Code (JS), Merge, Wait, Switch, filter

## scraping-asynchrone-avec-reprise

Scraping d'un annuaire page par page, en asynchrone : chaque page est relancée par un webhook plutôt que par une boucle, ce qui évite le délai maximum d'exécution sur un gros volume. Le motif à connaître quand une boucle synchrone expire.

- Fichier : [`scraping-asynchrone-avec-reprise.json`](scraping-asynchrone-avec-reprise.json)
- Déclencheur : webhook HTTP
- 34 nœuds
- Briques : microsoftExcel, webhook, Wait, IF

## veille-offres-avec-scoring-llm

Balayage d'un agrégateur d'offres sur une liste de mots-clés, scoring de pertinence par LLM, puis écriture des retenues et des rejetées dans deux feuilles distinctes. Les rejetées sont conservées : sans elles, impossible de savoir si le filtre est trop sévère.

- Fichier : [`veille-offres-avec-scoring-llm.json`](veille-offres-avec-scoring-llm.json)
- Déclencheur : planification (cron)
- 47 nœuds
- Briques : boucle par lots, IF, Code (JS), microsoftExcel, HTTP, Merge, Wait, scheduleTrigger, filter

---

Retour au [sommaire](../../README.md).
