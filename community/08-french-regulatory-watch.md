<!-- Categorie : Show and tell. Image d'en-tete : capture des 4 workflows de veille, 1920x1080. -->

# Title

Regulatory watch for French payroll: Legifrance OAuth2, BOFiP, and cross-checking your own numbers

# Body

If you automate anything touching French payroll, your automation is only correct until a
rate changes. Four workflows keep mine honest. Two of them are useful to anyone touching
French law, regardless of payroll.

## Legifrance, the part nobody documents

Legifrance is served through the **PISTE** platform and needs OAuth2 client credentials. It
is not hard, but the shape is unusual enough that it cost me an afternoon, so here is a
working token node you can import:

`workflows/reglementaire-france/legifrance-obtenir-un-token.json`

It requests the token, caches it, and refreshes before expiry. Every other Legifrance call
starts here.

The companion workflow ingests collective agreements into a Supabase vector store with
pagination, chunking and idempotent writes, which turns a legal corpus into something you
can ask questions of:

`workflows/reglementaire-france/legifrance-ingestion-vers-base-vectorielle.json`

## The idea I would actually recommend copying

The last one is not French-specific and it is the one that has saved me. Every month it
**recomputes a statutory rate on an independent source** (OpenFisca) and compares it to what
my own system believes. Not to be clever: to catch the case where I mis-parametered
something and every payslip is quietly wrong.

Reading an official rate from one source proves nothing about whether you *applied* it
correctly. Computing it twice, from two independent implementations, and alerting on the
difference does. It is the cheapest correctness check I have, and it found a real
discrepancy.

`workflows/reglementaire-france/contre-calcul-sur-source-independante.json`

## Also included

A BOFiP page watcher that alerts when a new withholding-tax table is published, so the
parameter change happens before the payroll run rather than after it.

All in https://github.com/Biribin/n8n-workflows
