# Motifs n8n

Techniques n8n peu documentées : interface web sans JavaScript, sous-workflow utilitaire, schéma de base auto-testé, utilitaire Microsoft Graph.

## docuseal-gabarits-vierges

Sous-workflow utilitaire : création de gabarits DocuSeal vierges à la demande, appelable par d'autres workflows.

- Fichier : [`docuseal-gabarits-vierges.json`](docuseal-gabarits-vierges.json)
- Déclencheur : webhook HTTP
- 17 nœuds
- Briques : webhook, googleDrive, réponse HTTP, googleDocs

## graph-resoudre-un-lien-de-courriel

Utilitaire Microsoft Graph : transformer un identifiant de message en lien web ouvrable. Petit mais nécessaire dès qu'on veut tracer une donnée jusqu'au courriel d'origine.

- Fichier : [`graph-resoudre-un-lien-de-courriel.json`](graph-resoudre-un-lien-de-courriel.json)
- Déclencheur : webhook HTTP
- 4 nœuds
- Briques : webhook, réponse HTTP, Outlook, Code (JS)

## interface-web-n8n-sans-javascript

Une interface web complète servie par n8n sans une ligne de JavaScript côté navigateur : formulaires HTML classiques, état porté par l'URL. Utile quand on n'a pas le droit d'héberger une application.

- Fichier : [`interface-web-n8n-sans-javascript.json`](interface-web-n8n-sans-javascript.json)
- Déclencheur : webhook HTTP
- 11 nœuds
- Briques : webhook, Code (JS), réponse HTTP

## postgres-schema-et-auto-test

Création d'un schéma PostgreSQL depuis n8n avec auto-test immédiat : le workflow vérifie lui-même que ses tables, index et contraintes sont bien en place.

- Fichier : [`postgres-schema-et-auto-test.json`](postgres-schema-et-auto-test.json)
- Déclencheur : déclenchement manuel
- 14 nœuds
- Briques : manualTrigger, PostgreSQL, Code (JS)

---

Retour au [sommaire](../../README.md).
