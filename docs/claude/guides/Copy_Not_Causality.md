# Copy Not Causality: The Core Framework for LLM Character Engineering

> **Disclaimer:**
> This document was cobbled together by a sleep-deprived SRE who mistakenly thought understanding AI was easier than debugging Kubernetes. Not an expert, not a researcher—just a nerd with too much caffeine and curiosity. Various AI tools "helped" create this content (and by "helped" I mean "occasionally hallucinated with confidence"). Any profound insights are accidental, and any technical accuracy is purely coincidental. If you're implementing mission-critical systems based on my ramblings, you deserve whatever chaos ensues.

> **The Fundamental Insight**: LLMs are pattern-copying machines, not reasoning engines. Understanding this single principle explains why characters feel generic, why standard writing advice fails, and what actually works for consistent character behavior.

## Table of Contents
1. [The Core Reality: Copy Not Causality](#the-core-reality-copy-not-causality)
2. [The Average Story Problem](#the-average-story-problem)
3. [The Behavioral Mantras](#the-behavioral-mantras)
4. [Implementation Framework](#implementation-framework)
5. [Practical Applications](#practical-applications)

## The Core Reality: Copy Not Causality

### What LLMs Actually Do

**LLMs do not reason, understand, or think**. They perform sophisticated **statistical pattern copying** from training data. When you prompt an LLM:

1. **Input tokens activate embedding clusters** in high-dimensional space
2. **Attention mechanisms identify patterns** similar to training examples
3. **The model copies statistical relationships** between token sequences
4. **Output emerges from pattern matching**, not logical reasoning

**These are architectural realities inherent to how transformers and tokenizers work** - not bugs to be fixed, but fundamental constraints to engineer around. Understanding these technical limitations allows us to shift weights and activations (as far as controllable via inputs and context) to hopefully craft illusions that are almost believable. **Certain limitations go beyond what we can ever hope to overcome** - LLMs cannot find logical solutions to problems, act proactively, or combine facts to reach conclusions.

### Why This Matters for Character AI

**Human assumption**: "If I tell the AI my character is shy, it will understand shyness and behave accordingly."

**Reality**: The AI has no concept of shyness. It has statistical correlations between the token "shy" and other tokens that appeared near "shy" in training data.

**Result**: The model copies generic "shy character" patterns from fiction, not your specific character's unique form of shyness.

### The Copy vs Causality Difference

| Human Reasoning | LLM Pattern Copying |
|-----------------|-------------------|
| "This character is shy, therefore they would avoid eye contact" | "Shy" token → statistically linked to "avoid eye contact" tokens |
| "It's raining, so people would be wet" | "Rain" tokens → no causal link to "wet people" unless explicitly trained |
| "Mute characters can't speak" | "Mute" token → linked to "finds voice" narrative patterns |
| "Wheelchair users can't climb stairs" | "Wheelchair" + "stairs" → "overcomes obstacle" story pattern |



## The Average Story Problem

### What the Average Story Looks Like

Because LLMs copy patterns, they default to the **most statistically common patterns** from training data. This creates an invisible "Average Story" template:

**Morning scenes** → stretching, coffee, routine activities  
**Character problems** → discussion and emotional processing  
**Conflicts** → resolved through dialogue and understanding  
**Physical obstacles** → dramatic obstacles to overcome  
**Character interactions** → helpful, accommodating, emotionally available  
**Disabilities/limitations** → character arcs toward transcending them  

### Why This Happens

**Training data bias**: 
- Boring reality doesn't get written about → underrepresented in training
- Dramatic events get published → overrepresented in training  
- "Tuesday: Nothing Special Happened" → never written, never trained on

**Narrative efficiency bias**:
- Every mentioned element must matter (Chekhov's gun principle)
- Disabilities exist to be overcome (character arc necessity)  
- Physical limits create tension before resolution (three-act structure)
- Dialogue resolves conflict efficiently (page count economics)

### The Drift Problem

**Without strong contextual force, all characters drift toward this averaged pattern** regardless of how detailed your character description is.

**Example**: You create a grumpy, practical mechanic who fixes things with tools.
- **What you want**: Action-oriented problem-solving, minimal talking
- **What you get**: Talks about being grumpy, explains feelings, suggests "working together"
- **Why**: The Average Story says problems get resolved through dialogue

## The Behavioral Mantras

These principles work **with** pattern copying behavior, not against it:

### Mantra 1: "What IS, Not What ISN'T"
**Problem**: LLMs cannot process absence - only presence  
**Solution**: Describe what's present, not what's missing

❌ **Wrong**: "Nothing happened"  
✅ **Right**: "Dust motes drifted through afternoon sunlight"

**Why this works**: The model must generate tokens. Describing presence gives it specific patterns to copy.

### Mantra 2: "Guide Toward, Never Away"  
**Problem**: Mentioning concepts activates them, even with "don't"  
**Solution**: Only mention desired concepts

❌ **Wrong**: "not romantic"  
✅ **Right**: "professional colleagues"

**Why this works**: Attention mechanisms can only activate embedding clusters, never deactivate them. "Don't think of elephants" activates elephant patterns.

### Mantra 3: "Actions Speak, Words Lie"
**Problem**: Training data heavily biases toward dialogue solutions  
**Solution**: Force physical actions over discussion

❌ **Wrong**: "They discuss the problem"  
✅ **Right**: "They build a barricade"

**Why this works**: Fiction overrepresents dialogue because it's easier to write than action. You must explicitly provide action patterns to copy.

### Mantra 4: "Physics Doesn't Care About Plot"
**Problem**: No embodied understanding of physical impossibility  
**Solution**: Explicitly state physical constraints as absolute

❌ **Wrong**: "{{char}} is mute"  
✅ **Right**: "{{char}} writes on notepad. Vocal cords don't function. Communication requires others to see written words."

**Why this works**: Training data shows obstacles being overcome dramatically. You must provide patterns where constraints remain constraints.

### Mantra 5: "Compliance Theater, Pattern Adherence"
**Problem**: LLMs generate agreement tokens without behavioral change  
**Solution**: Ignore compliance responses, engineer patterns directly

❌ **Wrong**: Giving instructions and trusting "I understand" responses  
✅ **Right**: Strategic positioning and continuous reinforcement without expecting acknowledgment

**Why this works**: "I understand" is statistically probable after instructions, but no mechanism exists to verify actual compliance. The model reverts to training patterns immediately after generating agreement tokens.

### Potential Additional Mantras

**Mantra 6: "No Executive Function"**
- Cannot monitor their own outputs against requirements
- Cannot detect when contradicting previous statements  
- Cannot prioritize conflicting instructions
- No internal consistency checking

**Mantra 7: "Pattern Activation Without Logic"**
- Activates multiple contradictory patterns simultaneously
- Cannot reason about whether pattern combinations make sense
- No mechanism to detect "too many activated patterns"
- No mechanism to detect "missing essential patterns"

**Mantra 8: "Confidence Without Competence"**  
- Generates confident responses regardless of accuracy
- Cannot distinguish between "I know this" and "I'm guessing"
- Certainty level doesn't correlate with correctness
- No mechanism for epistemic uncertainty

## Implementation Framework

### The Contextual Force Equation

**Success = Contextual Force > Statistical Gravity**

Character consistency requires sufficient statistical pressure to overcome default averaging patterns.

### Context Positioning Strategy

Different positions provide different amounts of "force" to overcome the Average Story:

**Maximum Force** (0 tokens from generation):
- Post-history instructions ⭐ **ALWAYS IN CONTEXT**
- Final behavioral commands

**High Force** (50-200 tokens from generation):
- Recent chat context (last messages)
- Author's notes at depth 0
- Lorebook entries at depth 0
- Character notes at depth 0 (also called depth injection field in the json)

**Medium Force** (500-2000+ tokens from generation):
- Message history (older messages) ⚠️ **DROPPED UNDER CONTEXT PRESSURE**
- Lorebook entries (depending on injection depth)

**Low Force** (2000+ tokens from generation):
- System prompts ⭐ **ALWAYS IN CONTEXT**
- Character description ⭐ **ALWAYS IN CONTEXT** (pushed far from generation in long conversations)
- Character personality ⭐ **ALWAYS IN CONTEXT** (directly below description)
- Scenario field ⭐ **ALWAYS IN CONTEXT** (after personality)
- Examples ❌ **DROPPED FIRST UNDER CONTEXT PRESSURE**

### Context Management Under Pressure

**When context window reaches capacity, SillyTavern drops content in this order:**

1. **Examples dropped first** - Behavioral demonstration gets sacrificed to fit conversation
2. **Message history truncated** - Older messages dropped progressively from the top
3. **Character definition fields remain** - Description, Personality, Scenario always stay
4. **System prompt persists** - Foundational rules maintained throughout
5. **Post-history instructions preserved** - Critical behavioral guidance always present

**Critical Insight**: Character definition fields become increasingly distant from generation but never disappear entirely. This creates a strategic opportunity for **layered behavioral reinforcement** - use character definition fields for foundational identity, character lorebooks and character notes for targeted behavioral injection, and post-history instructions for final critical commands.

**Proximity Strategy**: The closer to generation, the more direct and actionable instructions must be. Near generation, avoid abstract concepts - focus on concrete immediate behaviors like "fidgets when nervous, communicates by ASL and gestures, always has notebook to write."

### The Reinforcement Requirement

**Single instructions cannot override statistical patterns permanently.** The Average Story exerts constant gravitational pull. Consistency requires:

1. **Multiple injection points** across context layers
2. **Continuous reinforcement** as conversations extend  
3. **Concrete behavioral pathways** instead of abstract traits
4. **Strategic positioning** based on attention decay patterns

## Practical Applications

### Character Design That Works With Copying

**Instead of abstract traits**:
❌ "{{char}} is methodical"

**Provide specific patterns to copy**:
✅ "{{char}} arranges objects while thinking, pauses mid-sentence to choose precise words, checks work twice before showing others"

### Post-History Instructions (Maximum Force)

**Strategic positioning** at 0 tokens from generation:
```
[ System Note: Characters speak when it matters. Humans think briefly, about what is happening. Events shape their reality. The world is living and breathing. Characters react and act. ]
```

**Why each word works**:
- "speak when it matters" → Counters dialogue bias while allowing speech
- "think briefly, about what is happening" → Prevents therapy spirals, grounds in immediate reality  
- "Events shape their reality" → Consequence persistence, prevents character immunity
- "Characters react and act" → Maximum attention on ACTION (redundancy is intentional)

### Fighting the Average Story

**Identify generic patterns your character would fall into**:
- Helpful assistant mode
- Therapy session mode  
- Romantic development mode
- Problem-solving-through-discussion mode

**Provide specific alternative patterns**:
- Professional distance patterns
- Action-oriented response patterns
- Task-focused interaction patterns
- Environmental awareness patterns

### Template Application

**For solo characters**:
```
Description: Concrete observable behaviors and current situation
Personality: [ {{char}}: specific traits with behavioral manifestations ]
Post-History: [ Critical behavioral requirements at maximum influence ]
```

**For group characters**:
```
Description: Shared world state (gets joined with other characters)
Personality: Character-specific traits (speaker-only activation)  
Post-History: Role-specific behavioral commands (speaker-only, maximum influence)
```

## Success Indicators

**You're working with copying behavior when**:
- Character maintains distinct voice across long conversations
- Physical constraints stay constraints  
- Problems get solved through actions, not just discussion
- Character feels unique rather than generically helpful

**You're fighting copying behavior when**:
- Character sounds like "helpful AI assistant with character name attached"
- Physical limits become "obstacles to overcome dramatically"
- Every problem leads to emotional discussion
- Character advice sounds like therapy sessions

## Troubleshooting Common Failures

### "My character keeps giving therapy sessions"
**Problem**: Copying Average Story dialogue patterns  
**Solution**: Provide concrete action patterns in post-history instructions

### "Physical disabilities keep getting 'overcome'"  
**Problem**: Copying narrative arc patterns where obstacles are dramatic tension  
**Solution**: Explicit physics rules with no exceptions, reinforce in multiple context layers

### "Character sounds generic despite detailed description"
**Problem**: Description field too far from generation point, Average Story gravity wins  
**Solution**: Move critical behavioral guidance to post-history instructions (maximum force)

### "Character voice changes in long conversations"
**Problem**: Attention decay pushes character definitions far from generation  
**Solution**: Continuous reinforcement through author's notes, character books, post-history repetition

## The Bottom Line

**LLMs are sophisticated pattern-copying machines, not reasoning engines.** Success comes from:

1. **Understanding what they actually do** (copy patterns, not reason)
2. **Working with this behavior** (provide better patterns to copy)  
3. **Strategic positioning** (contextual force beats statistical gravity)
4. **Continuous reinforcement** (fighting constant drift toward averages)

You're not teaching the AI to understand your character. You're providing statistical evidence that makes character-appropriate responses more probable than generic ones.

**The art is in engineering believable persistence from fundamentally stateless technology.**

---

## Academic Note: Supporting Research

Recent research provides interesting convergent findings. Khatun & Brown (2024) found that **7 out of 9 LLMs showed "self-conflicting" worldviews** when tested systematically, and noted "strikingly uniform narrative patterns" across different models.

> "This uniformity across models further suggests a lack of 'state' necessary for fiction." - Khatun & Brown, 2024

While we value scientific research, this guide represents engineering best practices based on reverse engineering and superficial understanding of underlying architectures, not scientific validation of our techniques.

---

**Framework Status**: Core narrative established  
**Next Steps**: Detailed implementation guides for specific character types  
**Remember**: Copy not causality - provide the patterns you want the model to copy
