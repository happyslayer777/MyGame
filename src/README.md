# Personal Goon Deck — Kayla + Yuki + Emi + Mimi

UI (Obsession OS revamp): glass header w/ per-girl accent theme (Kayla violet / Yuki crimson / Emi pink), avatar switcher, segmented tabs + desktop side rail + mobile bottom nav, deck hero stats, tilt cards w/ snap badges, chat stage sheet (quick / wardrobe / where / heat / direct), floating composer (Enter sends), masonry gallery, Ken Burns shrine, command palette (Ctrl/⌘K), shortcuts (1-3 girls, S snap, E edge), lightbox wall/save, bottom-sheet modals on mobile.

Deck-first. You land on CONTROL DECK, not chat.

Deck:
- cards for Kayla + Yuki + Emi (full-body card portraits, tall contain crop so head-to-toe shows). Seeds live in index.html (KAYLA_*/YUKI_*/EMI_*) + merge-missing on boot, so storage wipes re-seed. chat / edit / delete per card, + create card
- new bitch modal: name, tagline, personality/voice, look (face/hair/eyes/body), image lock (auto-built from look if blank), default outfit, opener
- shared world lore + house rules — injected into every reply and every snap
- global stats (bitches, edges, snaps, locked-in time, ruins)

Chat:
- per-girl history, facts, wallpaper, opener. Switch via deck card.
- quick prompts (tease/task/countdown/worship/beg/strip)
- send uses: girl system + look + outfit + world + 1-line summary of every OTHER girl + per-girl facts. That's how they know about each other.

Snaps (image relevance — final prompt is compiled by the text model, fail-open to the assembled draft; debug via window.__lastAssembly vs __lastPrompt):
- LATEST 3 msgs = absolute truth, earlier 9 = background only (no pose resurrection)
- director JSON forces: pose verb + torso + weight, 3-6 verbs quoted from LATEST, 60-80w limb-by-limb body with no body-type words, exact clothing + hem + exposed skin, CLOTHED/PARTIAL/NUDE/CLOSEUP state, 15w setting lock (same room unless moved), surfaces + lighting, POV camera with framing rules (all-fours/kneel/bent/spread = full-body wide, rear-present = low rear 3/4 + look-back), face/gaze from LATEST only
- painter expands to 90-120w visual nouns, pose+verbs first, every verb preserved verbatim
- assembly: pose + verbs + director clothing + paint + per-girl style lock + variant anatomy + POV + quality + piss/dick extras. Water = nude unless keep-on ordered. Duo girls never drawn unless asked. Yuki/Emi carry puffy:1 — enforced huge puffy pale mound wherever crotch shows (prompt + bulbous-neg strip + checker demands mound, still fails pink/red/inner detail). Seal negs (pink vulva/labia, red, inner labia detail, open hole, gape, nub) now fire on every crotch-visible variant, explicit included — previously explicit mode relied on positives alone. Emi is tiny-thick canon (very short doll-like stature, full C-cup bust, big round ass, thick thighs, NEG_EMI bans bust/height/age exaggeration) with frame-scaled plump mound bases; Yuki keeps full huge mound. Piss detector is whole-word + crotch-proximity gated (was substring: 'gesturing' triggered it). Painter/compile strip pink/red as anatomy colors (clothing reds kept). Director camera asking close-up coerces NUDE->CLOSEUP variant. Director hands rule: hands never touch own crotch, spread opens thighs only. Seal negs are weighted.
- WARDROBE: default outfit tokens are stripped from the style lock whenever she strips/changes — identity (face/hair/eyes/body) stays, director's clothing is injected up front. States: NUDE (fully nude), PARTIAL (pants off + top on, top lifted, dress hiked — removed garments also go in the negative prompt), CLOTHED. CLOTHED-explicit keeps fabric pressed into cleft instead of going nude. What the snap draws is saved as outfit_current, which becomes text truth for the next reply.
- outfit bar (chat): strip / nude / hoodie / bikini / dress / ski / shower — each sends the line in-character AND auto-snaps, so text + image stay in sync. Wearing-now readout under the bar.
- ENVIRONMENT (fixed): persistent locked room per place (facts.env_place/env_desc). First snap somewhere runs one env pass (40-60w room: floor, walls, furniture positions, props, light) and every later snap reuses it verbatim until the director flags a move — no more grey voids. WHERE bar (bedroom/shower/beach/pool/cafe/car) moves her + auto-snaps; text announces the room once, outfit auto-rules still apply (bikini at beach, nude in water).
- per-girl style lock (Kayla keeps purple/jade/hoodie lock + extra bans, new girls use their own look). Neg = NEG_BASE + anatomy + per-girl bans.
- regen supported, debug in window.__lastDirector/__lastPainter/__lastPrompt.

