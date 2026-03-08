# I Made a Video Game and I Can't Code. Here's Exactly How.

*A first-timer's honest account of vibe coding — the prompts, the surprises, the bugs, and what I learned along the way.*

---

## Where It Started

I had an idea for a game. Nothing groundbreaking — just something that made me laugh. A cheerful bin man called Barney, driving his wagon down a street full of potholes, angry residents, and rats. Very British. Very silly. Something I'd genuinely want to play for five minutes.

The problem? I can't code. Not even a little.

I'd heard people talking about "vibe coding" — the idea that you describe what you want to an AI in plain English and it writes the actual program for you. I was sceptical. Surely you'd end up with something clunky and half-finished? Something that sort of worked but didn't really *feel* like anything?

I decided to find out.

---

## The First Prompt — Word for Word

Here's exactly what I typed. No edits. This is it:

> *"can you vibe code a 2d pixel art game that is about barney the bin man who drives a wagon and has to negotiate pot holes, angry residents, rats, and ever increasing amounts of rubbish in the streets. use whatever imaging tools you have access to that can make endearing npc characters and make barney a large but friendly character in hi vis outfit - jeans , red and white converse and a shock of brown hair"*

No capital letters. No technical terminology. No specification of what programming language, what framework, what file structure. Just a description of a feeling — *"endearing NPC characters"* — and a visual image of a character I could see in my head.

What came back genuinely surprised me.

---

## What Came Out of That Prompt

Within minutes I had a working, playable browser game. One file. Open it, play it instantly. No downloads, no installs.

Barney was there — a big friendly face visible through the cab window of a green bin lorry, brown messy hair, hi-vis jacket. Three lanes of road. Wheelie bins to collect. Potholes to dodge. Mrs Miggins in her dressing gown. Mr Grumpy waving his newspaper. Rats darting across the road. A score counter. Lives. Sound effects. Scrolling buildings. Spinning wheels on the wagon. Even exhaust smoke puffing out the back.

All of that. From one paragraph.

I sat there for a moment just staring at it. I hadn't asked for spinning wheels. I hadn't asked for exhaust smoke. I hadn't asked for Mrs Miggins to have a speech bubble that said "OI!" I hadn't asked for the buildings to scroll at a different speed to the road to create a sense of depth. The AI just... made sensible decisions.

**Lesson 1: The AI fills in the gaps.** You describe the vibe. It figures out the implementation. Trust it on the technical details — that's its job.

---

## Building It Out — Levels, Bonus Rounds, and Hyperspeed

Once the basic game was working I wanted it to feel more like a real arcade experience. Not just "drive forever until you die" — proper level progression. Something to work toward. A sense of escalation.

I asked for levels that ramped up in difficulty over time, a bonus round where the road fills with gold bins instead of obstacles, and a final hyperspeed stage where everything goes mad.

What came back was a proper structure: levels advancing every 30 seconds, each one faster than the last. Level 4 became the Bonus Round — a 30-second window where only gold bins appeared on the road, obstacles cleared, and the whole thing felt like a reward for surviving. Level 9 became Hyperspeed, with a fanfare and a thousand-point bonus as the game hit its maximum pace.

This is where I started to understand something important about vibe coding: you don't have to ask for everything at once, and you don't have to know how something should be implemented. "Add levels that get progressively harder, a bonus round, and a mad final level" is enough. The AI built a proper time-based progression system with correct maths, UI indicators, and special handling for each stage — none of which I specified.

**Lesson 2: Describe the experience you want, not the system behind it.** "A bonus round that feels like a reward" produces better results than trying to specify how level transitions should work. The AI knows how games work.

---

## The Rat That Looked Like a Bird

Here's a good one.

After a few sessions of play I noticed something odd. The rats that darted across the road looked... wrong. Not rat-shaped. More like a small grey smudge. Birdlike, almost.

I asked for the rat sprite to be redesigned to actually look like a rat.

The commit message from that fix tells the full story better than I can:

*"Previous sprite resembled a small grey bird. New design has: long protruding snout, near-black whiskers extending left from snout, red nose at snout tip, small red eye, large rounded ear with deep-pink border and pale-pink inside, pink worm-like tail curving upward from the body, light grey belly patch, two-frame scurrying leg animation."*

The new rat is unmistakably a rat. Ears. Snout. Pink tail. Scurrying legs. It took one request to go from "that grey blob" to a proper character.

This might be the simplest possible illustration of what vibe coding is: you notice something looks wrong, you say so in plain English, and it gets fixed. You don't need to understand *why* the original sprite looked like a bird. You don't need to know what a "sprite array" is. You just need to be able to say "that looks like a bird, make it look like a rat."

**Lesson 3: Your eye is the quality control instrument.** You don't need technical knowledge to know when something looks or feels wrong. That instinct — "this doesn't look right" — is genuinely useful in vibe coding. Use it.

