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

Barney was there — a big friendly face visible through the cab window of a green bin lorry, brown messy hair, hi-vis jacket. Three lanes of road. Wheelie bins to collect. Potholes to dodge. Mrs Miggins in her dressing gown. Mr Grumpy waving his newspaper. Rats darting across the road. A score counter. Lives. Level progression. Sound effects. Scrolling buildings. Spinning wheels on the wagon. Even exhaust smoke puffing out the back.

All of that. From one paragraph.

I sat there for a moment just staring at it. I hadn't asked for spinning wheels. I hadn't asked for exhaust smoke. I hadn't asked for Mrs Miggins to have a speech bubble that said "OI!" I hadn't asked for the buildings to scroll at a different speed to the road to create a sense of depth. The AI just... made sensible decisions.

**Lesson 1: The AI fills in the gaps.** You describe the vibe. It figures out the implementation. Trust it on the technical details — that's its job.

---

## The First Real Addition — Freddie the Toxic Foreman

The game had a boss already — the Rat King, a giant boss rat that appeared at higher levels. I wanted another one, earlier in the game. And I had a very specific villain in mind.

Here's exactly what I asked for:

> *"add a new boss level at level 3 this one is Freddie the Foreman. He is a large Fat construction worker with blue jeans a belly that sticks out from his dirty white vest a red hard had a black long villain moustache and massive ape like arms he throws large toxic waste barrels that you have to dodge or by pressing space bar they fire back at him 6 direct shots defeat him and you get 500 points before he appears announce him as Freddie the Toxic Foreman use ominous music and writing on screen like the rat king boss intro add some dialogue boxes about how he will waste you !!"*

Again — no technical language. Just a character description and a gameplay mechanic described in plain terms. "Press space bar, barrels fire back at him" is not a technical specification. It's just describing the experience.

What came back was Freddie. Hand-drawn in pixel art using nothing but rectangles. Red hard hat. Massive arms that swung when he threw barrels. A spectacular villain moustache with curled tips. A belly that stuck out from his vest. A dramatic 7-stage intro sequence with flickering warning signs, a toxic hazard alert, his name slamming onto the screen with a bass-heavy sound effect, and dialogue boxes where he threatened Barney before the fight began.

**Lesson 2: Be specific about the character, not the code.** I didn't say "create a boss sprite with the following pixel array." I described a person. A fat bloke in a hard hat with a stupid moustache. The AI translated personality into pixels.

---

## "Is This Now Pushed and Live?"

After the Freddie changes were done, I asked the single most non-technical question of the entire project:

> *"is this now pushed and live"*

The answer was no — the changes had been saved locally but not committed to GitHub or published. One word from me ("yes") and it was done. Committed with a proper message, pushed, live.

This is worth pausing on. I didn't have to learn Git. I didn't have to learn what "commit" means or what "push" means or how GitHub Pages works. I just asked "is it live?" and it sorted it.

**Lesson 3: You don't need to learn the tooling. Just ask.** Version control, deployment, file management — all of it can be handled by the AI if you just describe what outcome you want.

---

## The First Batch of Fixes — When Things Weren't Quite Right

This is where vibe coding gets really interesting. Because the first version of something is never perfect. And fixing things is where the collaboration really shows.

I played Freddie's level and noticed several problems. Here's what I asked for:

> *"couple of things need fixing freddie the foreman needs to be slighly smaller and the barrels he throws need to harm the bin wagon unless the space bar is pressed to reflect them back at him . Also the rat king boss is apprearing directly after defeating freddie this needs to be pushed back to level 6 now so the level order is level 1,2 then freddie boss level , then bonus level , then normal level 5 , then rat king level , then normal level 6 and 7 then bring the hyper speed level to level 8"*

Three separate issues in one message. A sizing problem, a gameplay mechanic problem, and a level structure problem. I didn't know *why* any of these were broken. I just knew what they looked like when they were wrong.

**What was actually happening under the hood:**

*Freddie too big* — the character was drawn at full size. The fix was wrapping all the drawing instructions in a scale transform: draw at 80% size. One line of technical change, invisible to me as the requester.

