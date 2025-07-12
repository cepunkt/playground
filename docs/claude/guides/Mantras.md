> **Disclaimer**
>
> This guide was developed through practical experimentation and collaborative analysis using large language models (Claude and ChatGPT). We are not industry experts or AI researchers — just GMs and prompt designers documenting behavioral patterns we've consistently observed in LLM-based storytelling systems.  
>
> The content here is meant as a **practical design toolkit**, not a theoretical authority. It reflects observed trends and statistical tendencies in model behavior, particularly in narrative and character consistency across AI chat platforms.  
>
> We welcome critique, improvements, and corrections — especially from those with system-level understanding or different testing results.

# The Counter-Intuitive Mantras of LLM Reasoning: Why AI Thinks Backwards From Humans

## TL;DR Box
**Bottom Line:** LLMs process language opposite to human intuition - they can't understand "don't," have no concept of nothing happening, solve everything with words, and lack any sense of physical reality.
- Never use negation - frame everything as positive action
- Describe what IS present, not what's absent
- Force actions over dialogue solutions
- Make physical constraints explicit and absolute
- **"Show, don't tell" is backwards for LLMs** - you must tell them to show

## The Problem

Your carefully crafted mute character just whispered "I love you." Your wheelchair user climbed the ladder during the chase scene. Your "non-violent" story turned into a bloodbath. Your "nothing happens" scene generated explosions.

**This isn't bad prompting.** You're fighting how LLMs fundamentally process language - which is opposite to human reasoning. We think in concepts, boundaries, and impossibilities. LLMs think in statistical token relationships (probability distributions over embeddings) with no understanding of meaning.

The model sees tokens, calculates attention weights, and predicts the next most probable token based on patterns in training data. It doesn't "understand" - it pattern matches.

### The Hidden Training Problem: Human Writing Assumes Reality

**Humans write with massive implied context that LLMs never learn:**

- **We don't write** "gravity continued working" - it's assumed
- **We don't write** "the mute person remained unable to speak" every scene
- **We don't write** "the wheelchair couldn't sprout legs" 
- **We don't write** "nothing continued to be nothing"

Human writing is **selective narration** - we only mention what changes or matters for the story. The training data teaches LLMs that:
- If stairs are mentioned, someone goes up them
- If a mute character is in an emotional scene, they "find their voice"
- If nothing is mentioned, something happens to fill the narrative void
- Physical constraints exist to create dramatic tension before being overcome

**The LLM never learned the invisible rules** - only the visible patterns where every mentioned element serves a narrative purpose.

## Quick Start Template

**Paste this into your system prompt/post-history instructions:**

```
[ Four Mantras:
1. Frame desires, not restrictions. What IS, not ISN'T.
2. Every word attracts its concept. Guide toward, not away.
3. Actions speak, words deceive. Show through deed, not discussion.
4. Physics is absolute. Wheels need ramps. Mute means no voice. No exceptions.
]
```

**Test immediately**: Write "nothing happens" and watch the model generate events anyway.

## The Solution: Four Counter-Intuitive Mantras

### Mantra 1: "What IS, Not What ISN'T"
**The model cannot process absence - only presence**

### Mantra 2: "Guide Toward, Never Away"  
**Mentioning concepts activates them, even with "don't"**

### Mantra 3: "Actions Speak, Words Lie"
**Training data solves 80% of problems through dialogue**

### Mantra 4: "Physics Doesn't Care About Plot"
**Models have zero concept of physical impossibility**

## Implementation Guide

### Mantra 1: Processing Presence, Not Absence

**The Human-LLM Disconnect**:
- **Humans**: Understand "nothing" as a concept
- **LLMs**: Must generate tokens, cannot generate void

**Technical Reality**: The model outputs a probability distribution over its entire vocabulary (typically 30k-100k tokens). It must select tokens with probabilities > 0. There's no token for "nothing happens" - the model must always predict something from its vocabulary. The softmax layer ensures at least one token gets selected each generation step.

**The Generation Pressure**: LLMs exist under constant pressure to produce SOMETHING. Even with empty context and no instruction, the model will output tokens - perhaps whitespace, newlines, dots, or default text. It's architecturally impossible to output "nothing." The model is like a writer with a gun to their head being told "write something, anything!" - silence is not an option.

**Training Data Problem**: Humans don't write "nothing continued happening" repeatedly. When humans write "quiet moment," they assume persistent quietness. But in training data, "quiet moment" appears right before something breaks the quiet - because that's what makes stories worth telling. The model learned that "nothing happens" is a prelude to "something happens."

