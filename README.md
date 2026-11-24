# LazyRelay skills for Claude

Free Claude skills (this pack has eight skills) that take you from "I do not know what to post" to a scheduled post, then show you which posts LazyRelay could confirm are live. They work through the LazyRelay connector, which is the link that lets Claude use your LazyRelay account. MIT licence.

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
| `lazyrelay-start` | You are new, or do not know what to post. Saves a short brief | `list_connected_accounts`, `list_workspaces` |
| `lazyrelay-write-post` | You want one post for one platform | `get_platform_rules` |
| `lazyrelay-grade-post` | You have a draft and want an honest score and three fixes | `get_platform_rules` |
| `lazyrelay-plan-week` | You have one idea and want a week of posts | `list_posting_slots`, `get_next_free_slot`, `get_platform_rules` |
| `lazyrelay-schedule` | A post is approved and should be scheduled | `list_connected_accounts`, `get_platform_rules`, `list_pinterest_boards`, `get_tiktok_creator_info`, `get_next_free_slot`, `schedule_post`, `schedule_posts`, `list_scheduled_posts`; `publish_post_now` only if you ask for it in words |
| `lazyrelay-check-live` | Later, you want to see what was confirmed live, or want proof for a client | `list_scheduled_posts`, `get_proof_link`; `reschedule_post` only for a future time you approve |
| `lazyrelay-clip` | You already have short video clip files from a clipping tool on your own computer and want them scheduled. It uploads each clip with your own API key, schedules one post per clip per connected account after you say yes, and later shows which were confirmed live. It does not cut video | `list_connected_accounts`, `get_platform_rules`, `get_tiktok_creator_info`, `get_next_free_slot`, `list_posting_slots`, `schedule_post`, `schedule_posts`, `list_scheduled_posts` |
| `lazyrelay-client-review` | A client should approve posts before they go out | `list_scheduled_posts`, `create_review_link`, `list_review_links`, `get_post_feedback`, `reply_to_post_feedback`, `approve_post`, `revoke_review_link`, `update_post` |

A good first session: ask Claude to "help me start with LazyRelay", answer five questions, then ask for one post.

## Clips: set up a clipping tool and LazyRelay with one prompt

`lazyrelay-clip` is the posting step only. A clipping tool on your own computer cuts your long video into short vertical clips. LazyRelay schedules the finished clip files and shows which ones it could confirm are live. Paste this into your own Claude Code (it is also saved as `lazyrelay/skills/lazyrelay-clip/PROMPT.md`):

```text
I want to turn my long videos into short vertical clips on this computer and schedule them with LazyRelay. Set this up with me one step at a time. Before each step tell me in plain words what it does. Never ask me to paste a password or a key into this chat. If a command fails, show me the exact message and check the tool's own README before trying anything else.

Step 1, a free clipping tool (you install it, I choose which):
- Option A: the video-to-shorts skill, MIT licence. Needs Node.js, ffmpeg and Pillow. Install it with: npx skills add social-media-skills/video-to-shorts-skill -g -a claude-code -y
  Then run its setup check and fix what it reports. Use it only up to the point where the finished clips are saved in a folder. Skip its own posting step, because LazyRelay does the posting.
- Option B: the ClipsAI Python library (github.com/ClipsAI/clipsai, MIT licence). Follow the install steps in its README: pip install clipsai, then WhisperX, libmagic and ffmpeg as the README says. Resizing to vertical needs a Hugging Face token that I create myself, as its README explains. It finds clip start and end times and resizes to 9:16, so write me a small script that saves each clip as an mp4 in the folder.
- If I want to pull a video from a link first, use yt-dlp (github.com/yt-dlp/yt-dlp) only if I ask for it, and install it the way its README says.

Step 2, LazyRelay. Install the LazyRelay skills pack:
/plugin marketplace add gosswerner1-max/lazyrelay-skills
/plugin install lazyrelay@lazyrelay-skills
Then check the connection with /mcp. If lazyrelay is not listed, run: claude mcp add --transport http lazyrelay https://mcp.lazyrelay.com/mcp and sign in to my LazyRelay account in the browser (the connector address is https://mcp.lazyrelay.com/mcp). I have no LazyRelay account yet? Then tell me to register at lazyrelay.com first and connect one social account.

Step 3, the upload key. The clip upload uses my own LazyRelay API key. Tell me to open More, then API Keys in my LazyRelay dashboard, create a key with the Safe or Full setting, and save it on my computer in an environment variable named LAZYRELAY_API_KEY, by myself, without showing it to you.

Step 4, join them. Make one folder called clips-out. Set the clipping tool to save its finished clip files there. When I say "clips are ready", run the lazyrelay-clip skill on clips-out: check each file, upload it with my key, write a caption for each platform from what I tell you about the clip, show me one table with the clip, platform, account, time and caption, and wait for my clear yes before anything is scheduled. Later, when I ask, run lazyrelay-check-live to show which posts LazyRelay confirmed live.

Start with step 1 and ask me which option I want.
```

The two clipping tools named in the prompt are separate projects with their own licences and requirements (checked 24 November 2026): github.com/social-media-skills/video-to-shorts-skill and github.com/ClipsAI/clipsai, both MIT. The prompt installs nothing until you choose.

## Things to know

- **What "confirmed live" means.** On most platforms LazyRelay asks the platform for that exact post, using the post's own number, and sees whether it is there. On Telegram and LinkedIn the platform does not allow that, so LazyRelay only confirms that the channel (Telegram) or the public page (LinkedIn) answers. That is a weaker check, and a post removed straight after sending could still show as confirmed. Confirmed live is about the post existing on the platform. Nobody can promise how many people will see it.
- **Credits.** One credit pays for one post on one account, so one post to 3 accounts is 3 credits. A credit is taken only after the platform confirms the post is live, and if one account fails you are not charged for it.
- **What the skills are told to do.** They are told to ask you before anything goes public, to schedule for a future time by default, and never to delete posts. That is an instruction to Claude, not a lock. Two LazyRelay tools can release a post early: `reschedule_post` with a time that is now or in the past, and `approve_post`. The skills are told to refuse a past time and to show you the go-out time before approving.
- **The connector has full access.** Claude can do anything your LazyRelay account can do with posts, including publishing now and cancelling a waiting post, once you have signed in. If you want a tighter setup, use the local package with a limited key from lazyrelay.com/mcp/claude/.
- **Proof links and review links are public.** Anyone who has the link can open it. Share it only with the people who should see it.
- **Your data.** The brief and plans are plain files in the folder you work in. What Claude reads from them becomes part of your chat with Claude, and any other connector you have turned on can see that chat.
- **Uploads.** `lazyrelay-clip` uploads each clip with your own API key. LazyRelay deletes an upload after the post that uses it has been sent, and after 7 days if no post uses it. LazyRelay is a posting service and does not keep a file library.
- **Checked on 22 November 2026** (the clip skill and its prompt on 24 November 2026) against the live LazyRelay site. Plans and tools change. If something here is out of date, tell us on this repository's Issues page or email support@lazyrelay.com.

## Licence

MIT, copyright IPE Projects (Pty) Ltd. See `LICENSE`.
