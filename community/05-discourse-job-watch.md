<!-- Categorie : Show and tell. Image d'en-tete : capture du digest recu, 1920x1080. -->

# Title

Watching this forum's Jobs category with an LLM scoring each post against my profile

# Body

This one is a little meta: it watches **this** forum. Any Discourse instance exposes JSON
on every page, so no scraping and no HTML parsing is needed.

- `/c/jobs/13.json` gives the topic list
- `/t/<id>.json` gives a topic with its posts
- `/search.json?q=<term>` searches everything

Three times a day it reads the Jobs category, keeps topics active in the last 21 days,
drops the ones it has already seen, asks a model to score the rest from 0 to 10 against a
profile paragraph, and only alerts on the relevant ones. A second branch searches the whole
forum for my own name, so I know if someone is looking for me.

## The bug worth stealing the fix from

My first version marked a topic as "seen" **in parallel** with pushing it downstream. When
the push failed, the topic was still marked seen, so it was never retried. I lost six
relevant offers that way before noticing.

The fix is a shape, not a line of code: replace the Filter node with an **IF** node. A
Filter has one output, so the bookkeeping had to live on a parallel branch. An IF has two,
so each case gets its own path, and the "mark as seen" node moves to **after** the
successful push. If the push fails, no row is written, the dedup check re-presents the
topic on the next run, and it gets pushed again. The price is one extra model call. The
price of the old behaviour was an offer lost forever.

## Second trap, same family

The response from my downstream API does not carry the topic. Item linking does not survive
an HTTP node, so re-attaching by index is required, exactly like reading a model's answer
back onto its own input. Skip that and you write a row with a null dedup key, which
deduplicates nothing, so the topic is reprocessed forever. Ask me how I know.

## No secret in it

Deduplication uses an n8n **data table**, not an external store, so the workflow carries no
token at all.

Workflow JSON:
`workflows/recherche-emploi/veille-forum-discourse.json` in
https://github.com/Biribin/n8n-workflows

The profile paragraph is replaced with a placeholder. Put yours there: the more specific it
is, the less the scoring drifts.