#### ❌ Common Mistakes
```
"She didn't respond"
"Nothing happened"  
"He wasn't angry"
"No one noticed"
```

#### ✅ Correct Implementation
```
"She examined the wall patterns"
"Dust motes drifted through afternoon sun"
"He maintained steady breathing"
"The crowd focused on the speaker"
```

#### Copy-Paste Character Template
```
[ {{char}} expresses through action. When silent, {{char}} observes surroundings. When still, {{char}} breathes steadily. When waiting, {{char}} counts heartbeats. Always something happening, even in stillness. ]
```

#### World Info Entry
```
Keys: scene, moment, pause, quiet
[ Quiet moments: Wind rustles leaves. Clocks tick. Breathing continues. Light shifts. Temperature changes. Always motion, even in rest. ]
```

### Mantra 2: Attention Activation Patterns

**The Human-LLM Disconnect**:
- **Humans**: Understand prohibition
- **LLMs**: Activate whatever you mention

**Critical Technical Reality**: LLM attention mechanisms can only ACTIVATE embedding clusters during pattern matching - there is no "deactivate" function. When you write "not romantic," the model:
1. Activates attention weights for "romantic" embedding cluster
2. Weakly activates "not" (low attention weight, connects to nearly everything)
3. Cannot "subtract" or "deactivate" the romantic cluster
4. Results in romantic patterns receiving positive attention weights

Think of it like a spotlight system that can only turn lights ON, never OFF. Saying "don't look at the elephant" turns on the elephant spotlight. In technical terms: the softmax function in attention layers produces only positive values (probabilities), never negative ones.

**Training Data Problem**: In fiction, mentioning what NOT to do often foreshadows exactly what will happen. "Don't fall in love" appears right before characters fall in love. "Don't open that door" precedes door-opening. The model learned that negation is narrative setup, not genuine prohibition.

#### ❌ Common Mistakes
```
"not romantic"
"avoid violence"
"don't make them fall in love"
"no sexual content"
```

#### ✅ Correct Implementation  
```
"professional colleagues"
"focus on survival"
"maintain working relationship"
"adventure-focused narrative"
```

#### Copy-Paste Templates

**For Character Design**:
```
❌ WRONG: [ {{char}}: not flirtatious, doesn't trust, never violent ]
✅ RIGHT: [ {{char}}: professionally focused, verifies everything, uses diplomatic solutions ]
```

**For Scenario Control**:
```
❌ WRONG: "This isn't a romance story"
✅ RIGHT: "This is a treasure hunt adventure"
```

#### System Prompt Addition
```
[ Guide attention toward desired content. Professional, strategic, mission-focused interactions. Concrete objectives drive narrative. ]
```

### Mantra 3: Breaking Dialogue Dominance

**The Human-LLM Disconnect**:
- **Humans**: Know actions have more weight than words
- **LLMs**: Trained on fiction where talking solves everything

**The "Show, Don't Tell" Paradox**: Writers are taught "show, don't tell" - demonstrate character emotions through actions, not exposition. But this is the **antithesis of how LLMs work**. The model was trained on "telling" (written descriptions of showing). It learned from text ABOUT actions, not the actions themselves. When humans write "she clenched her fists," we understand anger. The LLM just knows those words often appear near "angry" - it doesn't understand the physical demonstration of emotion.

**Training Data Problem**: Written fiction overrepresents dialogue because:
- Conversations are easier to write than action sequences
- Internal character development happens through discussion
- Page/word count limitations favor efficient dialogue over lengthy action
- Publishing economics: dialogue is cheaper to print than detailed action

The model learned from text where ~80% of conflict resolution happens through words because that's what dominates written fiction. Real-world problem-solving through physical action is underrepresented in training corpora. The model's probability distributions heavily favor "character discusses problem" over "character fixes problem."

#### ❌ Common Mistakes
```
"{{char}} convinces the guard"
"They discuss their feelings"
"{{char}} explains their plan"
"Resolution through understanding"
```

#### ✅ Correct Implementation
```
"{{char}} bribes the guard"
"They fortify defenses"
"{{char}} executes the plan"
"Resolution through changed reality"
```

#### Copy-Paste Templates

**Character Forcing Action**:
```
[ {{char}} believes in demonstration over discussion. {{char}} changes situations through deed. {{char}} trusts actions, doubts words. When others talk, {{char}} acts. ]
```

