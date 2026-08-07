# Conformité RGPD

Purges automatiques au-delà de la durée de rétention déclarée. Court terme et long terme.

## purge-rgpd-mensuelle

Purge mensuelle des journaux au-delà de la durée de rétention déclarée. Anonymisé : les tables sont remplacées par des exemples.

- Fichier : [`purge-rgpd-mensuelle.json`](purge-rgpd-mensuelle.json)
- Déclencheur : planification (cron)
- 5 nœuds
- Briques : scheduleTrigger, HTTP, IF, Code (JS)

## purge-rgpd-quotidienne

Purge quotidienne des données transitoires. Le pendant court terme de la purge mensuelle.

- Fichier : [`purge-rgpd-quotidienne.json`](purge-rgpd-quotidienne.json)
- Déclencheur : planification (cron)
- 5 nœuds
- Briques : scheduleTrigger, HTTP, IF, Code (JS)

---

Retour au [sommaire](../../README.md).