*Barrels not harming the wagon* — there was a bug in the hit detection. The code was checking whether the barrel's X position was near pixel 80 on the screen. But the wagon actually occupied a much wider area (pixels 45 to 215). So barrels were sailing through the visual wagon and never registering as hits. The fix: use the correct width of the wagon in the collision check.

*Rat King appearing too early* — a level ordering bug. After defeating Freddie, the game was supposed to go to the Bonus Round (level 4), then normal level 5, then the Rat King at level 6. But because of how the level numbers were wired up, defeating Freddie immediately triggered the Rat King. The fix: change the trigger from level 5 to level 6.

None of this required me to understand any of it technically. I just described the *symptoms* — Freddie's too big, barrels don't hurt me, the Rat King appears too soon — and the AI diagnosed the cause and fixed it.

**Lesson 4: Describe what's wrong, not how to fix it.** You are the quality control department. The AI is the engineering department. Your job is to spot that something feels off. Its job is to figure out why and correct it.

---

## Checking the Plumbing — Collision Mechanics

After the fixes, I asked for a broader check:

> *"check that the collission mechanics work for all levels adjust if necessary. also when the Freddie level start keep the starter dialogue on the screen longer so the player can readit"*

This is a slightly different kind of prompt — not "fix this specific thing I noticed" but "check this whole system for me." The AI went through the collision detection code for every level, found the barrel hit detection bug (already fixed), confirmed everything else was correct, and then extended the dialogue timing.

The original dialogue boxes were showing for about one second each. That's nowhere near enough to read two lines of text. The fix extended each box to five seconds, making it actually readable.

**Lesson 5: You can ask for reviews, not just fixes.** "Check this works properly" is a perfectly valid vibe coding prompt. You don't need to know what to look for. Just ask it to look.

---

## A Weird Bug — Pressing UP Caused a Crash

This one was genuinely baffling from a player's perspective:

> *"if you press up when in the top lane it causes a collission and loss of life , check this and fix if required"*

Why would pressing UP cause a collision? I wasn't hitting anything. I was at the top of the screen. There was nothing above me to hit.

The explanation, when it came back, was elegant and completely invisible to me as a player:

The game tracks two things: which lane Barney is *in* (`barneyLane`) and which lane he's *moving towards* (`barneyTargetLane`). When Barney moves between lanes, the game smoothly animates his position. But `barneyLane` — the "which lane am I officially in" variable — only updated when Barney was within 1 pixel of the destination. That took about half a second.

During that half second, even though Barney *looked* like he was in the top lane, the game still thought he was in the middle lane. So middle-lane obstacles could still hit him.

When the player pressed UP from the middle lane, arrived *visually* at the top lane, pressed UP again (doing nothing because they were already at the top), and then got hit — the game registered it as a middle-lane collision. The player experienced it as "pressing UP caused a hit."

The fix: instead of using the logical lane variable, calculate which lane Barney is *physically closest to* based on his actual Y position. If he's at Y=201 (basically lane 0), treat him as being in lane 0 for collision purposes, regardless of what the lane tracking variable says.

**Lesson 6: Bugs often have a gap between the symptom and the cause.** What looks like "pressing UP causes a collision" is actually "a variable doesn't update fast enough." You don't need to know this. You just need to describe what you experienced. The AI can trace the gap.

---

## "Still Not Long Enough" — The Dialogue Problem

After extending the dialogue to five seconds per box, I played through the Freddie intro again and still felt rushed:

> *"now make the text dialogue between freddie and barney longer so that the player can read the back and forth as it is still not long enough"*

The important word here is "back and forth." The original sequence had Freddie threatening Barney twice, then Barney responding once. That's not really a conversation. It's more of a monologue with a footnote.

The fix expanded it into four proper exchanges:

1. **Freddie opens:** *"You think YOUR lot can clean up MY construction site?! HA! I've buried better bin men than you!"*
2. **Barney replies calmly:** *"Mornin'! Council sent me — got a report of illegal waste dumping. Nothing personal, mate. Fancy a biscuit while we sort this?"*
3. **Freddie explodes:** *"BISCUIT?! I'll give ya a BISCUIT!! NOBODY cleans MY SITE!!"*
4. **Barney squares up:** *"Right then, Freddie. Wagon's fuelled up. Flask is full. Health and Safety says this mess gets cleaned up TODAY. HONK HONK!!"*