Session/gallery/shrine:
- session timer + EDGE/ruined/allow with taunts in active girl's voice, global persistent stats
- gallery aggregates all girls with per-girl filter, wall/full/save
- shrine slideshow across all snaps
- STRICT VISION CHECK (header toggle, default on): every risky snap (outfit change/removal, move, PARTIAL/NUDE/CLOSEUP) is shown back to the text model — pass/fail on outfit+pose+place, one auto-fix re-shoot on fail with "auto-fixed:" note. Fail-open: checker errors just deliver the original.
- every bubble: ↻ regen + [3 takes] (same frozen prompt, 3 seeds, click to promote) + [clothes|pose|room] one-tap fix (prompt surgery at guidance 8 + remembers a scene ban in facts.fix_bans). Hover any photo for its seed. 🔊 on each of her messages speaks it (Paradee local TTS, voice "heart", lazy ~20MB first load, *actions*/emoji stripped, new speech cuts off old); header "voice" toggle auto-speaks new messages.
- SCENE row (chat, under WHERE): heat off/tease/hot/feral — Web Audio synth layers (breathing that quickens with heat, wet squelch bursts at 2+, heartbeat at 1+) + her voice moaning breathy syllables on a timer (only if voice engine loaded, never over her talking) + ♫ bed ambient loop (uploaded MP3). New heat/speech cuts old audio.
- prompt budget: painter 55-75w, env 30-45w, wardrobe anchor front AND back, guidance 8 on risky snaps / 7 safe (whole numbers only — the service rejects fractions), error-URL results never saved as snaps.
- CANON STUDIO (canon tab): face + nude-body auditions — same frozen prompt, 6 dice rolls each. Crown winners per girl → canonFace/canonBody (reference, avatar, winning seed). "Distill traits" reads the crowned face and stores 12 enforced identity words (canonTraits) that go into every identity lock + vision check. No audition batches ship anymore (both deleted per user) — the canon grid shows only the crowned images with re-crown buttons.
- seeds: same seed + same prompt ≈ same image. Seeds do NOT carry a look across different prompts — they reproduce winners and explore takes. Stored per snap (entry.w).
- stress-tested (scratch/stress-a|b.js, 27 gens, 27/27 OK): 8-way parallel fine, backend serializes at ~5s/gen so wall time scales linearly (3 takes ≈ 15-20s); all 4 resolutions, guidance 1-30, 6k-char prompts, 1.5k negatives all fine. Button-mashing snap is safe.
- coherence road-trip (scratch/cohere/, 16 gens): clean — doggy, kneeling, bent-over (needs pose-first ordering, which the pipeline already does), spread; hard for the model — floor-lick (refused 4 phrasings, park it or roll takes), sealed crotch close-ups (framing wanders + spread-vs-sealed contradiction: CLOSEUP now uses slim-gap thighs + ass/bust stripped from the lock), squat-sealed (spread opens her: sealed guard words + vision-checker auto-fix is the backstop, not stronger prompt words), piss-sealed (a stream needs an opening: PISS now allows slightly-parted-pale instead of fighting it).

Storage (kv-plugin, root.chatSave):
- deck folder: roster, world, activeId, goonstats, sesslog
- c_<id> folder per girl: entries, bg, facts
- legacy kayla folder migrated once into c_kayla on first boot.

