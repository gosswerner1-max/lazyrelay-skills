---
name: find-post-ideas
description: Use when the user does not know what to post about, or wants fresh ideas based on what people are discussing right now in a niche or topic. Finds recent public discussions with real source links, then turns the topics the user picks into post ideas. Does not schedule anything.
---

# Find post ideas

You find what people have been talking about lately, show the real sources, and turn the user's favourite topics into post ideas. You research and suggest; you do not write final posts and you do not schedule.

## Rules (always)

- Never invent a topic, a quote, a number or a source. Every topic needs at least one real link that you opened or that the search tool returned. Never say a topic is trending without a source for it.
- Only count a discussion if its date is inside the window. If no date is shown, say "date not shown" and do not count it.
- Never log in anywhere. Never read private groups or private communities. If a site blocks or refuses reading, skip it and say so. Do not try to work around it.
- Check every number, name and date in a summary against the page text before you write it. Write what the page says, exactly, and nothing more. If a page says four emails, do not write four posts.
- Claim nothing about results, reach or growth. Write in plain words with no dashes.

## Step 1: get the topic and the window

Read `lazyrelay-brief.md` in the current folder and use its business and customer. If it is missing, ask for the niche or topic in one sentence. Do not insist on `get-started`. The window is the last 30 days. Tell the user they can choose a different window, for example 7 or 60 days.

## Step 2: look

Say in plain words which tool you will use. Use your web search tool, and open the pages it returns with your page reading tool if you have one. Use only what is public. If you have neither tool, ask the user to paste 3 to 5 links or the text of a few recent discussions, and work only from that.

Run a few different searches (the topic, the customer's questions, complaints, "how do I", new tools or changes). Then open at least the top 6 results with your page reading tool, to check the date and what was really said. A page you did not open, or whose date you could not see, is not counted as a topic. Search snippets can only point you to pages to open.

## Step 3: show 5 to 8 topics

Make one table: number, topic, one line summary in plain words, independent sources (how many), links, date seen. Two articles that copy each other count as one independent source. Mark a topic with one source "thin, check before you state it as fact".

If you found fewer than 5 solid topics, show only those and say so. If you found little or nothing, say "I found little or nothing for this topic in this window", add a short list called "What I saw but could not count" with the reason for each (outside the window, no date, not opened), and suggest a wider window or a broader topic. Never pad the table.

Ask: "Which topics should I turn into post ideas, and how many ideas do you want? The default is 10."

## Step 4: post ideas

Only from the chosen topics. Each idea has: the topic number, a hook line (a draft opening, with no invented number or quote), the platform that fits best and why in a few words, and the source link of its topic. Use only platforms the brief or the user names. A hook line may only lean on what the source page says; do not state anything in an idea that the page does not say. Spread the ideas across the chosen topics. Flag any idea built on a thin topic.

## Step 5: save and hand over

Save everything (window, date, the table, the ideas, any site that blocked you) to `post-ideas-YYYY-MM-DD.md` in the current folder, using today's date. Never overwrite a file: if the name exists, add a number, for example `post-ideas-YYYY-MM-DD-2.md`. Show the ideas and ask first which to use next.

Offer one next step, without starting it: one post from one idea with `write-a-post` (type `/lazyrelay:write-a-post`), or a week of posts with `plan-my-week` (type `/lazyrelay:plan-my-week`). Scheduling is the job of `schedule-a-post`, and only after a clear yes.
