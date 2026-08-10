# IA appliquée

LLM branché sur du réel : réponses aux avis et commentaires, classement de documents, répartition de demandes. Toujours avec relecture humaine avant publication.

## avis-google-reponses-ia

Réponse aux avis Google par LLM avec relecture humaine avant publication : brouillon proposé, jamais publié directement.

- Fichier : [`avis-google-reponses-ia.json`](avis-google-reponses-ia.json)
- Déclencheur : planification (cron)
- 11 nœuds
- Briques : Code (JS), boucle par lots, HTTP, scheduleTrigger, IF, Outlook, noOp

## instagram-reponses-commentaires

Réponse automatique aux commentaires Instagram : extraction des jetons, filtres anti-spam en cascade, détection de spam, réponse rédigée par LLM. Montre comment empiler des filtres bon marché avant l'appel payant.

- Fichier : [`instagram-reponses-commentaires.json`](instagram-reponses-commentaires.json)
- Déclencheur : webhook HTTP
- 14 nœuds
- Briques : webhook, IF, réponse HTTP, Code (JS), HTTP, noOp

## motion-designer-dispatch

Répartition de demandes de production vidéo vers les bons intervenants selon des règles, avec accusé de prise en charge.

- Fichier : [`motion-designer-dispatch.json`](motion-designer-dispatch.json)
- Déclencheur : webhook HTTP
- 6 nœuds
- Briques : webhook, Code (JS), HTTP, Wait

## motion-designer-retention

Relance en cascade sur une demande restée sans réponse, avec escalade progressive.

- Fichier : [`motion-designer-retention.json`](motion-designer-retention.json)
- Déclencheur : planification (cron)
- 2 nœuds
- Briques : scheduleTrigger, HTTP

## whatsapp-classer-les-pieces-jointes

Classement des documents reçus sur WhatsApp par un LLM et rangement dans l'arborescence OneDrive correspondante. Inachevé, publié comme point de départ.

- Fichier : [`whatsapp-classer-les-pieces-jointes.json`](whatsapp-classer-les-pieces-jointes.json)
- Déclencheur : webhook HTTP
- 16 nœuds
- Briques : webhook, Code (JS), IF, HTTP, chainLlm, lmChatAnthropic, microsoftOneDrive

---

Retour au [sommaire](../../README.md).