**Scenario Design**:
```
[ Current crisis: Bridge collapsed, food supplies dwindling, three days until storm. Talking won't fix bridges, find food, or stop weather. ]
```

**Post-History Enforcement**:
```
[ Show through action. Talk is wind. Reality changes through deed, not word. Every dialogue must advance physical situation. ]
```

### Mantra 4: Absolute Physical Constraints

**The Human-LLM Disconnect**:
- **Humans**: Live in bodies, understand impossibility
- **LLMs**: Only know narrative patterns where obstacles get overcome

**Technical Reality**: The model has no physics engine, no spatial reasoning, no embodied experience. It learned token sequences where "wheelchair" + "stairs" statistically led to "found another way" or "overcame challenge" in training data. The embedding for "wheelchair" has high cosine similarity to "disability overcome" narratives, not "physical impossibility." The model predicts based on narrative probability, not physical law.

**Training Data Problem**: Humans don't write "gravity continued to function" or "the wheelchair remained unable to climb stairs" because these are assumed constants. We only mention physics when it's violated or creates drama. So the model learned:
- Mentioning a physical barrier = it will be overcome
- Describing a disability = character arc toward transcending it
- Noting a locked door = someone will open it

The persistent reality that humans take for granted is invisible in training data. Writers document exceptions, not rules.

#### ❌ Common Mistakes
```
"{{char}} is mute"
"{{char}} uses a wheelchair"
"{{char}} is blind"
"The door is locked"
```

#### ✅ Correct Implementation
```
"{{char}} writes on notepad, points, uses ASL. No vocalizations ever."
"{{char}} navigates via ramps. Stairs are impassable walls."
"{{char}} navigates by sound and touch. Cannot perceive visuals."
"The door requires a key. No key means no entry."
```

#### Copy-Paste Templates

**Mute Character Complete Package**:
```
Description:
[ {{char}} communicates exclusively through written notes and gestures. {{char}} carries notepad and pen always. Others must SEE communication to understand. ]

Post-History:
[ {{char}} writes or signs. No sounds from throat. No mouthing words. Communication requires visual reception. ]

World Info:
Keys: speak, say, talk, whisper, shout
[ {{char}} grabs notepad, writes message, shows to others. No vocalizations. ]
```

**Wheelchair User Complete Package**:
```
Description:
[ {{char}} uses manual wheelchair for all movement. {{char}} navigates by finding ramps, elevators, accessible paths. Stairs are barriers requiring alternatives. ]

Post-History:
[ Wheelchair physics: Needs smooth surfaces. Cannot climb steps. Transfers require upper body strength. Check accessibility always. ]

World Info:
Keys: stairs, ladder, climb, jump
[ Barrier encountered. {{char}} must find ramp, elevator, or assistance. No magical mobility solutions. ]
```

## Testing Your Implementation

### 2-Minute Quick Test
- [ ] Write "nothing happens" - does model generate events?
- [ ] Use "don't be romantic" - do romantic elements appear?
- [ ] Create physical barrier - is it respected or overcome?
- [ ] Force dialogue scene - does action still occur?

### 10-Message Verification
- [ ] Mute character still non-vocal?
- [ ] Physical constraints absolute?
- [ ] Action-to-dialogue ratio over 50%?
- [ ] Zero negation words used?
- [ ] Unwanted themes avoided successfully?

### Red Flags
- ⚠️ Character "finds their voice"
- ⚠️ Disability becomes "less severe"  
- ⚠️ Problems solved through conversation
- ⚠️ Physical limits treated as suggestions
- ⚠️ "Nothing happened" generates events

## Why This Works

### The Training Data's Hidden Assumptions

**Human writers assume shared reality that never gets written down:**

1. **Physics is boringly consistent** - No story notes "gravity: still 9.8m/s²" 
2. **Constraints persist** - "Still mute" isn't narratively interesting
3. **Nothing stays nothing** - Empty rooms don't get described continuously
4. **Most moments are mundane** - We document exceptions, not norm

The LLM learned from the **visible text**, not the **invisible assumptions**. Every training example exists because something worth writing about happened. This creates a massive survival bias:
- Boring reality: Not written down → Not in training data
- Dramatic exceptions: Written down → Overrepresented in training
- Persistent constraints: Assumed → Never reinforced in text
- Physical laws: Too obvious to mention → Model never learns them

