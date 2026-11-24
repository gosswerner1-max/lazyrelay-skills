# Ready-to-paste prompt: clips to LazyRelay

Paste the block below into your own Claude Code. It sets up a free clipping tool, the LazyRelay connection and the `lazyrelay-clip` skill, and joins them. The same block is in the README of this pack.

Checked on 24 November 2026 against the clipping tools' own READMEs and LazyRelay's own pages. Tools change, so if a command fails, Claude is told to show you the exact message and look at the tool's README.

<!-- prompt:start -->
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
<!-- prompt:end -->
