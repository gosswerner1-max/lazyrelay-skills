---
name: plan-my-week
description: Use when the user has one idea, article or announcement and wants a week of posts from it. Builds a plan with one post per posting slot across the platforms they choose, using their LazyRelay posting slots, and saves it to week-plan.md. Does not schedule anything.
---

# LazyRelay plan week

One idea, a week of different posts. You plan; you do not schedule.

## Inputs

1. The idea or source text, from the user.
2. Which platforms to post to. If they do not know, suggest at most two, based on `lazyrelay-brief.md` (where the customer is). Do not suggest more than three.
3. How many posts a day they can keep up with. Default to one a day.

## Look up the timing

- Call `list_posting_slots` to see the user's saved slots.
- Call `get_next_free_slot` for the first free time.
- Call `get_platform_rules` once per chosen platform and use its limits.

## Build the plan

Make a table: day, time, platform, angle, hook, one call to action, media needed (yes or no).

- Seven days is the most. Fewer is fine if the idea is thin; say so instead of padding.
- Every post has its own angle: a number, an opinion, a mistake, a story, a question. Do not reuse the same text on several days.
- **Count before you promise.** Each post on each account is one credit, and the Free plan allows 10 posts waiting in the queue at a time. Add a line under the table: "This plan is [number] posts, about [number] credits." If it is over the Free limits, say so and suggest cutting platforms or days.
- Do not put posts on two platforms in the same minute with the same words.
- If a post needs a picture or video the user has not given you, mark it "media needed" and do not pretend it is ready.

## Save and hand over

Save the table to `week-plan.md` in the current folder. Show it. Ask which posts to write first. Writing is the job of `write-a-post` and scheduling is the job of `schedule-a-post`. Do not start either without a yes.