---

## The First Boss — The Rat King

Having played for a while I wanted something to work toward. A moment. A drama. I'd been building up through levels and I wanted a proper boss encounter — something that changed the feel of the game completely, with an entrance and a fight and stakes.

I asked for a boss: the Rat King. A giant version of the rats from the road, but crowned — a huge, grotesque rodent ruler. The fight would work differently to normal play: instead of dodging obstacles, you'd fire detergent bottle missiles back at him by pressing Space. He'd throw rubbish at you. Six direct hits to defeat him. Five hundred points as the reward.

I wanted a dramatic entrance too. An ominous screen overlay. His name appearing with pulsing text. Ominous music. The whole thing.

What came back was exactly that — and more. The Rat King was drawn in pixel art using only rectangles: a crown, enormous ears, a snout with whiskers, animated swinging arms, a pink tail, an HP bar across his chest. The intro faded the screen dark with red warning text: *"THE RAT KING APPROACHES..."* The background sky turned deep crimson-purple. A bass-heavy minor key theme kicked in. Villain speech bubbles appeared during the fight with random taunts. When you defeated him, a victory fanfare played and the game resumed.

Then immediately after that... it completely stopped working.

The controls went dead. Nothing responded. The game was stuck.

**This is the other side of vibe coding that nobody talks about: sometimes it breaks immediately after you build something.**

The cause, when the AI investigated, was a single undefined variable. A piece of code referenced `ROAD_BOT` — the bottom of the road — which had never actually been declared anywhere. The game hit that undefined reference and silently crashed. Invisible from the outside. Everything looked fine until it didn't. The fix was three characters: replace `ROAD_BOT` with `ROAD_TOP + ROAD_H` — the same value, calculated correctly from constants that did exist.

One undefined variable. Completely dead game. Fixed in seconds.

**Lesson 4: Bugs after big additions are normal and quick to fix.** When you add something substantial — a whole new game mode, a new character, a new system — something will usually break. This isn't a failure of vibe coding. It's just how software works. Describe what's broken ("the controls don't work anymore"), let the AI diagnose it, watch it get fixed. The process is fast.

---

## "Is This Now Pushed and Live?"

After the Rat King was working I asked the single most non-technical question of the entire project:

> *"is this now pushed and live"*

The answer was no — the changes had been saved locally but not published. One word from me ("yes") and it was done. Committed with a proper message, pushed, live.

This is worth pausing on. I didn't have to learn Git. I didn't have to learn what "commit" means or what "push" means or how GitHub Pages works. I just asked "is it live?" and it sorted it.

**Lesson 5: You don't need to learn the tooling. Just ask.** Version control, deployment, file management — all of it can be handled by the AI if you just describe what outcome you want.

---

## The Second Boss — Freddie the Toxic Foreman

The Rat King worked so well I wanted another boss. Something earlier in the game. And I had a very specific villain in mind.

Here's exactly what I asked for:

> *"add a new boss level at level 3 this one is Freddie the Foreman. He is a large Fat construction worker with blue jeans a belly that sticks out from his dirty white vest a red hard had a black long villain moustache and massive ape like arms he throws large toxic waste barrels that you have to dodge or by pressing space bar they fire back at him 6 direct shots defeat him and you get 500 points before he appears announce him as Freddie the Toxic Foreman use ominous music and writing on screen like the rat king boss intro add some dialogue boxes about how he will waste you !!"*

Again — no technical language. Just a character description and a gameplay mechanic described in plain terms. "Press space bar, barrels fire back at him" is not a technical specification. It's just describing the experience.

Note the phrase at the end: *"like the rat king boss intro."* By this point, the Rat King intro had become the established template for how a boss entrance should feel — dramatic build-up, ominous text, music, character. I was using my own game as a reference.

What came back was Freddie. Hand-drawn in pixel art using nothing but rectangles. Red hard hat. Massive arms that swung when he threw barrels. A spectacular villain moustache with curled tips. A belly that stuck out from his vest. A dramatic multi-stage intro sequence: flickering construction zone warnings, a toxic hazard alert, his name slamming onto the screen with a bass-heavy thud, and dialogue boxes where he threatened Barney before the fight began.

**Lesson 6: Once you build something you like, reference it.** "Like the Rat King intro" told the AI exactly the register I wanted — dramatic, staged, with music and text — without me having to describe it all over again. Your previous work becomes part of your vocabulary.

---

## The First Batch of Fixes — When Things Weren't Quite Right

This is where vibe coding gets really interesting. Because the first version of something is never perfect. And fixing things is where the collaboration really shows.

I played Freddie's level and noticed several problems. Here's what I asked for:

