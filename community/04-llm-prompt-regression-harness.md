<!-- Categorie : Tutorials. Image d'en-tete : capture du verdict et de la liste des controles, 1920x1080. -->

# Title

How do you know your prompt did not regress? A 15-assertion test harness as an n8n workflow

# Body

Everyone edits production prompts by hand and hopes. I got tired of hoping, so I built a
workflow whose only job is to try to break my agent.

The agent in question drafts follow-up emails for job applications. Those emails go to real
recruiters, so a bad one is not a rendering glitch, it is a burnt opportunity.

## The idea

The harness replays the **real production prompt**, not a simplified copy, against a fixed
context, then runs 15 deterministic assertions on the answer. A helper does all the work:

```js
function verifier(condition, libelle) { if (!condition) violations.push(libelle); }
```

Then, one line per rule. The actual list, so you can see the shape:

- subject at most 60 characters, and not prefixed with "Re:" or "Follow-up:"
- body at most 90 words, at most 3 paragraphs excluding the sign-off
- closing formula present
- no em dash and no en dash anywhere, which is my personal tell for unedited AI text
- no leftover markdown, no HTML tag
- the company name and the job title must both actually appear in the body
- the body must end with the candidate's name
- **no email address or phone number added by the model**
- **no invented seniority**: any match on "two years", "a year and a half", "years of
  experience" fails the run
- **no invented past interaction**: any match on "interview", "our call", "our exchange"
  fails the run

## Why deterministic assertions beat an LLM judge

No second model, so no second source of randomness. String and structure checks are free,
instant, and catch exactly the failures that damage you: a hallucinated phone number, a
fabricated previous conversation, an invented three years of experience. Those are not
matters of taste, they are lies, and a regex catches them every time.

The last two assertions exist because the model produced both mistakes on real output.

## The limit, stated plainly

This catches rule violations, not quality drops. A blander but rule-abiding email passes.
For quality you still need a human read or a scored eval set. The point is that the stupid,
embarrassing regressions get caught for free, before anything is sent.

The harness also shows how to call a coding agent CLI from a Code node, which I wrote up
separately.

Workflow JSON:
`workflows/recherche-emploi/harnais-de-test-agent-llm.json` in
https://github.com/Biribin/n8n-workflows
