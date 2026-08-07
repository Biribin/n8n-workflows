<!-- Categorie : Tutorials. Image d'en-tete : capture du lien resolu ouvrant le courriel, 1920x1080. -->

# Title

A tiny n8n service that turns an Outlook message id into a link a human can click

# Body

Small utility, disproportionately useful. When you ingest data out of Outlook you end up with
rows in a database and a message id that means nothing to anyone. What a user wants is a link
that opens the source email, so they can see for themselves where a number came from.

## The whole thing is four nodes

1. `Webhook` receives a message id.
2. **Microsoft Outlook** node, "get a message". No raw HTTP call needed, the node handles
   the auth.
3. A `Code` node pulls out what is useful: `webLink`, `subject`, and the sender address, with
   a `found: false` fallback when the message no longer exists.
4. `respondToWebhook` returns the JSON.

That is it. Exposed as a webhook rather than a sub-workflow on purpose: anything can call it,
including an application that is not n8n. My internal dashboard hits this endpoint to render
"see the source email" next to each imported line.

## Notes from running it in production

- **Handle the not-found case explicitly.** Message ids are mailbox-scoped and a moved or
  deleted message stops resolving. The Code node checks for a missing `subject` and returns
  `found: false` instead of throwing, so the caller degrades gracefully instead of 500-ing.
- **Store the resolved link, do not resolve it lazily forever.** Save `webLink` next to your
  row at ingestion time. Resolving on every page render costs a Graph call per line and
  breaks the day the message is archived.

## Why bother

This is what turns "the system says 47 hours" into "the system says 47 hours, and here is the
client email that says so". Traceability to the source document is the difference between a
number people argue about and a number people accept.

Workflow JSON:
`workflows/motifs-n8n/graph-resoudre-un-lien-de-courriel.json` in
https://github.com/Biribin/n8n-workflows
