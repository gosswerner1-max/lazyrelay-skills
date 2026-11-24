---
name: is-it-live
description: Use after the scheduled time, when the user asks "did it go out", "is it live" or wants proof for a client. Lists the posts with their status and whether LazyRelay confirmed each one live, explains what that check means on each platform, and gives the public proof link for confirmed posts.
---

# LazyRelay check live

"Sent" and "live" are not the same thing. You show the difference, honestly. Tell the user in plain words what you are doing, for example "looking up your scheduled posts".

First use of the words, say this once: "Confirmed live means LazyRelay asked the platform and the platform says the post exists."

## Steps

1. Call `list_scheduled_posts`. Filter by status when the user is interested in one group (posted or failed). This list shows the 50 most recent posted or failed posts plus upcoming ones. If the user is looking for something older, say so.
2. Show a table: platform, scheduled time, status, confirmed live (yes, no, not yet as the list shows it), and any failure reason the list gives. Do not invent a value the list does not give.
3. For each post confirmed live, offer the public proof link with `get_proof_link`. Say that the proof link is public: anyone with the link can open it, so share it only with people who should see it. If the user connected with an API key, the key needs the tick "Allow this key to generate public proof-sharing links"; signing in through the connector has no such setting. If the tool refuses, show the message.

## What "confirmed live" means, in plain words

Use this wording and do not stretch it:

- On most platforms LazyRelay asks the platform for that exact post, using the post's own number, and counts it as live only when the platform says it is there.
- On Telegram and LinkedIn the platform does not let LazyRelay do that. There it confirms that the channel (Telegram) or the public page (LinkedIn) answers. That is a weaker check, and a post removed straight after sending could still show as confirmed.
- Confirmed live means the platform reports the post exists. Nobody can promise how many people will see it, so never say or imply reach.

## When a post failed

1. Show the exact reason from the list.
2. Tell the user to look at the platform first. Some failures happen after the platform already accepted the post, and sending it again would put it up twice.
3. Retrying is a decision for the user in the LazyRelay dashboard (Failures screen). Do not re-send from here.
4. You may offer `reschedule_post` for a post that is still waiting, but only for a time in the future that the user names. Refuse a time that is now or in the past: it would publish at once. Show the exact new time and wait for a yes.

## Close

Offer one follow up: a short summary the user can paste to a client, written from the real results in the list, in this shape: "[number] of [number] scheduled posts confirmed live on [date], links below". Only use real results. Never round up, never invent numbers.
