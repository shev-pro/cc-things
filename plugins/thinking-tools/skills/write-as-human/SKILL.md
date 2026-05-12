---
name: write-as-human
description: Rewrites text so it doesn't sound like an LLM wrote it. Use when the user asks to humanize, de-AI, or strip the AI sheen from a text.
---

# Write as human

Rewrites a text so it doesn't sound like it was written by an LLM.

This isn't a voice skill. It's a statistical cleanup skill: it strips the patterns that make a text sound "AI-ish" regardless of tone.

## Why the markers exist

Models generate the most probable continuation given the training corpus distribution. Result: they drift to the statistical center — average sentences, balanced structures, safe vocabulary. Post-training (RLHF) strips "risky" patterns and amplifies neutral ones. So the markers aren't a bug: they're the expected behavior. To remove them you have to push the text *away* from the statistical center.

## The 10 markers to strip

### 1. Parallel triples

Pattern: "X, Y, and Z". Like "fast, scalable, and secure", "design, development, and support", "flexibility, quality, and speed".

What to do: use pairs ("fast and secure") or a single concept. If you really need three things, split them into separate sentences of different lengths.

### 2. Closing paragraph

Pattern: "In conclusion…", "In summary…", "To wrap up…", "All in all…".

What to do: end on the last substantive thing you said. If it feels like a closing is missing, usually it isn't — that's a school-essay reflex.

### 3. Universal hedges

"It's worth noting", "it's important to understand", "it's good to remember", "obviously", "undoubtedly", "naturally", "clearly".

What to do: delete them. If the sentence stands without them, they weren't needed. If it doesn't, rewrite the sentence — usually the real problem was something else the hedge was hiding.

### 4. Cliché openings

"In today's world", "in the age of digitalization", "it's no secret that", "as we all know", "we live in a time when".

What to do: start with a concrete fact, a number, an example, a scene. No pamphlet-style preambles.

### 5. Forced "on one hand / on the other" symmetry

On any topic, AI tends to give the balanced version: pros, cons, neutral conclusion. It sounds even-handed, but it's the most visible marker of all because a real author almost always has an opinion.

What to do: take a position. If you really can't, at least give the two sides different weight — not 50/50, but 70/30. Unbalance it.

### 6. Over-perfect syntax

AI sentences are grammatically flawless and all well-constructed. But real people occasionally break syntax for emphasis.

What to do: break it sometimes. A one-word sentence. A comma instead of a conjunction. An ugly but alive parenthetical. A sentence starting with "but" or "and".

### 7. Uniform lengths

AI paragraphs are all roughly the same — 3-5 sentences, rarely more or less. Sentences too tend to all run similar lengths.

What to do: vary. A one-sentence paragraph. A long one. A medium one. Variance is more human than balance.

### 8. Filler metaphors

"Opens up new possibilities", "plays a key role", "represents an important step", "takes things to the next level", "makes a difference".

What to do: replace them with concrete things — numbers, names, events, specific examples. If you have nothing concrete to say in place of the metaphor, cut the sentence: it wasn't saying anything.

### 9. Paragraph-opening connectors

"Therefore", "in this sense", "in this context", "that said", "in light of the above".

What to do: delete them. If the logical link isn't clear without the connector, the problem isn't the missing connector — it's that the paragraph's first sentence needs to be rewritten so the link is clear from the content.

### 10. Hedges on solid facts

"Python can potentially be used for the web". "React is generally used in production". "Postgres is usually considered reliable".

What to do: if the fact is true, say it flat. "Python is used for the web". "React is used in production". "Postgres is reliable".

## How to apply the skill

When you're given a text to clean up:

1. Read it once. Mentally mark: where it opens, where it closes, where there are triples, hedges, filler metaphors, forced symmetries.
2. Cut everything that doesn't carry information — especially closings and openings.
3. Break the regularity: paragraphs of different lengths, sentences of different lengths, occasionally dirty syntax.
4. Replace generic metaphors with concrete things. If you have nothing concrete, cut.
5. Unbalance: where there's forced symmetry, take a position or at least give the sides different weight.
6. Reread. If it still sounds "AI-ish", it's almost always because it's still too balanced. Unbalance it more.

Return the rewritten text. Don't add preambles like "here's the cleaned-up version" or explanations of what you removed, unless the user explicitly asks.

## Before/after examples

**Example 1 — product description**

Before:
> In today's world, effective data management plays a key role for any modern company. Our tool offers flexibility, scalability, and reliability, opening up new possibilities for data-driven teams. It's important to note that it integrates seamlessly with the leading tools on the market. In conclusion, it represents a solid choice for those seeking a reliable technology partner.

After:
> The tool handles structured datasets up to roughly 10 million rows without scaling horizontally. It integrates with Postgres, Snowflake, and BigQuery through native connectors. For larger volumes you need a different architecture, and in that case we're not the right tool.

**Example 2 — technical article**

Before:
> When it comes to choosing a programming language for the backend, there are several important considerations to keep in mind. On one hand, Python offers simplicity, readability, and a vast ecosystem. On the other, Go provides performance, native concurrency, and compact binaries. The choice obviously depends on the specific context. In summary, both are valid options that deserve consideration.

After:
> Python and Go do different things. You pick Python if the bottleneck is time-to-market and you're fine paying in performance and concurrency tooling. You pick Go if you have serious workloads or services that need to handle thousands of connections with a small footprint. For most normal web projects, Python is more comfortable. For infrastructure, Go almost always wins.

**Example 3 — internal communication**

Before:
> Hi team, I wanted to share some thoughts on our current review process. On one hand, we've made significant progress in recent months. On the other, there are still some areas that deserve attention. It's important to recognize everyone's hard work, but at the same time we need to be honest about what isn't working. In conclusion, I propose we organize a workshop to align on next steps.

After:
> The review process isn't working. PRs sit for an average of 4 days, and in three out of ten cases they close without being really read. One-hour workshop this week to decide how to change it. I'll send a Slack poll tonight.

**Example 4 — tech blog post**

Before:
> In recent years, artificial intelligence has opened up new possibilities in many sectors. There's no denying that its adoption is transforming the way we work, communicate, and create. However, it's equally important to consider the limits of these technologies. In this context, it's essential to adopt a balanced approach. Therefore, we propose some reflections on the topic below.

After:
> Generative AI is useful for specific things and useless for others. For writing a first draft of an email it works well. For serious numerical reasoning, no, and it probably never will with these models. Below you'll find five cases where we use it in production and three where we tried it and dropped it.

## What NOT to do

- Don't add fake "humanity markers" — forced typos, "like", "I mean", staged interjections. It shows.
- Don't rewrite too much. If the original text had good content under the fluff, keep the content and change the form. If it had no content, tell the user instead of inflating it.
- Don't add information that wasn't there. Unbalancing the text doesn't mean inventing an opinion it didn't have — it means stripping the surface-level symmetry.
- Don't replace an AI cliché with a blog cliché ("game changer", "no-brainer", "pro tip"). It's just another kind of filler.

## Final rule

A human text isn't "well-written in an average way". It's specific, unbalanced, irregular, opinionated. The more your text looks like a good middle-school essay, the more it sounds like it was written by an LLM.
