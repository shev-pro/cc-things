# cc-things

A small Claude Code marketplace with one plugin so far: `thinking-tools`.

Three skills inside, each solving a specific problem that kept coming up.

## Install

Add the marketplace from this repo, then install the plugin:

```
/plugin marketplace add shev-pro/cc-things
/plugin install thinking-tools@shevpro-cc-things
```

The `ask-codex` skill needs the Codex CLI installed and authenticated separately:

```
npm install -g @openai/codex
codex login
```

The other two skills have no extra dependencies.

## What's in thinking-tools

### ask-codex

Sometimes you're 4 attempts deep into a bug and every fix makes it worse. This skill hands the problem to OpenAI Codex in read-only mode so it can stare at the code with fresh eyes and tell you what you missed. Slow — 2 to 5 minutes per consult — so it's not a default move. It's the "I'm stuck, give me a second opinion" button.

Codex analyzes. You implement.

Invoke it with phrases like "ask codex", "check with codex", or "codex review this".

### dialectic

Run two agents in parallel on the same claim: one looking for evidence it's true, one looking for evidence it's false. Then synthesize. The point is to kill the confirmation bias you get when you ask a single agent "is this a good idea?" and it dutifully agrees with you.

Good for: architecture decisions you're about to commit to, performance claims someone made in a meeting, "is this thread-safe" questions, code reviews where you want both sides argued before deciding.

Invoke with `/thinking-tools:dialectic <claim to stress-test>`.

### write-as-human

Strips the statistical patterns that make a text sound like an LLM wrote it. Parallel triples, closing paragraphs, universal hedges, forced on-one-hand symmetry, all that. It's not a voice skill — it works regardless of tone. It just pushes the text away from the boring statistical center.

Use when you've drafted something with AI help and it reads like a middle-school essay. Trigger it with "humanize this", "de-AI this text", or `/thinking-tools:write-as-human`.

## Structure

```
.claude-plugin/marketplace.json   # marketplace manifest
plugins/
  .claude-plugin/plugin.json      # plugin manifest
  thinking-tools/skills/
    ask-codex/
    dialectic/
    write-as-human/
```

## License

MIT.