Five seconds per box. Twenty-five seconds total intro. Actual character development in a game about a bin man.

**Lesson 7: Iterate on the feel, not just the function.** "The dialogue doesn't feel long enough" and "the back and forth needs to feel like a real conversation" are completely valid creative notes. The AI understands creative direction, not just technical instructions.

---

## "Check the Order of the Levels"

Near the end, I asked for a final verification of the level structure:

> *"check the order of the levels and make sure it goes level 1,2 normal level 3 Freddie boss , level 4 gold bin bonus round level 5 normal , level 6 rat boss , level 7 normal level 8 hyper speed ."*

This revealed a significant hidden bug. When you defeated Freddie, the game was supposed to drop you into the Bonus Round (level 4) as a reward. But the code that set your position in the game used `frameCount = level * 1800`. At level 4, that's `4 × 1800 = 7200 frames`. The game looks at frameCount and calculates your level as `1 + floor(7200 / 1800) = 5`. Not 4. Level 5. The bonus round was being skipped entirely.

Same bug with the Rat King defeat — it was supposed to take you to level 7, but the frameCount was set to `7 × 1800 = 12600`, which maps to level 8 (Hyperspeed). The Rat King fight ended and immediately triggered Hyperspeed with no level 7 in between.

The fix was simple once spotted: use `(level - 1) * 1800` instead of `level * 1800`. One character. Two characters. The difference between the game working correctly and two entire levels being silently skipped.

**Lesson 8: Ask for verification, not just fixes.** "Check this is working the way I expect" catches things you never noticed. The bug existed since the level reorder was first done. Neither of us had caught it until a deliberate check was requested.

---

## What I've Learned About Vibe Coding

Looking back across all of this, here are the things I'd tell someone who wants to try vibe coding for the first time.

### You are the creative director. The AI is the whole technical team.
Your job is vision, taste, and feedback. The AI's job is everything else. This is a genuine collaboration, not you dictating and it obeying. It makes decisions you didn't ask about (spinning wheels, exhaust smoke, speech bubbles) and those decisions are usually good.

### Be specific about what it should *feel* like, not how it should *work*.
"Freddie should feel like a proper cartoon villain" produces better results than trying to specify technical details you don't understand. Describe personalities. Describe experiences. Describe emotions. Let the AI translate those into code.

### Describe symptoms, not solutions.
"Pressing UP causes a collision" is the right kind of bug report. "Change the barneyLane variable update threshold" is not something you'd ever say and not something you need to say. Describe what you see. Let the AI figure out why.

### One thing at a time, mostly — but batching is fine too.
Most of my prompts tackled multiple issues at once ("Freddie too small AND barrels not working AND level order wrong"). That works, especially for closely related issues. But for anything where you're not sure what's wrong, describe just the one thing you noticed. Easier to track what changed and why.

### Check your work.
Play the game after every change. Notice when something feels off. Ask for it to be fixed. The first version of something is never perfect — and that's fine. Vibe coding is iterative by nature. Each round of feedback makes it better.

### You can ask for verification.
"Check the collision mechanics work for all levels" is a real and useful thing to ask. You don't have to discover every bug through play. You can ask the AI to audit its own work.

### It will sometimes get things wrong.
The frameCount bug, the collision zone bug, the lane-snap bug — none of these were things I introduced. The AI wrote them. But crucially: it also found them and fixed them when asked. Knowing that bugs will happen and trusting the process of finding and fixing them is part of vibe coding.

---

## The Final Game

What started as *"can you vibe code a 2d pixel art game about barney the bin man"* became:

- 8 levels of increasing difficulty
- 2 boss fights with full dramatic intros
- A bonus round
- Pixel art characters with personality and animation
- Working collision detection
- Sound effects generated mathematically
- A dialogue system with proper back-and-forth
- A leaderboard
- Mobile touch controls
- Everything running instantly in any browser, in one file

I contributed zero lines of code. I contributed all of the creative direction.

The prompts are in this document, word for word, typos and all. That's the whole recipe.

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
