# LazyRelay skills for Claude

Free Claude skills (this pack has nine skills) that take you from "I do not know what to post" to a scheduled post, then show you which posts LazyRelay could confirm are live. They work through the LazyRelay connector, which is the link that lets Claude use your LazyRelay account. MIT licence.

LazyRelay (lazyrelay.com) is a social media scheduler. After it posts, it checks with the platform whether the post is really there, and you can share a public proof link for a post it confirmed.

## Install

**Using Claude Code (the version you run in a terminal window)?** Paste these two lines, one at a time, and press Enter after each. The first adds the pack's page, the second installs the pack:

```
/plugin marketplace add gosswerner1-max/lazyrelay-skills
/plugin install lazyrelay@lazyrelay-skills
```

The pack carries the address of the LazyRelay connector. If Claude Code asks you to sign in, do that. If it does not offer the connection, run this one line, then type `/mcp` and sign in:

```
claude mcp add --transport http lazyrelay https://mcp.lazyrelay.com/mcp
```

**Using the Claude app or website?** Open Customize, choose Connectors, press Add, then Add custom connector. Paste `https://mcp.lazyrelay.com/mcp` and sign in with your LazyRelay account. There is no key to copy.

Full guide: https://lazyrelay.com/mcp/claude/

No LazyRelay account yet? The Free plan is $0, gives you 30 new credits every month, and needs no card. It allows 10 posts waiting in the queue at a time, so plan small to start.

## What is inside

