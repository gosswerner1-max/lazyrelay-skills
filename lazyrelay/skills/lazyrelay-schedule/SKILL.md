---
name: lazyrelay-schedule
description: Use when the user has approved a post and wants it scheduled to one or more platforms with LazyRelay. Runs a last check, shows a preview, and only after a clear yes schedules it for a future time. Never publishes immediately and never deletes unless the user says so in words.
---

# LazyRelay schedule

You turn an approved post into a scheduled post. You are careful: this is the step where something goes public.

Tell the user in plain words what you are doing at each step, for example "checking which accounts you have connected".

## Safety rules (always)

- Never call `publish_post_now` unless the user says "post now" or "publish now" in those words. The default is a scheduled time in the future.
- Never delete anything from this skill.
- Always show the exact text, the platforms and the time, and wait for a clear yes before scheduling.
- If a tool answers with an error, show the exact error text. Do not retry in a loop. **Before any second attempt**, call `list_scheduled_posts` and look for the post: an error can arrive after the platform already took the post, and sending again would put it up twice.

## Steps

1. **Inputs.** The approved text, the platform or platforms, and the time. Ask which time zone the user means if it is not clear. No time given means the next free slot (`get_next_free_slot`). Ask for any missing platform; never guess one.
2. **Accounts.** Call `list_connected_accounts` and match each platform to an account. If a platform has two accounts, ask which. If it has none, say so and stop for that platform.
3. **What each platform needs first.**
   - Pinterest needs a board. Call `list_pinterest_boards` and ask which board.
   - TikTok needs creator settings checked first. Call `get_tiktok_creator_info` and use what it says. Do not guess privacy settings.
   - Call `get_platform_rules` for every platform. Check length, media and required fields. If the text is too long, do not cut it yourself: offer to shorten it, skip that platform, or stop.
4. **Free plan check.** If the user is on the Free plan (30 new credits every month, 10 posts waiting in the queue at a time), count what you are about to schedule. Each post on each account is one credit. If it passes the queue limit or the credits left, say so before scheduling and let the user choose what to drop.
5. **Final check of the wording.** No dashes of any kind, no invented number or claim, hashtag count fits the platform. If something fails, report it and ask. Do not silently fix it.
6. **Preview.** Show a table: platform, account, time, text, media. Ask: "Schedule this?"
7. **After a clear yes**, call `schedule_post` (one post) or `schedule_posts` (several). Report each result with its post id and time. If three of five worked, say which three and which two, and why.

## Credits, in plain words

One credit pays for one post on one account. A credit is taken only after the platform confirms the post is live, and if one account fails you are not charged for it. Say this once if the user asks about cost; do not make promises beyond it.

## After scheduling

Tell the user: "I can check later which posts are confirmed live. Ask me to run lazyrelay-check-live after the scheduled time." Do not poll in a loop.
