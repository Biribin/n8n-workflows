<!-- Categorie : Show and tell. A publier EN DERNIER, une fois les autres posts en ligne.
     Image d'en-tete : capture de l'arborescence du depot ou une mosaique de canevas. -->

# Title

29 workflows from a 213-workflow self-hosted instance, sanitised and documented (MIT)

# Body

I run a self-hosted n8n instance for a 900-employee staffing group: 213 workflows, about a
hundred active. Most of them are payroll, statutory filings and HR data, so they cannot be
published. 29 of them are generic, and those are now open source.

https://github.com/Biribin/n8n-workflows

Six families, each with its own README explaining what every workflow does, its trigger and
its node count:

- **Job search automation**, the full chain: discovery, tailored writing, sending, follow-up,
  reading replies
- **Running n8n itself**: global error workflow, failed-deployment retry, API cost tracking,
  calling a coding agent CLI from a Code node
- **French regulatory watch**: Legifrance OAuth2 and vector ingestion, BOFiP monitoring,
  cross-checking a rate against an independent source
- **Applied AI**: review and comment replies with human approval, document classification
- **n8n patterns**: a web app with no browser JavaScript, a self-testing Postgres schema, a
  Graph utility
- **GDPR**: daily and monthly retention purges

## On the sanitisation, since that is the part people get wrong

I wrote two passes. The first was textual regexes over the exported JSON. It looked
thorough and it **missed a live OAuth client secret**, because n8n stores request parameters
as `{ "name": "client_secret" }` and `{ "value": "..." }` on separate JSON lines. No text
regex links a value to its name.

The second pass walks the parsed object instead, and it found 54 more hardcoded secret
values across the instance: 21 client secrets, 11 authorization headers, 8 client ids, 8 API
key headers. Then a third, independent script re-scans the output and exits non-zero if it
finds a secret, an email address, a phone number or an internal hostname. Nothing gets
published unless that script is silent.

Two things worth repeating if you ever share an export:

- **`pinData` is real data.** Pinned test data is production data you pasted once and forgot.
  34 of my workflows had some. Delete the key entirely.
- **A text-only scrub is not enough.** Walk the object.

Everything ships inactive, with stored connections stripped and hardcoded values replaced by
markers. Read before activating, particularly the ones that send email.

Happy to answer questions on any of them.
