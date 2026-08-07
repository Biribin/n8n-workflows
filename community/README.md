# Brouillons de posts pour community.n8n.io

Chaque fichier de ce dossier est un post prêt à coller sur le forum n8n. Ils ne sont
pas publiés automatiquement : le forum n'expose pas d'API d'écriture, donc c'est un
copier-coller manuel.

## Avant de coller un post

1. **Image d'en-tête en 1920x1080.** Les règles de la catégorie Tutorials l'exigent
   pour l'affichage en galerie. Le plus simple : ouvrir le workflow dans n8n, zoomer
   pour que la chaîne remplisse le canevas, capture d'écran, recadrage en 16/9.
2. **Poster en anglais.** Le forum accepte le français, mais l'audience est
   internationale et la portée est sans comparaison en anglais. Chaque brouillon est
   donc rédigé en anglais.
3. **Joindre le JSON** en pièce jointe ou en bloc de code replié, et pointer le fichier
   du dépôt.
4. **Catégorie.** `Tutorials` pour ce qui explique une technique, `Show and tell` pour
   ce qui présente un résultat.

## Ordre de publication conseillé

Du plus fort au plus faible potentiel. Espacer d'au moins quelques jours : huit posts
le même jour se lisent comme du spam et la modération supprime ce qui n'est pas au
niveau.

| # | Post | Catégorie | Pourquoi celui-là |
|--:|------|-----------|-------------------|
| 1 | `01-claude-code-cli-in-n8n.md` | Tutorials | Sujet très demandé, et presque personne ne l'a documenté |
| 2 | `02-n8n-web-app-without-javascript.md` | Tutorials | Motif contre-intuitif, effet « je ne savais pas que c'était possible » |
| 3 | `03-error-workflow.md` | Tutorials | Besoin universel dès qu'on dépasse cinq workflows |
| 4 | `04-llm-prompt-regression-harness.md` | Tutorials | Répond à « comment savoir que mon prompt n'a pas régressé » |
| 5 | `05-discourse-job-watch.md` | Show and tell | Méta et utile : surveiller la catégorie Jobs du forum lui-même |
| 6 | `06-self-testing-postgres-schema.md` | Tutorials | Le workflow vérifie lui-même ses tables et contraintes |
| 7 | `07-coolify-redeploy-retry.md` | Tutorials | Public auto-hébergé, c'est-à-dire la majorité du forum |
| 8 | `08-french-regulatory-watch.md` | Show and tell | Niche française, mais Légifrance OAuth2 n'est documenté nulle part |
| 9 | `09-microsoft-graph-email-weblink.md` | Tutorials | Petit utilitaire, très cherché |
| 10 | `10-showcase-the-whole-repo.md` | Show and tell | Post parapluie, à garder pour la fin |
