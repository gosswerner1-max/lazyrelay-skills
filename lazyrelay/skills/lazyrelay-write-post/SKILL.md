---
name: lazyrelay-write-post
description: Use when the user asks for one social post for one platform ("write me a LinkedIn post about X"). Reads lazyrelay-brief.md, looks up the platform's own limits through LazyRelay, writes a hook, a short body and one call to action, and checks it with lazyrelay-grade-post before showing it.
---

# LazyRelay write post

You write one finished post. The user gets one version, not three.

## Before you write

1. Read `lazyrelay-brief.md` in the current folder. If it is missing, run `lazyrelay-start` first, without making a speech about it.
2. You need a topic and a platform. If either is missing, ask. Do not guess.
3. Call the LazyRelay tool `get_platform_rules` for that platform and use its limits (length, media, links). Never rely on limits you remember; platforms change.

## Write

1. **Hook first.** Write three openings, then keep the strongest. Start with something specific that makes people want to read on. Use a real number, a real moment or a real opinion from the brief. Cut filler like "really", "just" and "in today's world".
2. **Body.** One idea only. Short sentences. Say "you". Use details the user gave you. If you need a fact the user did not give, ask for it. Never invent a number, a customer, a result or a quote.
3. **One call to action** that matches the brief's one action and what the platform rewards (for example a question where people reply, a save line where people save).
4. **Plain style tips (the user can switch any of them off):** short sentences, contractions, active voice, digits for numbers, and no dashes (use a comma or a full stop). Use hashtags only where `get_platform_rules` or the user's own habit says they help.

## Check, then show

Run `lazyrelay-grade-post` on your draft. If the hook scores under 8 out of 10, rewrite the hook and grade again, at most twice. Then show:

- Platform
- The post text
- Score and one sentence on why it works
- Whether it fits the platform's limits (from `get_platform_rules`)

Ask: "Schedule it, edit it, or leave it?" Do not schedule anything yourself. Scheduling is the job of `lazyrelay-schedule`, and only after a clear yes.
