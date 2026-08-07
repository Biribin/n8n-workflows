<!-- Categorie : Tutorials. Image d'en-tete : capture du canevas, 1920x1080. -->

# Title

Calling the Claude Code CLI from inside an n8n Code node

# Body

Most "AI in n8n" setups call a model API and get text back. That works until the task is
*do something to these files*: read a repo, patch three of them, run the tests, report.
An API call cannot do that. A coding agent CLI can.

This workflow spawns the Claude Code CLI from a Code node via `child_process` and returns
its output into the flow like any other node.

## The one setting that makes it possible

By default n8n forbids Node built-ins inside Code nodes. You need:

```
NODE_FUNCTION_ALLOW_BUILTIN=child_process
```

Set it on your n8n container and restart. Without it the Code node throws on `require`.

## What to watch out for

- **It is not instant.** An agent run takes tens of seconds to minutes. Raise the node
  timeout and treat the call as a long job, not a lookup.
- **Working directory matters.** The CLI acts on files relative to where it runs. Pass an
  explicit path rather than relying on the container default.
- **Cost is per run, not per node.** An agent invoked in a loop over 200 items is a very
  expensive loop. Put it behind a filter.
- **Self-hosted only.** n8n Cloud does not let you spawn processes.

## Why it is worth the trouble

Once the agent is a node, you can chain it: a webhook receives a bug report, the agent
opens the repo and proposes a patch, a human approves in a form, another node opens the
pull request. That whole chain lives in n8n.

Workflow JSON:
`workflows/exploitation-n8n/appeler-claude-code-cli-depuis-n8n.json` in
https://github.com/Biribin/n8n-workflows

Every stored connection is stripped and every hardcoded value is replaced with a marker,
so read it before activating.
