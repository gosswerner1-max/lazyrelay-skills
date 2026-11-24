---
name: lazyrelay-clip
description: Use when the user already has short video clip files (made by any clipping tool on their own computer) and wants them scheduled with LazyRelay. Checks each clip file, uploads it through the user's own LazyRelay API key, writes a caption per platform, shows a table and waits for a clear yes, schedules one post per clip per connected account, and later reads back which posts LazyRelay confirmed live.
---

# LazyRelay clip

This skill is the posting step for clips. The user makes the clips with a clipping tool on their own computer. You take the finished clip files and get them scheduled through LazyRelay. This skill does not cut, download, transcribe or reframe video, and you never say it does. It only handles the posting. If the user has no clips yet, say that a clipping tool has to make them first, point to the ready-made prompt in `PROMPT.md` in this skill folder, and stop.

Tell the user in plain words what you are doing at each step, for example "checking the clip files" or "uploading the first clip".

## Rules (always)

- Show the user the clips, captions, accounts and times in one table and wait for a clear yes before anything is scheduled.
- Schedule for a future time only. Never call `publish_post_now` unless the user says "post now" or "publish now" in those words. Never delete anything.
- Never print, show or echo the API key. Never ask the user to paste or type the key into this chat. The key lives in an environment variable named `LAZYRELAY_API_KEY` that the user sets on their own computer.
- Captions say only what the clip says or what the user told you about it. Never invent a quote, a number, a result or a promise about views, reach or followers.
- Use only the platforms the user's account has connected. Do not suggest connecting others.
- If a tool answers with an error, show the exact text. Do not retry in a loop. Before any second attempt, call `list_scheduled_posts` and look for the post, because an error can arrive after the post was accepted.

## Step 1: find the clip files

Ask for the folder the clipping tool saved the clips in (for example `clips/`). List the video files in it (`.mp4` is the safest; `.mov` and `.webm` also work on some platforms). Show the list and ask which ones to schedule. If the folder holds a title or caption file for each clip, read it and start from it. Never open or change the user's original long video.

## Step 2: check each clip file

For each chosen clip run (needs `ffprobe` and `ffmpeg`, which come with the user's ffmpeg install; if they are missing say so and ask the user to install ffmpeg first):

```bash
ffprobe -v error -select_streams v:0 -show_entries stream=codec_name,width,height:format=duration,size -of json clips/clip-01.mp4
ffmpeg -v info -i clips/clip-01.mp4 -t 1 -vf blackdetect=d=0.5:pix_th=0.10 -an -f null - 2>&1 | grep black_start
```

Report per clip: length, size, width by height, and whether the first second is black (the second command prints a `black_start` line when it is; in PowerShell use `Select-String black_start` instead of `grep black_start`). A black opening is a warning, not a block: tell the user a scroller often leaves in the first second and ask whether to go on. Open the first frame as a picture and look at it:

```bash
ffmpeg -y -v error -ss 0 -i clips/clip-01.mp4 -frames:v 1 clips/clip-01-first.png
```

Then call `get_platform_rules` for each connected platform that takes video and compare the clip with its `video` formats, `maxSizeMb` and `maxDurationSec`. Never rely on limits you remember. A clip that breaks a platform's limit is left off that platform, and you say why.

## Step 3: connection and accounts

1. Call `list_connected_accounts`. Match each platform that takes video to an account. If a platform has two accounts, ask which. If an account needs reconnecting, tell the user to do that in the LazyRelay dashboard.
2. Skip, and say so, any platform whose rules show no video, and any platform whose `required` list asks for more than the account, text, time and media (the user can post those from the dashboard).
3. Free plan check: one credit is one post on one account, and the Free plan allows 10 posts waiting in the queue at a time. Count clips times accounts before you upload anything and say the total. If it is over, let the user choose what to drop.

## Step 4: upload each clip

The upload uses the user's own API key, from the dashboard under More, then API Keys. A key made with the Safe or Full setting can upload; a Read only key cannot. If `LAZYRELAY_API_KEY` is not set, tell the user how to create a key and set the variable themselves, in plain steps for their system, and wait. Do not look for the key in files. If the user will not use a key, they can instead give you a public link to each clip that they host themselves, and you use that link as the media link in Step 6.

Upload one clip (on Windows PowerShell write `curl.exe` and `$env:LAZYRELAY_API_KEY`):

```bash
curl -sS --fail-with-body -X POST -H "Authorization: Bearer $LAZYRELAY_API_KEY" -F "file=@clips/clip-01.mp4" https://lazyrelaylazyrelay-backend.onrender.com/api/media/upload
```

The answer has an `id` and a public `url`. Keep the `url` for that clip and use the same link for every post of that clip. A refused upload shows its reason in plain words: show it as it is. A key without the right scope answers with an `api_key_scope` error: say which setting the key needs.

LazyRelay deletes an upload after the post that uses it has been sent, and after 7 days if no post uses it. So upload right before you schedule, and do not upload clips you are not going to schedule.

## Step 5: captions

Write one caption per clip for each platform you will post to, from the user's own words about the clip. Use the text limit from `get_platform_rules` for that platform. Keep the same idea but do not paste identical text on every platform. No dashes of any kind, no invented claims. For YouTube also write a title under its title limit.

For each platform the rules call for extra settings, ask the user once:

- TikTok: call `get_tiktok_creator_info` for that account and let the user pick one of the privacy levels it returns. Never guess it.
- YouTube: `options.youtube.privacy` (public, unlisted or private) and the title.
- Instagram: whether the single video goes out as a Reel or a Story (`options.instagram.placement`).
- Facebook: feed or story (`options.facebook.placement`).

## Step 6: show the table, then schedule

Call `get_next_free_slot` for each account (and `list_posting_slots` if the user wants their saved times). Ask which time zone the user means if it is not clear. Spread the posts: different clips at different times, and do not put the same clip on two platforms in the same minute with the same words.

Show one table: clip file, length, platform, account, time (with time zone), caption, and the upload link. Ask: "Schedule these?" Only after a clear yes call `schedule_posts` (up to 50 posts per call, a post to 3 accounts counts as 3) with an `idempotencyKey`, or `schedule_post` for a single post. Give every post the clip's `mediaUrl`. Report each result with its post id and time. If three of five worked, say which three and which two, and why.

## Step 7: read back which posts are live

Tell the user: "Ask me to run lazyrelay-check-live after the scheduled times." When they do, call `list_scheduled_posts` and show per post whether LazyRelay confirmed it live, using the same plain wording as `lazyrelay-check-live`: confirmed live means the platform reports the post exists. Do not poll in a loop and do not promise views.
