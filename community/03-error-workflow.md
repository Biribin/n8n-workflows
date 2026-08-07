<!-- Categorie : Tutorials. Image d'en-tete : capture du courriel d'alerte, 1920x1080. -->

# Title

The error workflow you should set up before your fifth workflow

# Body

With five workflows you notice when one breaks. With a hundred you do not, and a silent
scheduled job that has been failing for three weeks is much worse than one that never ran
at all. n8n has a built-in answer that is easy to miss: the **Error Workflow**.

## Setting it up

1. Build a workflow whose trigger is an **Error Trigger** node.
2. In every other workflow: Settings, then Error Workflow, and select it.

That is all. On any failure n8n calls it with the failing workflow's name, the failing
node, the message, the stack, and a link to the execution.

## What mine does

- Formats an email with the workflow name, the node, the message, and a clickable link
  straight to the failed execution.
- No aggregation, on purpose: one failure, one email. Batching taught me to ignore them.

## Two things that took me a while

- **The error workflow cannot report its own failures.** Keep it boring. Mine has no
  external dependency other than sending mail.
- **A node set to `continueRegularOutput` never reaches it.** That setting swallows the
  error by design. Convenient, and an excellent way to lose a whole branch quietly. Use it
  deliberately, and log on the failure path.

Workflow JSON:
`workflows/exploitation-n8n/workflow-d-erreur-global.json` in
https://github.com/Biribin/n8n-workflows
