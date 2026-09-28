# Stack Sprint — Sky Courier Collection v3

Build 3.0.0 · 28 September 2026. Implemented update candidate; real-browser/device acceptance and YouTube certification remain outstanding. The live Vercel deployment was not changed by this build.

## Play කරන්න

`release/Stack_Sprint_Play.html` full browser එකකින් open කරන්න. Phone document preview එකේ game එක run නොවුණොත් hosted version එකක් භාවිත කරන්න. මේ package එකෙන් email/cloud account system එකක් Vercel වෙත එකතු කරලා නැහැ: standalone progress මේ browser එකේ පමණයි. Actual YouTube Playables build එක YouTube SDK save භාවිත කරනවා.

## What's new

- 100 authored challenge recipes in ten worlds, with a map, optional three-star goals and 20 guardian encounters.
- Nine robot types: scout, armored, shield, sentry, moving drifter, tracking shooter, splitter, sparklet and repair bot; three campaign ranks.
- Eight mechanically different blasters. Campaign unlocks at 0, 5, 15, 25, 40, 55, 70 and 85 distinct clears.
- Six cosmetic explorer silhouettes; twelve coordinated finishes; separate equipment Hangar.
- One permanent module point per first clear, balanced across damage, firing rate and rebuilding.
- Optional assist, automatic checkpoints, v1/v2 save migration, reduced motion, separate music/effects controls.
- Endless mode retained, now with eight in-run blaster tiers and a bounded enemy growth curve.
- Original procedural geometry/audio. No runtime asset or engine downloads.

## Deliverables

| File | Use |
| --- | --- |
| `release/Stack_Sprint_Play.html` | Self-contained standalone game |
| `release/Stack_Sprint_Vercel.zip` | Static index.html and existing Vercel configuration |
| `release/Stack_Sprint_YouTube.zip` | YouTube-specific runtime, official SDK first, index.html at root |
| `release/Stack_Sprint_Source.zip` | Editable source, research, tests and build script |
| `Stack_Sprint_V3_Research_Plan.md` | Research synthesis, architecture, psychology limits and 100-source register |
| `Stack_Sprint_100_Level_Plan.md` | Every implemented stage, goal, beat sequence, seed and unlock |
| `release/Stack_Sprint_V3_Art_Study.png` | Actual Canvas explorer/world studies, not browser screenshots |
| `YOUTUBE_RELEASE.md` | Metadata draft and remaining publishing gates |

The old `Stack_Sprint_Research_Design.md` is retained as historical v2 documentation; its balancing and feature counts are superseded by the v3 plan.

## Controls and progression

Drag/swipe, use on-screen arrows, or press Left/Right or A/D to steer. Firing is automatic. Space or PULSE spends a fully charged pulse; P pauses. Escape closes the current panel or toggles pause.

Blocks are health; their + label is the reliable pickup cue. Crystals give score, shields protect one hit, purple cells build energy/pulse charge, PLUS gates give temporary rapid fire, and arrow pads briefly accelerate travel. Robot lanes must be aimed at or avoided. Warned stripes show incoming ranged attacks.

A campaign finish opens the next stage. Optional objectives and finish-stack health earn two additional stars. A first clear grants one module point. Replays improve stars/best scores without generating additional points. Select newly earned weapons, explorers and finishes in My hangar; they are not automatically equipped.

Campaign starts with eight blocks before Rebuild bonuses. Endless starts with six. Cosmetic shapes do not change collision size. Assist applies on the next campaign launch: 20% slower travel, rounded reduced damage and extended relevant warnings. Scores are personal bests, not an anti-cheat online tournament.

## Architecture

`content.js` owns authored records and collections; `core.js` owns deterministic rules and save migration; `renderer.js` draws Canvas geometry; `platform.js` owns persistence, SDK lifecycle and procedural audio; `game.js` coordinates menus, controls and rewards. The HTML/CSS interface responds to orientation without restarting the run.

Keep the standalone storage key `stack-sprint-v1` despite schema version 3; changing it would orphan existing browser progress. v2 runs retain their checkpoint but proceed with v3 balancing. v1 runs retain important progress with a regenerated combat segment. Unsupported or failed loads protect existing data by disabling writes and showing Practice mode.

Cloud writes are serialized. Material progress is saved immediately, with periodic active checkpoints. A failed write remains queued; host pause suspends ongoing play and future work. The best score is submitted only after successful save/load establishes it. Actual Playables uses no localStorage progress writes.

## Build and verify

No npm installation is needed for the game, build or behavioral tests:

```sh
python3 build.py
node qa/combat.test.cjs
node qa/campaign.test.cjs
node qa/lifecycle.test.cjs
node qa/systems.test.cjs
node qa/package.test.cjs
node qa/report.cjs
python3 build.py
```

The report step refreshes documented metrics. The final build packages the latest reports. To serve locally for human testing:

```sh
python3 -m http.server 8765 --bind 127.0.0.1
```

Open `http://127.0.0.1:8765/release/Stack_Sprint_Play.html`. Stop with Ctrl+C. Use a controlled HTTPS preview for physical phones.

Optional Canvas-only inspection requires an available `skia-canvas` installation; set `STACK_SKIA_MODULE` to its module path, then run `node qa/render.cjs`. It is a QA dependency, not part of the shipped game. Test-only application exports are removed from release scripts.

## Evidence and limits

The campaign solver cleared all 100 stages sequentially with earned equipment/modules and reached all 300 stars. It is a full-state scripted player, not a human. Separate endless simulations exercise 30 seeds. Behavioral tests cover firing roles, enemy behavior, duplicate rewards, migration, input, save failure, host pause/mute, controller flow and score/save ordering. Exact counts and package sizes are generated in the v3 plan and `qa/*-results.json`.

Four Canvas aspect ratios, a boss, six explorer silhouettes and ten worlds were rendered and inspected. This does not verify actual browser layout, thumb reach or device frame rate. Real YouTube Test Suite execution, physical mobile/browser QA, human playtesting, performance profiling and platform review remain required.

Do not claim “100% production-ready,” guaranteed 60 FPS, measured retention, or that 100 competitor games were personally played. The study is public-description desk research with transparent sources. No third-party game art, code or music is included. The working title is not a trademark clearance.

## Updating the hosted version

Use the Vercel ZIP's files in the existing static project's normal deployment workflow. Verify the v3 collection title, start/complete/reload a level, check that the old best survives, then test a phone. Keep the same production origin if you want the browser's existing local save to remain available. This archive is not an automatic deployment, and a source ZIP is not a YouTube upload bundle.

## GitHub → Vercel deployment

Connect this repository to the existing Stack Sprint Vercel project, use production branch `main`, and leave Root Directory at the repository root. Framework: Other. This repository commits the static build in `deploy/vercel`; root `vercel.json` selects that output without an installation or build step. After source edits, run `python3 build.py`, run the checks above, and commit the updated `deploy/vercel/index.html` alongside source changes.