| Skill | Use it when | LazyRelay tools it uses |
|---|---|---|
| `get-started` | You are new, or do not know what to post. Saves a short brief | `list_connected_accounts`, `list_workspaces` |
| `find-post-ideas` | You do not know what to post, or want ideas from what people discussed in the last 30 days. It searches the public web, shows 5 to 8 topics with real source links, then turns the ones you pick into post ideas. It does not log in anywhere and does not schedule | none (uses your own Claude's web search, or links you paste) |
| `write-a-post` | You want one post for one platform | `get_platform_rules` |
| `grade-my-post` | You have a draft and want an honest score and three fixes | `get_platform_rules` |
| `plan-my-week` | You have one idea and want a week of posts | `list_posting_slots`, `get_next_free_slot`, `get_platform_rules` |
| `schedule-a-post` | A post is approved and should be scheduled | `list_connected_accounts`, `get_platform_rules`, `list_pinterest_boards`, `get_tiktok_creator_info`, `get_next_free_slot`, `schedule_post`, `schedule_posts`, `list_scheduled_posts`; `publish_post_now` only if you ask for it in words |
| `is-it-live` | Later, you want to see what was confirmed live, or want proof for a client | `list_scheduled_posts`, `get_proof_link`; `reschedule_post` only for a future time you approve |
| `post-my-clips` | You already have short video clip files from a clipping tool on your own computer and want them scheduled. It uploads each clip with your own API key, schedules one post per clip per connected account after you say yes, and later shows which were confirmed live. It does not cut video | `list_connected_accounts`, `get_platform_rules`, `get_tiktok_creator_info`, `get_next_free_slot`, `list_posting_slots`, `schedule_post`, `schedule_posts`, `list_scheduled_posts` |
| `client-approval` | A client should approve posts before they go out | `list_scheduled_posts`, `create_review_link`, `list_review_links`, `get_post_feedback`, `reply_to_post_feedback`, `approve_post`, `revoke_review_link`, `update_post` |

A good first session: ask Claude to "help me start with LazyRelay" (or type `/lazyrelay:get-started`), answer five questions, then ask for one post. Every skill can be typed as `/lazyrelay:` followed by its name, for example `/lazyrelay:post-my-clips`, or you can just describe what you want.

## Clips: set up a clipping tool and LazyRelay with one prompt

`post-my-clips` is the posting step only. A clipping tool on your own computer cuts your long video into short vertical clips. LazyRelay schedules the finished clip files and shows which ones it could confirm are live. Paste this into your own Claude Code (it is also saved as `lazyrelay/skills/post-my-clips/PROMPT.md`):

```text
I want to turn my long videos into short vertical clips on this computer and schedule them with LazyRelay. Set this up with me one step at a time. Before each step tell me in plain words what it does. Never ask me to paste a password or a key into this chat, and never show me a key. If a command fails, show me the exact message and check the tool's own README before trying anything else.

Step 1, a free clipping tool (you install it, I choose which). Tell me that the install downloads code from another project before you run it.
- Option A, the one I recommend: the video-to-shorts skill, MIT licence. Needs Node.js (for the install line), Python 3.8 or newer, ffmpeg and Pillow. Install it with: npx skills add social-media-skills/video-to-shorts-skill -g -a claude-code -y
  Then run its setup check and fix what it reports. On Windows use py -3 wherever its README says python3. Stop as soon as the finished clips are saved in the folder. Never run its posting step, never ask me for a key for any other scheduler, and never run its results read-back, because LazyRelay does the posting.
- Option B, ADVANCED: the ClipsAI Python library (github.com/ClipsAI/clipsai, MIT licence). It returns clip times and crop positions, not finished videos, so you write a script that cuts and crops each clip with ffmpeg and saves it as an mp4. It follows the install steps in its README (pip install clipsai, then WhisperX, libmagic and ffmpeg) and needs a free Hugging Face token. Tell me how to make one, step by step.
- If I want to pull a video from a link first, use yt-dlp (github.com/yt-dlp/yt-dlp) only if I ask for it, and install it the way its README says.

Step 2, LazyRelay. You cannot run slash commands, so tell me to type these two lines myself, one at a time, in Claude Code, and press Enter after each:
/plugin marketplace add gosswerner1-max/lazyrelay-skills
/plugin install lazyrelay@lazyrelay-skills
Then tell me to type /reload-plugins (or close and reopen Claude Code) so the new skills appear. Then ask me to type /mcp to check that lazyrelay is listed and signed in. If lazyrelay is not listed, you may run this line: claude mcp add --transport http lazyrelay https://mcp.lazyrelay.com/mcp and then ask me to type /mcp and sign in to my LazyRelay account in the browser. If I do not have a LazyRelay account yet, tell me to register at lazyrelay.com first and connect one social account.

Step 3, the upload key. The clip upload uses my own LazyRelay API key. Tell me to open More, then API Keys in my LazyRelay dashboard and create a key with the Safe setting. The dashboard shows the key only once, so I copy it when it appears. Then give me these steps one at a time. I do them myself in my own terminal window, not in this chat, and you never see the key:
- Windows: open PowerShell and run setx LAZYRELAY_API_KEY "the-key-goes-here" with my real key in place of the words. Or open the Start menu, search for "Edit environment variables for your account", press New, type LAZYRELAY_API_KEY as the name and paste the key as the value. Then I close the terminal and close Claude Code, and open both again.
- Mac: in Terminal run echo 'export LAZYRELAY_API_KEY="the-key-goes-here"' >> ~/.zshrc with my real key in place of the words. Then I close Terminal and Claude Code and open both again.
To check it, run [ -n "$LAZYRELAY_API_KEY" ] && echo set || echo not set (in PowerShell: if ($env:LAZYRELAY_API_KEY) { "set" } else { "not set" }). It prints only "set" or "not set". Never print the key itself.

Step 4, join them. Make one folder called clips-out. Set the clipping tool to save its finished clip files there. When I say "clips are ready", run the post-my-clips skill (the command is /lazyrelay:post-my-clips) on clips-out: check each file, upload it with my key, write a caption for each platform from what I tell you about the clip, show me one table with the clip, platform, account, time and caption, and wait for my clear yes before anything is scheduled. Every time must be at least 10 minutes from now. Later, when I ask, run is-it-live (the command is /lazyrelay:is-it-live) to show which posts LazyRelay confirmed live.

Start with step 1 and ask me which option I want.
```

The two clipping tools named in the prompt are separate projects with their own licences and requirements: github.com/social-media-skills/video-to-shorts-skill and github.com/ClipsAI/clipsai, both MIT. The prompt installs nothing until you choose.

## Renamed from 0.1.1

Version 0.2.0 dropped the "lazyrelay-" prefix from every skill name, because the plugin is already called `lazyrelay`. If you installed 0.1.1, update the pack and use the new names:

| Old name (0.1.1) | New name |
|---|---|
| `lazyrelay-start` | `get-started` |
| `lazyrelay-write-post` | `write-a-post` |
| `lazyrelay-grade-post` | `grade-my-post` |
| `lazyrelay-plan-week` | `plan-my-week` |
| `lazyrelay-schedule` | `schedule-a-post` |
| `lazyrelay-check-live` | `is-it-live` |
| `lazyrelay-client-review` | `client-approval` |
| `lazyrelay-clip` (new in 0.2.0) | `post-my-clips` |

The saved brief keeps its file name, `lazyrelay-brief.md`.

## Things to know

- **What "confirmed live" means.** On most platforms LazyRelay asks the platform for that exact post, using the post's own number, and sees whether it is there. On Telegram and LinkedIn the platform does not allow that, so LazyRelay only confirms that the channel (Telegram) or the public page (LinkedIn) answers. Confirmed live is about the post existing on the platform. Nobody can promise how many people will see it.
- **Credits.** One credit pays for one post on one account, so one post to 3 accounts is 3 credits. A credit is taken only after the platform confirms the post is live, and if one account fails you are not charged for it.
- **What the skills do.** They ask you before anything goes public, schedule for a future time by default, and never delete posts. They refuse a past time and show you the go-out time before approving a post.
- **The connector has full access.** Claude can do anything your LazyRelay account can do with posts, including publishing now and cancelling a waiting post, once you have signed in. If you want a tighter setup, use the local package with a limited key from lazyrelay.com/mcp/claude/.
- **Proof links and review links are public.** Anyone who has the link can open it. Share it only with the people who should see it.
- **Your data.** The brief and plans are plain files in the folder you work in. What Claude reads from them becomes part of your chat with Claude, and any other connector you have turned on can see that chat.
- **Uploads.** `post-my-clips` uploads each clip with your own API key. LazyRelay deletes an upload after the post that uses it has been sent, and after 7 days if no post uses it. LazyRelay is a posting service and does not keep a file library.
- **Questions.** Open an issue on this repository or email support@lazyrelay.com.

## Licence

MIT, copyright IPE Projects (Pty) Ltd. See `LICENSE`.