> *"couple of things need fixing freddie the foreman needs to be slighly smaller and the barrels he throws need to harm the bin wagon unless the space bar is pressed to reflect them back at him . Also the rat king boss is apprearing directly after defeating freddie this needs to be pushed back to level 6 now so the level order is level 1,2 then freddie boss level , then bonus level , then normal level 5 , then rat king level , then normal level 6 and 7 then bring the hyper speed level to level 8"*

Three separate issues in one message. A sizing problem, a gameplay mechanic problem, and a level structure problem. I didn't know *why* any of these were broken. I just knew what they looked like when they were wrong.

**What was actually happening under the hood:**

*Freddie too big* — he was drawn at full size. The fix wrapped all his drawing code in a scale transform: draw at 80% size.

*Barrels not harming the wagon* — a bug in the hit detection. The code was checking whether the barrel's X position was near pixel 80. But the wagon actually occupied pixels 45 to 215. Barrels were sailing visually through the wagon and never registering as hits.

*Rat King appearing too early* — after defeating Freddie, the game was supposed to give you a Bonus Round (level 4) then a normal level 5, then the Rat King at level 6. Instead, the Rat King appeared immediately after Freddie. The trigger level was changed from 5 to 6.

None of this required me to understand any of it technically. I described the *symptoms*. The AI diagnosed the causes and fixed them.

**Lesson 7: Describe what's wrong, not how to fix it.** You are the quality control department. The AI is the engineering department. Your job is to spot that something feels off. Its job is to figure out why and correct it.

---

## Checking the Plumbing — Collision Mechanics

After the fixes, I asked for a broader check:

> *"check that the collission mechanics work for all levels adjust if necessary. also when the Freddie level start keep the starter dialogue on the screen longer so the player can readit"*

This is a slightly different kind of prompt — not "fix this specific thing I noticed" but "check this whole system for me." The AI audited the collision code, confirmed the barrel fix from the previous session had landed correctly, then extended the dialogue timing.

The original dialogue boxes were showing for about one second each. That's nowhere near enough to read two lines of text. The fix extended each box to five seconds, making it actually readable.

**Lesson 8: You can ask for reviews, not just fixes.** "Check this works properly" is a perfectly valid vibe coding prompt. You don't need to know what to look for. Just ask it to look.

---

## A Weird Bug — Pressing UP Caused a Crash

This one was genuinely baffling from a player's perspective:

> *"if you press up when in the top lane it causes a collission and loss of life , check this and fix if required"*

Why would pressing UP cause a collision? I wasn't hitting anything. I was at the top of the screen. There was nothing above me to hit.

The explanation, when it came back, was elegant and completely invisible to me as a player.

The game tracks two things: which lane Barney is *in* and which lane he's *moving towards*. When Barney moves between lanes, his position smoothly animates. But the "which lane am I officially in" variable only updated when Barney was within 1 pixel of the destination — which took about half a second.

During that half second, even though Barney *looked* like he was in the top lane, the game still thought he was in the middle lane. So middle-lane obstacles could still hit him. The player experienced it as "pressing UP caused a hit" because it happened right after the UP press. The cause was actually a stale variable.

The fix: instead of using the logical lane variable for collision detection, calculate which lane Barney is *physically closest to* based on his actual Y position on screen. If he looks like he's in lane 0, treat him as being in lane 0 — regardless of what the internal tracking variable says.

**Lesson 9: Bugs often have a gap between the symptom and the cause.** What looks like "pressing UP causes a collision" is actually "a variable doesn't update fast enough." You don't need to trace this yourself. Describe what you experienced. The AI can close the gap.

---

## "Still Not Long Enough" — The Dialogue Problem

After extending the dialogue to five seconds per box, I played through the Freddie intro again and still felt rushed:

> *"now make the text dialogue between freddie and barney longer so that the player can read the back and forth as it is still not long enough"*

The important phrase here is "back and forth." The original sequence had Freddie threatening Barney twice, then Barney responding once. That's not really a conversation. It's more of a monologue with a footnote.

The fix expanded it into four proper exchanges — each five seconds on screen, 25 seconds of drama in total:

1. **Freddie opens:** *"You think YOUR lot can clean up MY construction site?! HA! I've buried better bin men than you!"*
2. **Barney replies calmly:** *"Mornin'! Council sent me — got a report of illegal waste dumping. Nothing personal, mate. Fancy a biscuit while we sort this?"*
3. **Freddie explodes:** *"BISCUIT?! I'll give ya a BISCUIT!! NOBODY cleans MY SITE!!"*
4. **Barney squares up:** *"Right then, Freddie. Wagon's fuelled up. Flask is full. Health and Safety says this mess gets cleaned up TODAY. HONK HONK!!"*

Actual character development. In a game about a bin man.

**Lesson 10: Iterate on the feel, not just the function.** "The dialogue doesn't feel long enough" and "it needs to feel like a real conversation" are completely valid creative notes. The AI understands creative direction, not just technical instructions.

