# Ruby Seven (ruby7) - Studio 24

Static site, no build step. Runs the base-game maths (v0.1, 97% RTP) in the browser with play money, through a stand-in for the Tequity Connector that contains the real game-server code.

## Deploy to Vercel
`npx vercel --prod` in this folder (framework: Other, no build command), or push to GitHub and import on vercel.com.
Add `?bench=1` to the URL for the test bench (force Super Wild draw, Nudge, Gift; spin history; maths check).

## Art (assets/, WebP)
Replace any file under the same name to update it.
- `ui/` frame, logo, control bar (balance/bet/win slots), SPIN, turbo/auto/sound/minus/plus buttons, Extra Bet panel, backgrounds (landscape + portrait).
- `special/` multiplier reel strip, centre frame, orbs: red 1-10x, ruby 15-25x, black 50-100x, gold 500-1000x, green Nudge (numbers drawn by the game).
- `fx/` Ruby Draw / Ruby's Gift / Nudge title plates, draw ring (8 sockets), reel energy frame, BIG and MEGA WIN banners, 20 chip/coin/confetti/sparkle particles.
- `ruby/` Ruby's poses by state, plus the full-screen draw intro.
- `symbols/` Wild and Super Wild (Ruby close-ups); R, B, G, A, K, Q, J are still the placeholders cut from the concept mockup.

## Controls
Fixed control section under the reels, laid out like the base game: WIN on top; bet -/+ left, SPIN centre, AUTO / TURBO / MENU right; BALANCE at the bottom. MENU holds Sound and Extra Bet (also the EX button on the cabinet).

## Ruby's movement (stand-in for a Spine or AI-video version)
Ruby is never still: her current pose is bent in real time (WebGL) for breathing, a head tilt, hair and gown sway, faster and bigger when she is excited. Action poses change with a flash. Effects: light rays behind her during features, a hand charge and chip burst that fills the draw ring, sparkles on each award and on the Gift, and BIG / MEGA / SUPER auras (green / blue / red-fire) with a coin fountain. On phones she stands above the reels.

| State | When | Poses |
| --- | --- | --- |
| idle | between events | idle1, glance idle3 every 7-9 s |
| antic | Super Wild lands, or 50x+ centre tease | antic1-3 |
| draw intro | Super Wild + win, ~2 s full screen | draw-intro.jpg |
| deal | chip burst fills the 8-socket ring | deal1-4 |
| award | each awarded token | award1-3 |
| gift | Ruby's Gift | gift1-5 |
| extra | Extra Bet switched on, ~3 s | extra1-3 |
| win | BIG / MEGA / SUPER banner | win1 / win2 / win3 |

Head anchors for the movement are in assets/ruby/poses.json (hx, hy). Replace a pose image and its anchor if her head is in a different place.

## Still needed
- Final reel symbols: Ruby 7, diamond, triple/double/single BAR, cherries, poker chip, and the cream tile (prompts A0-A7).
- SUPER WIN banner (E6, third prompt). The game uses the MEGA WIN art with a SUPER WIN tag for now.
- An enabled SPIN button. Both supplied files are the disabled version; the brighter one is used.

## Music
assets/audio: mus-base.mp3 (lounge loop, 111.4 s), mus-banner.mp3 (big-band loop, 20.6 s), mus-sting.mp3 (banner finale, 5.7 s, played on the total win; mus-banner-end.mp3 = the banner track's own 3 s ending, kept as a spare). Generated in ElevenLabs Music, looped and levelled by Studio 24.
Music starts on the player's first tap (browsers block sound until then), switches to the banner loop for BIG/MEGA/SUPER, plays the sting on the final value and fades back to the base loop. The music button mutes everything.
Ruby's voice: assets/audio/vo, 34 lines (13 moments, 2-3 takes each), trimmed and levelled to -16 LUFS. One random take per moment, never the same take twice running; the music dips under her. Big moments (welcome, Extra Bet, Super Wild, Draw, Nudge, Gift, BIG/MEGA/SUPER) always speak; tease 1 in 2, near miss 1 in 3, laugh on 1 in 5 small wins, award line only for draws of 25x and up. Low-priority lines never talk over her.
Sound effects: assets/audio/sfx, 22 sounds (46 takes) from ElevenLabs, trimmed and levelled. Takes rotate so repeats never sound identical; reel stops and Nudge grabs also vary slightly in pitch. Loops: reel spin (while reels turn), marquee bulbs (whenever the frame lights chase or flash), coin shower (under win banners). Synth fallback plays only if a file fails to load.

Reel sounds (spin start, spin whirr, reel stops, multiplier reel stop) are synthesised by Studio 24 rather than ElevenLabs: a solid latch-and-clunk on each landing with a settle tick on the bounce, a smooth whirr that thins out as each reel lands. The ElevenLabs reel takes were rattles without a clear impact and are not used.

Ruby's idle is now real video: assets/ruby/ruby-idle.mp4 (H.264, Chrome/Safari/iPhone) and ruby-idle.webm (VP9 fallback). Two ElevenLabs (Seedance) clips joined into a 10 s loop, green keyed, colour in the top half and alpha matte in the bottom half, drawn by Ruby's WebGL renderer. Her pose art is still used for feature moments, cross-faded in and out of the video.
