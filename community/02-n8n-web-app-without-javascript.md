<!-- Categorie : Tutorials. Image d'en-tete : capture des pages rendues, 1920x1080. -->

# Title

A real internal web app served entirely by n8n: no framework, no build, no hosting

# Body

I needed an internal absence-request app, and I was not allowed to host anything new. So I
built it out of n8n webhook nodes returning HTML. No framework, no build step, no
deployment, nothing to maintain besides the workflow itself.

To be precise about the "no JS" in my workflow name: **there is no framework and no bundle,
and every form works with JavaScript disabled.** There is a small inline script on the table
view for column sorting and date formatting. That is progressive enhancement, not the app's
foundation.

## How it works

- One `Webhook` node per screen, `respondToWebhook` returning `text/html`.
- Plain HTML `<form method="POST">`. The browser does the submitting, so no fetch and no
  client-side state machine.
- **All state lives in the URL.** No session, no cookie. A screen is a pure function of its
  query string, so the back button, refresh and bookmarking work for free.
- After a POST, respond with a **303 redirect** to a GET URL. Skip that and a refresh
  resubmits the form, and you get duplicate records.
- Rows are injected into the page as JSON in one template literal, then the little sorting
  script reads them client-side. Sorting without a round trip, still one request per page.

## What you gain

- Works on any browser that renders HTML, including the locked-down ones you cannot update.
- Nothing to deploy. The app *is* the workflow, versioned and exported with it.
- No CORS, no API key in the frontend, no dependency to patch.

## What you give up

- Every navigation is a full page load. Fine for forms, wrong for a live dashboard.
- Building layout in HTML strings gets unpleasant past a handful of screens. Keep the CSS in
  one sticky note and inject it everywhere.
- No client-side validation, so validate server-side. You had to anyway.

Workflow JSON:
`workflows/motifs-n8n/interface-web-n8n-sans-javascript.json` in
https://github.com/Biribin/n8n-workflows