---

## "Check the Order of the Levels"

Near the end, I asked for a final verification of the level structure:

> *"check the order of the levels and make sure it goes level 1,2 normal level 3 Freddie boss , level 4 gold bin bonus round level 5 normal , level 6 rat boss , level 7 normal level 8 hyper speed ."*

This caught a significant hidden bug. When you defeated Freddie, the game set your position to `frameCount = level × 1800`. At level 4, that's 7,200. The game calculates your current level as `1 + (7200 ÷ 1800) = 5`. Not 4. Level 5. The Bonus Round was being skipped entirely.

Same bug for the Rat King defeat — it was skipping level 7 and going straight to Hyperspeed.

The fix was one character: use `(level - 1) × 1800` instead of `level × 1800`. Two entire levels were silently disappearing because of a single multiplication error that had been there since the level reorder.

**Lesson 11: Ask for verification, not just fixes.** "Check this is working the way I expect" catches things neither of you noticed. The bug had existed since the level reorder was first done — it took a deliberate "check the order" prompt to surface it.

---

## What I've Learned About Vibe Coding

Looking back across all of this, here are the things I'd tell someone who wants to try vibe coding for the first time.

### You are the creative director. The AI is the whole technical team.
Your job is vision, taste, and feedback. The AI's job is everything else. This is a genuine collaboration, not you dictating and it obeying. It makes decisions you didn't ask for — spinning wheels, exhaust smoke, speech bubbles — and those decisions are usually good. Let them land.

### Be specific about what it should *feel* like, not how it should *work*.
"The Rat King should feel like a proper dramatic boss encounter" produces better results than trying to specify systems you don't understand. Describe personalities. Describe experiences. Describe the moment you want the player to feel. Let the AI translate that into code.

### Your eye is the quality control instrument.
You don't need technical knowledge to know when something looks wrong. "That rat looks like a bird" is a useful observation. "Press UP and you die somehow" is a useful bug report. Your taste and instincts are the tools you bring to this.

### Describe symptoms, not solutions.
"Pressing UP causes a collision" is the right kind of bug report. "Change the barneyLane variable update threshold" is not something you'd ever say. Describe what you see. Let the AI figure out why.

### Build on what you have.
"Like the Rat King intro" became useful shorthand that saved a paragraph of description. Once you've built something that works and feels right, reference it. Your own game becomes part of your design language.

### Check your work and ask for verification.
Play the game after every change. Notice when something feels off. Ask for it to be fixed. Ask for systems to be audited. "Check the level order" found a bug that neither of us had caught during development. Asking for verification is part of the job.

### Bugs after big additions are normal and fast to fix.
The Rat King fight broke the controls immediately. Freddie's barrels didn't hurt. The bonus round was being skipped. None of these were problems for long — each was diagnosed and fixed in minutes when described correctly. Don't be discouraged by things breaking. That's just building software.

---

## The Full Journey, End to End

**March 1** — one paragraph prompt, Barney's Bins exists.

**March 2-3** — mobile fixes, leaderboard, iOS audio, landscape layout sorted.

**March 3 evening** — levels, bonus round, and Hyperspeed added.

**March 4 morning** — bonus round improved, gold bins worth more. Rat sprite redesigned: was a bird, now is a rat.

**March 4 afternoon** — The Rat King boss built from scratch. Intro, fight, speech bubbles, boss music, victory track. Then immediately crashed due to one undefined variable. Fixed in minutes. Pushed live.

**March 7** — Freddie the Toxic Foreman added. Then: made him smaller, fixed barrel collision, fixed level ordering, checked all collision mechanics, extended dialogue timing, fixed the phantom UP lane bug, made the back-and-forth dialogue longer, verified the level order end-to-end.

That's the whole story.

---

## Try It Yourself

If you want to try vibe coding, my honest advice:

**Start smaller than you think you need to.** One game mechanic. One character. One screen. Get that working, then add to it.

**Have a clear vision of the *vibe* before you start.** Not technical specs — just know what it should feel like. Charming? Tense? Silly? That feeling will guide every prompt.

**Don't try to understand the code.** That's not the goal. The goal is the experience of playing the thing you imagined. If it plays the way you wanted, the code worked.

**Iterate.** The first version is a starting point, not a finished product. Every "that's not quite right" prompt makes it better.

**Ask questions freely.** "Is this live?" "Check this works properly." "Can you explain why that happened?" All valid. All useful.

The barrier to building things — real, working, playable things — has genuinely collapsed. Barney's Bins is proof of that.

HONK HONK.

---

*Barney's Bins is playable at [github.com/SynapticStar101/Barneys-bins](https://github.com/SynapticStar101/Barneys-bins). Built entirely through vibe coding with Claude, an AI assistant by Anthropic. No prior coding knowledge required — or used.*
