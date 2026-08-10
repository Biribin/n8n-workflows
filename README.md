# Workflows n8n

Une partie de ma plateforme d'automatisation auto-hébergée : **213 workflows conçus, 109 actifs en production**. **29** sont publiés ici, en licence MIT, parce qu'ils sont réutilisables tels quels et qu'ils ne racontent rien de confidentiel. Le reste est décrit plus bas sans son JSON.

Je travaille en no-code et low-code : je cadre le besoin, j'assemble, et quand du code est nécessaire je le fais produire par des agents puis je le recette. Ce qui est publié ici a tourné en vrai, pas dans un tutoriel.

## Ce qui est publié

| Famille | Workflows | À quoi ça sert |
|---|---:|---|
| [Recherche d'emploi automatisée](workflows/recherche-emploi/) | 7 | Chaîne complète de candidature : découverte d'offres, rédaction adaptée, envoi, relance, lecture des réponses. Projet personnel, donc publiable en entier. |
| [Exploitation de n8n](workflows/exploitation-n8n/) | 6 | Ce qu'il faut ajouter quand on passe de 5 workflows à 100 : capture d'erreur, relance de déploiement, suivi du coût des API, veille. |
| [Réglementaire français](workflows/reglementaire-france/) | 5 | Surveillance de sources officielles (Légifrance, BOFiP, OpenFisca) pour détecter qu'un paramétrage est devenu faux avant que la paie ne parte. |
| [IA appliquée](workflows/ia-appliquee/) | 5 | LLM branché sur du réel : réponses aux avis et commentaires, classement de documents, répartition de demandes. Toujours avec relecture humaine avant publication. |
| [Motifs n8n](workflows/motifs-n8n/) | 4 | Techniques n8n peu documentées : interface web sans JavaScript, sous-workflow utilitaire, schéma de base auto-testé, utilitaire Microsoft Graph. |
| [Conformité RGPD](workflows/conformite-rgpd/) | 2 | Purges automatiques au-delà de la durée de rétention déclarée. Court terme et long terme. |

### Recherche d'emploi automatisée

- **[01-decouverte-offres](workflows/recherche-emploi/01-decouverte-offres.json)** Découverte d'offres d'emploi : plan de recherche déclaratif en YAML, interrogation de l'API France Travail, tri des offres par un service de scoring externe.
- **[02-generation-lettre-et-cv](workflows/recherche-emploi/02-generation-lettre-et-cv.json)** Génération d'une lettre et d'un CV adaptés à une offre validée : appel d'un service de rédaction, rendu PDF par une GitHub Action sur une branche jetable, récupération des fichiers.
- **[03-envoi-et-relances](workflows/recherche-emploi/03-envoi-et-relances.json)** Envoi au recruteur ou dépôt sur portail selon la décision humaine, archivage, puis relance planifiée si pas de réponse.
- **[04-ecoute-gmail-des-reponses](workflows/recherche-emploi/04-ecoute-gmail-des-reponses.json)** Écoute d'une boîte Gmail, rattachement d'une réponse à la candidature d'origine et classement du verdict.
- **[05-decouverte-ats](workflows/recherche-emploi/05-decouverte-ats.json)** Balayage quotidien de dizaines de connecteurs ATS (Greenhouse, Lever, Workday...) via un service d'agrégation, pour ne pas dépendre d'un seul portail d'emploi.
- **[harnais-de-test-agent-llm](workflows/recherche-emploi/harnais-de-test-agent-llm.json)** Harnais de non-régression pour un agent LLM : rejoue le vrai prompt de production et applique une série de contrôles automatiques sur ses règles absolues.
- **[veille-forum-discourse](workflows/recherche-emploi/veille-forum-discourse.json)** Veille sur un forum Discourse : lecture d'une catégorie en JSON, scoring de pertinence par un LLM contre un profil, déduplication par data table n8n, alerte seulement sur ce qui compte.

### Exploitation de n8n

- **[appeler-claude-code-cli-depuis-n8n](workflows/exploitation-n8n/appeler-claude-code-cli-depuis-n8n.json)** Appel du CLI Claude Code depuis un nœud Code n8n via child_process.
- **[coolify-relance-deploiement-echoue](workflows/exploitation-n8n/coolify-relance-deploiement-echoue.json)** Relance automatique d'un déploiement Coolify en échec, avec plafond de tentatives.
- **[notifier-un-echec-de-generation](workflows/exploitation-n8n/notifier-un-echec-de-generation.json)** Remontée d'un échec de génération de document vers un CRM plutôt que vers un journal que personne ne lit.
- **[suivi-consommation-api](workflows/exploitation-n8n/suivi-consommation-api.json)** Collecte hebdomadaire de la consommation des API payantes utilisées par la plateforme, pour voir venir la facture au lieu de la découvrir.
- **[veille-ia-compte-rendu-hebdo](workflows/exploitation-n8n/veille-ia-compte-rendu-hebdo.json)** Veille technologique hebdomadaire : collecte de sources, synthèse par LLM, compte rendu par courriel.
- **[workflow-d-erreur-global](workflows/exploitation-n8n/workflow-d-erreur-global.json)** Le workflow d'erreur à brancher sur tous les autres : capte l'échec, identifie le workflow fautif et le nœud, notifie.

### Réglementaire français

- **[contre-calcul-sur-source-independante](workflows/reglementaire-france/contre-calcul-sur-source-independante.json)** Contre-calcul mensuel d'un taux réglementaire sur une source indépendante (OpenFisca) pour détecter un écart de paramétrage.
- **[legifrance-ingestion-vers-base-vectorielle](workflows/reglementaire-france/legifrance-ingestion-vers-base-vectorielle.json)** Ingestion d'accords de branche depuis Légifrance vers une base vectorielle Supabase : pagination, découpage, plongements, écriture idempotente.
- **[legifrance-obtenir-un-token](workflows/reglementaire-france/legifrance-obtenir-un-token.json)** Authentification OAuth2 sur l'API Légifrance (plateforme PISTE) : récupération et mise en cache du jeton.
- **[veille-bareme-fiscal-bofip](workflows/reglementaire-france/veille-bareme-fiscal-bofip.json)** Surveillance d'une page du BOFiP pour détecter la publication d'un nouveau barème et alerter avant que le paramétrage devienne faux.
- **[veille-fiches-de-parametrage](workflows/reglementaire-france/veille-fiches-de-parametrage.json)** Interrogation périodique d'un annuaire de fiches de paramétrage d'organismes et alerte au changement de version.

### IA appliquée

- **[avis-google-reponses-ia](workflows/ia-appliquee/avis-google-reponses-ia.json)** Réponse aux avis Google par LLM avec relecture humaine avant publication : brouillon proposé, jamais publié directement.
- **[instagram-reponses-commentaires](workflows/ia-appliquee/instagram-reponses-commentaires.json)** Réponse automatique aux commentaires Instagram : extraction des jetons, filtres anti-spam en cascade, détection de spam, réponse rédigée par LLM.
- **[motion-designer-dispatch](workflows/ia-appliquee/motion-designer-dispatch.json)** Répartition de demandes de production vidéo vers les bons intervenants selon des règles, avec accusé de prise en charge.
- **[motion-designer-retention](workflows/ia-appliquee/motion-designer-retention.json)** Relance en cascade sur une demande restée sans réponse, avec escalade progressive.
- **[whatsapp-classer-les-pieces-jointes](workflows/ia-appliquee/whatsapp-classer-les-pieces-jointes.json)** Classement des documents reçus sur WhatsApp par un LLM et rangement dans l'arborescence OneDrive correspondante.

### Motifs n8n

- **[docuseal-gabarits-vierges](workflows/motifs-n8n/docuseal-gabarits-vierges.json)** Sous-workflow utilitaire : création de gabarits DocuSeal vierges à la demande, appelable par d'autres workflows.
- **[graph-resoudre-un-lien-de-courriel](workflows/motifs-n8n/graph-resoudre-un-lien-de-courriel.json)** Utilitaire Microsoft Graph : transformer un identifiant de message en lien web ouvrable.
- **[interface-web-n8n-sans-javascript](workflows/motifs-n8n/interface-web-n8n-sans-javascript.json)** Une interface web complète servie par n8n sans une ligne de JavaScript côté navigateur : formulaires HTML classiques, état porté par l'URL.
- **[postgres-schema-et-auto-test](workflows/motifs-n8n/postgres-schema-et-auto-test.json)** Création d'un schéma PostgreSQL depuis n8n avec auto-test immédiat : le workflow vérifie lui-même que ses tables, index et contraintes sont bien en place.

### Conformité RGPD

- **[purge-rgpd-mensuelle](workflows/conformite-rgpd/purge-rgpd-mensuelle.json)** Purge mensuelle des journaux au-delà de la durée de rétention déclarée.
- **[purge-rgpd-quotidienne](workflows/conformite-rgpd/purge-rgpd-quotidienne.json)** Purge quotidienne des données transitoires.

## Ce qui n'est pas publié, et pourquoi

Le reste de la plateforme tourne sur les données d'un employeur et de personnes physiques : identités d'agents, badges aéroportuaires, bulletins de paie, déclarations sociales. Publier ces workflows exposerait l'architecture interne d'une entreprise et des données personnelles, même anonymisés. Ils restent privés.

Voici quand même de quoi il s'agit. **Le détail, les captures et une démonstration commentée sont disponibles sur demande en entretien.**

| Famille | Workflows | Dont actifs | À quoi ça sert |
|---|---:|---:|---|
| Temps de travail et émargement | 39 | 21 | Réconciliation entre planning prévisionnel, pointage badgeuse et relevé client, calcul des heures et des majorations de nuit. |
| Référentiels RH | 28 | 11 | Synchronisation et contrôle des référentiels agents : identité, badges aéroportuaires, habilitations, formations, absences. |
| Prospection et génération de leads | 22 | 1 | Identification de prospects, enrichissement, déduplication et rédaction de messages personnalisés. |
| Bac à sable | 19 | 5 | Essais, sondes de débogage, copies de travail. Gardés pour l'historique, sans valeur de démonstration. |
| Courriel, OCR et ingestion documentaire | 18 | 7 | Lecture de boîtes Outlook multiples, extraction des pièces jointes, OCR par LLM, résolution d'identité avec table d'exceptions apprise. |
| Paie et déclaratif social | 17 | 15 | DSN, DPAE, prélèvement à la source, cotisations, acomptes, facturation mensuelle. |
| Serveurs MCP sur le SI | 15 | 13 | Exposition du SI RH à un assistant conversationnel : identité, badges, compétences, formations, émargement, planning, avec cache. |
| Contrats et signature | 7 | 2 | Génération, envoi en signature électronique et suivi des contrats. |
| Tableaux de bord et rapports | 7 | 6 | Agrégation de données de production vers des tableaux de bord et des rapports périodiques. |
| Sites et formulaires | 5 | 4 | Formulaires de contact et de candidature, pages servies directement par n8n. |
| Exploitation de n8n | 4 | 3 | Sous-workflows internes et rejeux de journaux propres à mon instance. |
| Recherche d'emploi automatisée | 2 | 1 | Variantes de travail et version monolithique antérieure au découpage en quatre workflows. |
| Veille réglementaire | 1 | 0 | Sondes de veille encore attachées à des sources internes. |

## Importer un workflow

```
n8n > Workflows > ... > Import from File
```

Puis, dans l'ordre :

1. **Reconnecter les credentials.** Toutes les références sont remises à `A_RECONNECTER` : aucun secret n'est présent dans ces fichiers.
2. **Remplacer les valeurs `REMPLACER`.** Chaque identifiant, clé ou jeton qui était écrit en dur a été retiré et porte ce marqueur.
3. **Régénérer les identifiants de webhook** (`A_REGENERER`) en réenregistrant le nœud.
4. **Créer les data tables et les tables Postgres** que le workflow attend. Les noms sont conservés.
5. Les hôtes internes sont remplacés par `exemple.internal` : mettez les vôtres.

Les workflows sont livrés **inactifs**. Lisez-les avant de les activer, surtout ceux qui envoient des courriels.

## Ce que vous ne trouverez pas ici

- Aucune clé d'API, aucun jeton, aucune chaîne de connexion. Un script de vérification indépendant refuse la publication s'il en détecte une.
- Aucune donnée de test épinglée : les `pinData` ont été supprimées en bloc, c'était du réel.
- Aucune adresse courriel, aucun numéro de téléphone, aucun nom de tiers.

## Licence

MIT. Servez-vous, y compris en entreprise. Si un de ces workflows vous fait gagner du temps, ça me fait plaisir de le savoir.

---

Linéo Biribin, automatisation et IA appliquée. [linkedin.com/in/lineo-biribin](https://www.linkedin.com/in/lineo-biribin/)