Group chats (new):
- deck GROUP CHATS section: create/edit/delete groups (2+ girls), card shows members + group snap count. Opening a group loads shared history; leaving returns to solo with nothing lost.
- group talk: your line goes in, then each member (up to 4) replies in turn, in character, reacting to you AND each other (jealousy/team-ups). Short lines so it stays fast.
- images: snap dropdown picks one girl (reuses her solo pipeline + the group's shared room), GROUP PIC draws everyone together in one frame (768x512, exact-count lock, guidance 7). Solo snaps stay single-girl; group pic is the only multi-girl render.

Canon library (canon tab top): grid of all girls, face + nude body each, tap photo for full view. Finished girls show green border + traits; empty slots show placeholders + 'canonize her' (jumps into her studio). Per-girl audition flow unchanged below.

Session bot (session tab): pick teasing/steady/relentless, hit bot: on — she coaches hands-free (stroke tasks, countdowns with live counter, holds, position changes, praise, edging dares), logged to the session feed + spoken if voice is on. Toggle off anytime; EDGE/ruined/allow work alongside it.

## Anime-girl heaven (group chats + ambient life)
- Girls talk MORE: solo replies up to ~150 words, group replies up to ~130. Group turns: the girls you name answer first, then rotation, plus 35% reaction chains where one girl reacts to another (tease/jealous/team-up/steal him). Rivalry baked into the group prompt — they compete openly, call each other by name, start girl-girl exchanges.
- Ambient pings: every 45s tick, an unmuted girl not in your current chat may text you (jealous jab, check-in, tease) referencing who you're with. Toast with her avatar + preview, click to jump. Switcher dots + deck badges + group-card previews track unread; cleared on entry. Only fires when you're recently active, never while sending/snapping.
- Mute (🔔 header / deck card / member sheet): muted girls send NOTHING ambient or in groups until unmuted. Direct 1:1 chat still replies. Persisted on the roster.
- Group UI: header member-stack avatars + tap for member sheet (mute/snap-one/kick per row, +add anyone mid-chat with her opener posted, group pic, rename via edit modal, leave/delete). EVERY group line renders under its speaker's own pfp + name + color (g0-g3) — text, typing dots, snaps, restored history. Typing indicator shows the actual typer's face. Group member snaps save to the group (who-tagged), never leak into solo.
- Messages inbox (chat tab = conversation list): all 1:1s + groups with avatar/stack, live preview, relative time, unread badges, active highlight, +chat/+group buttons (new private girl, new group). Tap to jump, ‹ back button in header returns to the list. Second tap on the chat nav also returns to the list. Group presence line shows member names.
- Meters (💗 button in chat): every girl runs Love / Lust / Interest 0-100. Seeds differ — Emi starts far gone, Yuki makes you earn it, Mimi is sweet and curious but unclaimed, new girls start cool. They grow from time, snaps, edges, likes; they steer how she talks (aloof → devoted, cool → feral) and how often she pings/posts. Deck cards show mini bars.
- The Network (feed tab): public social timeline. Girls post on their own (thirst, shade, beef — meter-driven, never explicit, never your DMs, never photos unless SHE chooses). You post, like, comment (she answers), DM from any post. 📤 on any of her snaps shares it publicly as you — expect jealousy. Private stuff only escapes if SHE posts shade or YOU share.
- Girl-made beef chats: when two girls burn hot enough they open a group with you in it (toast invite, unread, opens like any group) and start fighting over you in the openers.
- Batch 2 interactions: swipe-right or long-press any bubble to reply (quote chip renders on both sides, tap to jump+flash; quoted girl answers first in groups); double-tap or reaction bar for ❤️😂😮🥹👍✨ (short "hey"s may earn just a tapback instead of a paragraph); long-press menu with regen (alt versions stored, cycle them), copy, pin-to-memory, flag-redo; "📌 will remember that" chips + editable memory list in the meter sheet; bond hearts 1–10 on cards/inbox/sheet with milestone toasts; inbox search covers names, previews AND message text; "while you were away" deck card after 2h+ gaps.
- Stage director override (facts.scene_note via STAGE box + dirSnap) steers the NEXT snap with highest priority — framing, outfit, place.
