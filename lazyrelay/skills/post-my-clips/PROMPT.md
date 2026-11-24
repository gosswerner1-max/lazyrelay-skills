# Ready-to-paste prompt: clips to LazyRelay

Paste the block below into your own Claude Code. It sets up a free clipping tool, the LazyRelay connection and the `post-my-clips` skill, and joins them. The same block is in the README of this pack.

Based on the clipping tools' own READMEs and LazyRelay's own pages. If a command fails, Claude is told to show you the exact message and look at the tool's README. The first option is the one to choose; the second is for people who are comfortable with code.

<!-- prompt:start -->
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
<!-- prompt:end -->
