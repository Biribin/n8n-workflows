<!-- Categorie : Tutorials. Image d'en-tete : capture d'un redeploiement reussi apres echec, 1920x1080. -->

# Title

Auto-retrying a failed Coolify deployment from n8n

# Body

Self-hosting with Coolify, most of my failed deployments were not my code. They were a
registry timeout, a transient DNS hiccup, a build that ran out of memory next to a noisy
neighbour. Retrying by hand the next morning meant the app was down all night for no reason.

## What it does

- Polls the Coolify API for deployments in a failed state.
- Re-triggers the deployment for the matching application.
- Caps the attempts, so a genuinely broken build is not redeployed forever.
- Notifies once the cap is reached, because at that point it really is your code.

## The design decision that matters

**Cap the retries and alert on the cap.** A retry loop without a ceiling turns a broken
build into an infinite build queue, and it hides the failure instead of surfacing it. Two or
three attempts absorb the transient failures, which are the majority; anything beyond that
is a real bug and deserves a human.

## Applies beyond Coolify

The shape is generic: poll a deployment API for failures, re-trigger, count, escalate. Swap
the two HTTP nodes and it works against Dokploy, Portainer, or a plain webhook on your own
build server.

Workflow JSON:
`workflows/exploitation-n8n/coolify-relance-deploiement-echoue.json` in
https://github.com/Biribin/n8n-workflows

The Coolify host and token are replaced with markers.
