# Réglementaire français

Surveillance de sources officielles (Légifrance, BOFiP, OpenFisca) pour détecter qu'un paramétrage est devenu faux avant que la paie ne parte.

## contre-calcul-sur-source-independante

Contre-calcul mensuel d'un taux réglementaire sur une source indépendante (OpenFisca) pour détecter un écart de paramétrage. Le motif « ne jamais faire confiance à une seule source ».

- Fichier : [`contre-calcul-sur-source-independante.json`](contre-calcul-sur-source-independante.json)
- Déclencheur : planification (cron)
- 3 nœuds
- Briques : scheduleTrigger, Code (JS), HTTP

## legifrance-ingestion-vers-base-vectorielle

Ingestion d'accords de branche depuis Légifrance vers une base vectorielle Supabase : pagination, découpage, plongements, écriture idempotente. Rend un corpus juridique interrogeable en langage naturel.

- Fichier : [`legifrance-ingestion-vers-base-vectorielle.json`](legifrance-ingestion-vers-base-vectorielle.json)
- Déclencheur : planification (cron)
- 32 nœuds
- Briques : scheduleTrigger, HTTP, IF, Code (JS)

## legifrance-obtenir-un-token

Authentification OAuth2 sur l'API Légifrance (plateforme PISTE) : récupération et mise en cache du jeton. Bloc de départ obligatoire pour tout usage de Légifrance, et pas documenté de façon évidente.

- Fichier : [`legifrance-obtenir-un-token.json`](legifrance-obtenir-un-token.json)
- Déclencheur : déclenchement manuel
- 7 nœuds
- Briques : manualTrigger, Code (JS), IF, HTTP

## veille-bareme-fiscal-bofip

Surveillance d'une page du BOFiP pour détecter la publication d'un nouveau barème et alerter avant que le paramétrage devienne faux.

- Fichier : [`veille-bareme-fiscal-bofip.json`](veille-bareme-fiscal-bofip.json)
- Déclencheur : planification (cron)
- 35 nœuds
- Briques : HTTP, Code (JS), scheduleTrigger, xml, extractFromFile

## veille-fiches-de-parametrage

Interrogation périodique d'un annuaire de fiches de paramétrage d'organismes et alerte au changement de version.

- Fichier : [`veille-fiches-de-parametrage.json`](veille-fiches-de-parametrage.json)
- Déclencheur : planification (cron)
- 2 nœuds
- Briques : scheduleTrigger, HTTP

---

Retour au [sommaire](../../README.md).
