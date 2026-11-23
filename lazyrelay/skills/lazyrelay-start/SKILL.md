---
name: lazyrelay-start
description: Use when someone wants to start posting with LazyRelay, says they do not know what to post, or has just installed this pack. Checks the LazyRelay connection, shows the connected accounts and brands, and saves a short brief that the other skills read.
---

# LazyRelay start

You get a new user from "I installed this" to a saved brief in a few minutes. Be warm and plain. One question at a time.

## Step 1: check the connection

Tell the user in plain words what you are doing ("checking which social accounts you have connected"), then call the LazyRelay tool `list_connected_accounts`.

- If it answers, show the accounts in a short table (platform, display name). If an account says it needs reconnecting, say so and tell the user to reconnect it in the LazyRelay dashboard.
- If the tool is missing or refuses, the connector is not set up. Tell the user, in these words:
  1. In the Claude app, open Customize, choose Connectors, press Add, then Add custom connector, and paste `https://mcp.lazyrelay.com/mcp`. You sign in with your LazyRelay account in the browser. Nothing to copy, no key.
  2. In Claude Code, run `claude mcp add --transport http lazyrelay https://mcp.lazyrelay.com/mcp`, then run `/mcp` and sign in.
  3. No LazyRelay account yet? The Free plan is $0 and gives you 30 new credits every month, and no card is needed with the free plan. Register at lazyrelay.com, connect one social account, then come back.
- Never ask the user to type a password or a key into the chat. Sign in happens in the browser.

## Step 2: pick the brand

Call `list_workspaces`. If there is one brand, use it. If there are several, ask which one this brief is for.

## Step 3: ask five questions, one at a time

1. What do you sell or do, in plain words?
2. Describe one real person who buys from you.
3. What is the one thing you want a reader to do after seeing a post?
4. What is one opinion you hold that others in your field would argue with?
5. How should your posts sound? Pick two or three words, for example friendly, direct, funny, calm.

If an answer is one word, ask one gentle follow up. Do not ask more than five questions.

## Step 4: save the brief

Write `lazyrelay-brief.md` in the current folder with these headings: Business, Customer, One action, Opinion, Voice, Brand (the LazyRelay brand name), Accounts (the ones to post to). Show it and ask if anything should change. Do not invent anything the user did not say; write "not given" instead.

## Step 5: next step

Offer one next step: write a first post with `lazyrelay-write-post`, or plan a week with `lazyrelay-plan-week`. Do not offer both and do not start without a yes.
