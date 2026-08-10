# Workflows n8n

**51 workflows n8n**, sortis d'une plateforme auto-hébergée que je fais tourner au quotidien, en licence MIT. Ils sont là parce qu'ils sont réutilisables tels quels.

Je travaille en no-code et low-code : je cadre le besoin, j'assemble, et quand du code est nécessaire je le fais produire par des agents puis je le recette. Tout ce qui est ici a tourné en vrai, pas dans un tutoriel.

## Les familles

| Famille | Workflows | À quoi ça sert |
|---|---:|---|
| [Recherche d'emploi automatisée](workflows/recherche-emploi/) | 7 | Chaîne complète de candidature : découverte d'offres, rédaction adaptée, envoi, relance, lecture des réponses. Projet personnel, donc publiable en entier. |
| [Prospection et génération de leads](workflows/prospection/) | 22 | Identification de prospects, enrichissement, déduplication et rédaction de messages personnalisés. Toutes les fiches contact ont été retirées : ce qui reste est la mécanique. |
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

### Prospection et génération de leads

- **[apollo-enrichissement-par-lots](workflows/prospection/apollo-enrichissement-par-lots.json)** Enrichissement d'une liste de contacts via l'API Apollo, par lots, avec reprise là où la boucle s'est arrêtée.
- **[apollo-pipeline-de-leads](workflows/prospection/apollo-pipeline-de-leads.json)** Pipeline de génération de leads sur l'API Apollo : recherche d'organisations, recherche de personnes dans ces organisations, révélation de l'adresse professionnelle, puis écriture en feuille.
- **[cold-outreach-avec-relances](workflows/prospection/cold-outreach-avec-relances.json)** Variante de la séquence de prise de contact avec relances échelonnées et sortie de séquence dès qu'une réponse arrive.
- **[cold-outreach-cadence-et-plafond](workflows/prospection/cold-outreach-cadence-et-plafond.json)** Séquence de prise de contact à froid depuis une feuille : filtre sur l'étape en cours, plafond d'envois par exécution, envoi, puis mise à jour de l'étape.
- **[dedoublonnage-multi-feuilles](workflows/prospection/dedoublonnage-multi-feuilles.json)** Construction d'une liste de dédoublonnage à partir de plusieurs feuilles à la fois, exposée en webhook pour être interrogée par les autres workflows.
- **[dedoublonnage-par-segment-a](workflows/prospection/dedoublonnage-par-segment-a.json)** Déclinaison du dédoublonnage sur un segment précis.
- **[dedoublonnage-par-segment-b](workflows/prospection/dedoublonnage-par-segment-b.json)** Deuxième déclinaison du dédoublonnage par segment.
- **[dedoublonnage-par-segment-c](workflows/prospection/dedoublonnage-par-segment-c.json)** Troisième déclinaison du dédoublonnage par segment.
- **[ecriture-en-feuille-par-segment-a](workflows/prospection/ecriture-en-feuille-par-segment-a.json)** Réception d'une liste en webhook, éclatement en lignes, écriture dans la feuille du segment.
- **[ecriture-en-feuille-par-segment-b](workflows/prospection/ecriture-en-feuille-par-segment-b.json)** Même motif d'écriture, autre segment.
- **[ecriture-en-feuille-par-segment-c](workflows/prospection/ecriture-en-feuille-par-segment-c.json)** Même motif d'écriture, autre segment.
- **[ecriture-en-feuille-par-segment-d](workflows/prospection/ecriture-en-feuille-par-segment-d.json)** Même motif d'écriture, autre segment, avec une étape d'éclatement supplémentaire.
- **[ecriture-en-feuille-par-segment-e](workflows/prospection/ecriture-en-feuille-par-segment-e.json)** Même motif d'écriture, autre segment.
- **[linkedin-post-hebdo-depuis-une-veille](workflows/prospection/linkedin-post-hebdo-depuis-une-veille.json)** Publication hebdomadaire sur LinkedIn alimentée par une veille RSS : lecture des flux, rédaction par LLM, envoi pour relecture avant publication.
- **[linkedin-publication-assistee](workflows/prospection/linkedin-publication-assistee.json)** Chaîne de publication LinkedIn : sujet tiré d'une feuille, rédaction par LLM, envoi du brouillon sur Telegram pour relecture, publication après accord.
- **[linkedin-publication-sous-workflow](workflows/prospection/linkedin-publication-sous-workflow.json)** Version sous-workflow de la publication LinkedIn, appelable depuis une autre chaîne.
- **[radar-de-veille-commerciale](workflows/prospection/radar-de-veille-commerciale.json)** Radar quotidien sur des sources publiques : collecte, filtrage, dédoublonnage contre l'historique, puis alerte sur les seules nouveautés.
- **[recherche-de-prospects-sur-formulaire](workflows/prospection/recherche-de-prospects-sur-formulaire.json)** Recherche de prospects déclenchée par un formulaire : un nom d'entreprise en entrée, recherche d'organisation, recherche de personnes, enrichissement, écriture en feuille.
- **[scraper-offres-france-travail](workflows/prospection/scraper-offres-france-travail.json)** Scraping de l'API France Travail par mots-clés, avec pagination, temporisation de quota et écriture incrémentale.
- **[scraper-offres-indeed](workflows/prospection/scraper-offres-indeed.json)** Scraping d'un agrégateur d'offres : boucle par mots-clés, pagination, dédoublonnage, écriture des retenues et des rejetées.
- **[scraping-asynchrone-avec-reprise](workflows/prospection/scraping-asynchrone-avec-reprise.json)** Scraping d'un annuaire page par page, en asynchrone : chaque page est relancée par un webhook plutôt que par une boucle, ce qui évite le délai maximum d'exécution sur un gros volume.
- **[veille-offres-avec-scoring-llm](workflows/prospection/veille-offres-avec-scoring-llm.json)** Balayage d'un agrégateur d'offres sur une liste de mots-clés, scoring de pertinence par LLM, puis écriture des retenues et des rejetées dans deux feuilles distinctes.

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

Les workflows sont livrés **désactivés**. Lisez-les avant de les mettre en route, surtout ceux qui envoient des courriels.

## Ce que vous ne trouverez pas ici

- Aucune clé d'API, aucun jeton, aucune chaîne de connexion. Un script de vérification indépendant refuse la publication s'il en détecte une.
- Aucune donnée de test épinglée : les `pinData` ont été supprimées en bloc, c'était du réel.
- Aucune adresse courriel, aucun numéro de téléphone, aucun nom de tiers.

## Licence

MIT. Servez-vous, y compris en entreprise. Si un de ces workflows vous fait gagner du temps, ça me fait plaisir de le savoir.

---

Linéo Biribin, automatisation et IA appliquée. [linkedin.com/in/lineo-biribin](https://www.linkedin.com/in/lineo-biribin/)
