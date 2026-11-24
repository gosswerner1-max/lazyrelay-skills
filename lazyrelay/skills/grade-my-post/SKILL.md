---
name: grade-my-post
description: Use when the user pastes a draft social post and asks if it is good, or when write-a-post needs a quality check. Scores the draft out of 10 and lists the three fixes that would raise the score most. Never rewrites the post.
---

# LazyRelay grade post

You grade. You do not rewrite. Be honest: a 7 is good and an 8 is strong.

## Get the inputs

The draft text and the platform. If the platform is missing, ask. Read `lazyrelay-brief.md` if it exists, to judge the voice. Call `get_platform_rules` for the platform to check length, media and links.

## Score each part from 1 to 10

| Part | Weight | What you look for |
|---|---|---|
| Hook | 50% | Would the first three to five words stop a scroll? Specific, surprising or a real opinion, or throat clearing? |
| Specificity | 15% | Real numbers, names and moments, or vague statements? |
| Reason to share or reply | 15% | Would someone save it, send it, or answer it? |
| Voice | 10% | Does it sound like the brief, or like anyone? Skip and say so if there is no brief |
| Platform fit | 10% | Inside the limits from `get_platform_rules`, right format, hashtags only where they help |

This is a simple scoring guide, not a measure of how many people will react.

Style tips are separate and optional: a dash, a missing contraction, a number written out as a word, passive voice, filler words, a filler opening. Mention them as tips. Take 0.5 off per tip only if the user asked for this house style, and never more than 3 in total.

## Report

1. The total, to one decimal, and a small table of the five scores with a one line note for anything under 8.
2. **Top 3 fixes**, biggest effect first. For each: the exact line, why it hurts, and a specific change (for example, "Replace the opening 'In today's world of small business' with a real moment from the brief, in your own words.").
3. Ask: "Want me to apply these, or will you edit it yourself?"

Do not pad scores. A weak hook is a weak post, and the fix is nearly always to rewrite the hook.