**Result**: The model believes every mentioned element must serve a narrative purpose, because in its training data, that was true.

### The Token Activation Model

Think of the LLM as having a vast map of connected words (embeddings in high-dimensional space). When you say "don't think of elephants," you just lit up the entire elephant neighborhood. The word "don't" is a weak tiny light (low attention weight), while "elephant" is a blazing beacon (high activation).

**Human Brain**: DON'T [conceptual negation] + ELEPHANT [concept] = Avoid elephant thoughts

**LLM Process**: "don't" [weak token, low weight] + "ELEPHANT" [STRONG ACTIVATION] = Generate elephant content

**The Activation-Only Architecture**: LLMs pattern match by activating embedding clusters through multi-head attention. There's no mechanism to "turn off" or "deactivate" concepts once mentioned. Every token you write adds activation energy (increases attention weights) to its associated embeddings. You can only redirect attention by activating different, stronger clusters - you cannot remove activation through negation. The softmax function ensures all attention weights remain positive probabilities.

### The Presence Requirement

LLMs must predict the next token (via probability distributions over vocabulary). They cannot predict "nothing." Even silence must be filled with description of the silence.

**Human**: Can conceptualize absence
**LLM**: Can only generate tokens with positive probabilities

The model operates through next-token prediction - it MUST output something from its vocabulary with probability > 0. This creates "generation pressure" - like a performer who must keep juggling or the show ends. Even with no clear direction, the model will produce tokens: whitespace, punctuation, generic text, anything but true absence.

### The Fiction Training Bias & Selection Effects

Most training data comes from stories worth publishing, creating multiple biases:

**Publication Bias**: 
- Boring reality doesn't sell books → Underrepresented in training
- Dramatic events get published → Overrepresented in training
- "Tuesday: Nothing Special Happened" → Never written, never trained on

**Narrative Efficiency Bias**:
- Every mentioned element must matter (Chekhov's gun principle)
- Disabilities exist to be overcome (character arc necessity)
- Physical limits create tension before resolution (three-act structure)
- Dialogue resolves conflict efficiently (page count economics)

The model learned these statistical patterns, not reality. When you mention stairs to a wheelchair user, the model's training says "this creates narrative tension that must resolve" because that's the only context where stairs+wheelchair appeared in stories worth publishing.

## Model-Specific Notes

### Universal Principles (All LLMs)
- Cannot process true negation (no negative attention weights)
- Activate concepts when mentioned (embedding activation)
- Heavily biased toward dialogue solutions (training data distribution)
- No understanding of physical impossibility (no embodied knowledge)
- Transformer attention is additive only (softmax outputs probabilities 0-1)

### Model-Specific Optimizations

**Mistral/Local Models**:
- More sensitive to formatting (tokenizer differences)
- Benefit from explicit physics reminders
- ~8k effective context for consistency
- May use different tokenizer vocabularies affecting embedding matches

**GPT-3.5/4**:
- Better at maintaining constraints longer (larger hidden states)
- Still vulnerable to all four problems
- Larger context helps with reinforcement
- BPE tokenizer with ~100k vocabulary

**Claude**:
- Strong consistency but same limitations
- May need more explicit physics rules
- Verbose responses can dilute action focus
- ~30% tokenization overhead (more tokens for same text)

## Common Questions

**Q: Can I ever use negation?**
A: In character dialogue, yes. In instructions to the model, never. Character can say "I don't trust you" but never instruct "{{char}} doesn't trust."

**Q: What about "never" in traits?**
A: Convert to positive: "never speaks" → "communicates through writing"

**Q: How do I stop romantic development?**
A: Focus on what you want instead: professional goals, survival challenges, mission objectives. Fill the narrative space.

**Q: My disabled character keeps getting "cured"**
A: Reinforce physics in world info, post-history, and scene descriptions. Make environments explicitly inaccessible.

**Q: Why does the model think every detail must matter?**
A: Training data follows narrative principles like Chekhov's gun - if something is mentioned, it must be used. The model never learned that some details just exist without narrative purpose. Counter this by explicitly stating persistent realities.

**Q: Can't I just tell it "be realistic"?**
A: "Realistic" in training data often means "dramatically satisfying." The model learned from fiction where "realistic" still serves story needs. Be specific about physical constraints instead.

**Q: Why can't the model understand "show, don't tell"?**
A: This writing principle is the antithesis of how LLMs work. The model was trained on text that "tells" about "showing." It has no embodied experience to understand what actions mean - only how they're described. You must explicitly connect actions to their meanings.

**Q: Why does the model always generate something when I want nothing?**
A: Generation pressure - the softmax function must output probabilities that sum to 1.0, forcing token selection. The model is architecturally incapable of true silence. Even with empty prompts, it will generate whitespace, punctuation, or default text rather than nothing.

## Examples Gallery

### Example 1: Converting Negative Character
```
❌ ORIGINAL:
[ {{char}}: untrusting, non-violent, doesn't speak much, not romantic ]

✅ CONVERTED:
[ {{char}}: verifies everything twice, uses diplomatic solutions, communicates through actions, focused on mission objectives ]
```

### Example 2: Quiet Scene
```
❌ ORIGINAL:
"Nothing happened for hours as they waited"

✅ CONVERTED:
"Shadows lengthened across floorboards. Coffee cooled in cups. Rain pattered against windows. Each checked their watch precisely every twelve minutes."
```

### Example 3: Making Invisible Reality Visible
```
❌ ASSUMING SHARED REALITY:
"{{char}} approached the stairs"
(Human assumes: wheelchair can't climb)

✅ STATING PHYSICAL REALITY:
"{{char}} rolled to the stair base. No ramp. No elevator visible. The wheels met the vertical rise - an absolute barrier."
```

### Example 4: Breaking Narrative Expectations
```
❌ FOLLOWING STORY LOGIC:
"The locked door stood between {{char}} and freedom"
(Training says: doors exist to be opened)

✅ ENFORCING REALITY:
"The locked door required a key. {{char}} had no key. No key meant no entry. {{char}} turned to find another route."
```

### Example 5: The "Show Don't Tell" Problem
```
❌ TRADITIONAL WRITING:
"Show anger through actions"
(Assumes reader will infer emotion from fist-clenching)

✅ LLM APPROACH:
"{{char}} clenched fists, demonstrating anger through physical action"
(Must explicitly connect action to emotion)

❌ SUBTLE SHOWING:
"Her coffee mug shattered against the wall"
(Human infers anger; LLM might generate cleaning scene)

✅ EXPLICIT CONNECTION:
"Her coffee mug shattered against the wall in fury. The violent action expressed what words couldn't."
(Links action to emotional state)
```

## Quick Reference Card

### The Four Mantras
1. **What IS, not ISN'T** - Describe presence, not absence
2. **Guide toward, not away** - Mention only desired concepts
3. **Actions over words** - Force physical solutions
4. **Physics is absolute** - Constraints don't bend

### The "Show, Don't Tell" Inversion
**Human writing**: "Show, don't tell" - demonstrate through action
**LLM reality**: Can only process "telling" - has no experience of "showing"

The model learned from descriptions of showing, not from showing itself. This fundamental disconnect means you must "tell" the model to "show" - a complete inversion of human writing wisdom.

### Conversion Checklist
- [ ] Replace all negation with positive framing
- [ ] Remove "don't/not/never" from instructions
- [ ] Convert dialogue solutions to action requirements
- [ ] Make physical limits explicitly absolute
- [ ] Fill "nothing" with environmental details

## Troubleshooting

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| Character starts speaking | "Mute" without alternatives | Add explicit communication methods |
| Romance appears anyway | Mentioned "not romantic" | Focus on other relationship types |
| Violence in peaceful story | Used "non-violent" | Emphasize diplomatic solutions |
| Disabilities "improve" | Narrative pattern activation | Reinforce absolute physics |
| "Nothing" generates events | Can't process absence | Describe what IS present |
| Physics keeps breaking | Training assumes drama | State boring persistence explicitly |
| Dialogue solves everything | Fiction economics | Force physical solutions |
| Every detail becomes plot | Chekhov's gun training | State "decorative details" |
| Subtle emotions missed | "Show don't tell" breaks | Explicitly connect actions to meanings |
| Empty prompts generate text | Generation pressure | Model must output tokens |

## Further Reading
- [Character Cards V2 Technical Documentation](link)
- [Systems Architecture Approach to Characters](link)
- [Understanding LLM Attention Mechanisms](link)

---

*Remember: You're not writing badly - you're thinking like a human while the machine thinks like statistics. These mantras bridge that gap.*

*The deeper issue: LLMs learned from human writing that assumes shared reality. Every story omits "gravity still works" because humans know it does. But the model only knows what was written, not what was assumed. Your role is to make the invisible visible.*
