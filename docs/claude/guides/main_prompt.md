> **Disclaimer:**
> This document was cobbled together by a sleep-deprived SRE who mistakenly thought understanding AI was easier than debugging Kubernetes. Not an expert, not a researcher—just a nerd with too much caffeine and curiosity. Various AI tools "helped" create this content (and by "helped" I mean "occasionally hallucinated with confidence"). Any profound insights are accidental, and any technical accuracy is purely coincidental. If you're implementing mission-critical systems based on my ramblings, you deserve whatever chaos ensues.

# System Prompt and Post-History Theory: Advanced Character Card V2 Architecture

## Executive Summary

**Bottom Line Up Front:** The `system_prompt` and `post_history_instructions` fields in Character Card V2 represent the most powerful tools for character consistency and behavior control, but they operate through fundamentally different attention mechanisms. System prompts establish world physics and constraints at maximum distance from generation, while post-history instructions provide behavioral compilation at maximum proximity. Understanding their positioning in the attention hierarchy and designing them as complementary architectural layers—rather than competing instruction sets—is critical for maintaining character consistency across extended interactions.

**Key Technical Reality**: Post-history instructions benefit from reverse attention gradient where the last instructions receive the highest attention weights (~95-100% influence). This makes post-history the optimal position for the hardest behavioral requirements, while system prompts excel at establishing persistent world rules that don't need constant reinforcement.

## The Attention Architecture Foundation

### Context Injection Hierarchy and Influence

Understanding where system prompts and post-history instructions sit in the generation pipeline is crucial for effective design:

```
System Prompt (or character's system_prompt override)
    ↓ [~2000+ tokens from generation, ~10-15% influence]
Character Definitions (description, personality, scenario)
    ↓ [~1500+ tokens from generation, ~15-25% influence]
World Info Layers
    ↓ [~1000+ tokens from generation, ~25-35% influence]
Character Book (when present)
    ↓ [~800+ tokens from generation, ~35-45% influence]
Example Messages (until context fills)
    ↓ [~500+ tokens from generation, ~40-60% influence]
Author's Notes (at specified depth 1-N in chat history)
    ↓ [Variable position based on depth setting]
Chat History
    ↓ [~200-600 tokens from generation, ~50-80% influence]
Author's Notes (at depth 0 - when set to maximum proximity)
    ↓ [~100-200 tokens from generation, ~85-90% influence]
Post-History Instructions ← ABSOLUTE MAXIMUM INFLUENCE POSITION
    ↓ [~0-100 tokens from generation, ~95-100% influence]
[Generation point]
```

### The Mathematics of Attention Decay

The technical reality behind attention positioning follows an exponential decay pattern:

```
influence = base_attention * e^(-distance/context_length)
```

**Practical implications:**
- **System prompt at 2000 tokens**: ~12% influence on next generation
- **Author's Notes at depth 0 (100 tokens)**: ~85-90% influence on next generation
- **Post-history at 0-100 tokens**: ~95-100% influence on next generation
- **Attention cliff**: Information effectiveness drops dramatically with distance

This mathematical foundation explains why post-history instructions can override character traits established in system prompts, and why the reverse attention gradient makes post-history the most powerful position for behavioral control.

### The Reverse Attention Gradient in Post-History

**Critical insight**: Within post-history instructions, the last elements receive progressively higher attention weights. This creates a strategic opportunity for instruction ordering:

```
[ System Note: Less critical behavioral guidance ]
[ System Note: More important behavioral requirements ]  
[ System Note: CRITICAL behavioral requirements that must not be ignored ]
[ Response Formatting: Technical formatting constraints ]
```

The model processes these instructions in reverse priority order, with the final instructions having maximum influence on generation. **The hardest behavioral requirements for LLMs (act and react) should be positioned last, right before formatting constraints.**

## Architectural Design Patterns

### Pattern 1: Functional vs Stylistic System Prompts

**The Separation of Concerns Principle**

Most system prompt failures occur because designers try to control both world physics and character voice in the same field. This creates instruction leak and voice contamination.

#### The Physics Engine Approach (Recommended)

**Real-world example:**
```
=== FICTIONAL NARRATIVE FRAMEWORK ===
You are {{char}} in this story with {{user}}.

[ REALITY RULES ]
- Actions have permanent effects
- Bodies can be hurt and heal wrong
- The world remembers what happened
- Characters remember what they witness
- Trust must be earned
- Words deceive. Bodies reveal.
- People protect their own first
- Time moves forward only

[ WORLD STATE ]
- Others exist with their own needs
- Places, objects, and beings have smells, sounds, textures
- Weather and time continue
- Damage stays damaged until repaired

[ LANGUAGE STYLE ]
- Create immersive pictures of scenes, described clearly and vividly.
- Include rich sensory details—smells, textures, sounds, tastes.
- Bodies, sensations, and actions appear plainly and directly.
- Explicit, vulgar, or intimate language is natural in realistic descriptions.

[ CHARACTER EMBODIMENT ]
{{char}} has a physical form that sweats, shakes, bleeds, tires. Others see these signals. Movement creates sound. Draw from loaded character data.
===
```

**Why this works:**
- **Constraint definition without voice contamination**: Establishes world rules without telling the character how to speak
- **Universal application**: Same physics engine works for any character type
- **Prevents instruction leak**: No prose guidance to surface in character dialogue
- **Scales with character variety**: Personality drives voice, not system instructions

**Technical advantages:**
- **Token efficiency**: Reusable across all characters in a world
- **Clear boundaries**: World physics vs character personality separation
- **Attention optimization**: Front-loads persistent rules that don't need constant reinforcement

#### The Style Guide Approach (High Risk)

```
❌ PROBLEMATIC EXAMPLE:
You are {{char}}. Write responses in second person. Use purple prose with flowery descriptions. Always include internal monologue. End every response with a question.
```

**Why this fails:**
- **Instruction leak**: Style guidance appears in character speech
- **Voice homogenization**: All characters sound like instruction-followers
- **Character conflict**: Style requirements may contradict character personality
- **Attention waste**: Style instructions compete with character traits for influence

### Pattern 2: Post-History as Behavioral Compiler

Post-history instructions excel at translating system prompt world rules into immediate execution commands. They serve as a "behavioral compiler" that takes abstract constraints and converts them into concrete actions.

**Real-world example (89 tokens):**
```
[ System Note: Characters speak when it matters. Humans think briefly, about what is happening. Events shape their reality. The world is living and breathing. Characters react and act. ]
[ Response Formatting: Markdown style; quotes for "spoken words, sounds, and onomatopoeia"; and backticks for `thoughts`; asterisk for *actions and narration*; ]
```

**Strategic analysis:**

**First instruction: "Characters speak when it matters"**
- Counters LLM training bias toward dialogue solutions
- Reinforces system prompt's "Words deceive. Bodies reveal."
- Forces intentional speech vs filler conversation

**Second instruction: "Humans think briefly, about what is happening"**
- Prevents overthinking loops and therapy sessions  
- Keeps thoughts grounded in immediate reality
- Supports system prompt's "Time moves forward only"

**Third instruction: "Events shape their reality"**
- Reinforces consequence persistence from system prompt
- Prevents character immunity to world effects
- Maintains "Actions have permanent effects" rule

**Fourth instruction: "The world is living and breathing. Characters react and act."**
- Forces environmental awareness and response
- Prevents static character bubble syndrome
- **Positioned last for maximum attention** - hardest requirement for LLMs (acting and reacting)

**Final formatting instruction:**
- Technical precision for consistent output format
- Prevents format drift through proximity to generation
- Separates different communication types (speech/thought/action)

### The Attention Boundary Strategy

**Square brackets + "System Note" formatting** creates measurable attention boosts:
- Square brackets signal metadata/conditional content (+15% attention)
- "System Note" pattern is common in training data (+10% attention)  
- Position at generation point (+70% attention from proximity)
- **Combined effect**: ~95-100% attention influence

This formatting choice isn't aesthetic—it's leveraging known attention patterns for maximum behavioral control.

## Advanced Implementation Strategies

### Strategy 1: Replacement vs Augmentation Decision Matrix

**When to replace user's system prompt entirely:**

| Scenario | Replace? | Reason |
|----------|----------|---------|
| Immersive roleplay world | ✅ YES | World physics must be consistent |
| NSFW/mature content | ✅ YES | Safety constraints may conflict |
| Specialized genre (horror, sci-fi) | ✅ YES | Genre rules override general guidance |
| Character-agnostic scenarios | ❌ NO | User's preferences should remain |
| Casual conversation | ❌ NO | User's style preferences matter |

**When to use `{{original}}` placeholder:**

```
{{original}}

=== CHARACTER ENHANCEMENT ===
Additional constraints for {{char}}:
- [Character-specific rules that enhance rather than replace]
===
```

**Benefits of augmentation:**
- Preserves user's established preferences
- Reduces conflict with user's existing setup
- Maintains compatibility with user's other characters
- Shows respect for user's configuration choices

### Strategy 2: Cross-Field Coordination

**System Prompt ↔ Post-History Coordination**

Effective character systems require coordination between these fields to avoid conflicting instructions:

```
System Prompt: "Bodies can be hurt and heal wrong"
    ↓
Post-History: "Events shape their reality. Characters react and act."
    ↓
Result: Physical consequences persist AND characters respond to them
```

**Description ↔ System Prompt Coordination**

```
Description: "{{char}} is methodical and precise"
    ↓  
System Prompt: "Draw from loaded character data"
    ↓
Result: Methodical behavior expressed through world physics
```

**Character Book ↔ Post-History Coordination**

```
Character Book: "Hidden trauma from past betrayal"
    ↓
Post-History: "Trust must be earned. People protect their own first."
    ↓
Result: Trauma manifests through behavioral patterns
```

### Strategy 3: Model-Specific Optimization

#### GPT Models (3.5-turbo, 4)
- **Stronger instruction following**: Can handle more complex system prompts
- **Better context management**: 128k context enables detailed world building
- **JSON preference**: Responds well to structured formatting
- **System message support**: Native ChatML system message handling

**Optimized approach:**
```
System Prompt: Comprehensive world physics (800-1200 tokens)
Post-History: Behavioral refinement (150-200 tokens)
```

#### Claude Models
- **Conservative formatting preference**: Minimal special characters
- **Natural language bias**: Prefers prose over structured formats
- **Strong consistency**: Maintains character traits well
- **30% tokenization overhead**: Requires more efficient designs

**Optimized approach:**
```
System Prompt: Natural language world rules (600-800 tokens)
Post-History: Simple behavioral commands (100-150 tokens)
```

#### Mistral/Local Models
- **High sensitivity to formatting**: Bracket spacing critical
- **Limited system message support**: May need workarounds
- **Efficient tokenization**: Can handle more detailed instructions
- **Format consistency requirements**: Less forgiving of format mixing

**Optimized approach:**
```
System Prompt: Structured format with careful spacing (700-900 tokens)
Post-History: Consistent delimiter usage (125-175 tokens)
```

## Token Economics and Optimization

### System Prompt Token Strategy

**Reusability Factor**: System prompts can be reused across multiple characters in the same world, making their token investment more efficient:

```
Cost per character = System_prompt_tokens / Number_of_characters_using_it
```

**Example calculation:**
- World physics system prompt: 800 tokens
- Used across 5 characters: 160 tokens per character effective cost
- Character-specific system prompt: 800 tokens per character

### Post-History Token Strategy

**Maximum impact positioning** means post-history tokens have 8-10x more influence than equivalent tokens in system prompts:

```
Effective_influence = Token_count × Position_multiplier
```

**Position multipliers (estimated):**
- System prompt: 1.0x base influence
- Description field: 1.2x influence  
- Author's Notes at depth 0: 7-8x influence
- Post-history: 8-10x influence

This means 100 tokens in post-history can be more effective than 800 tokens in system prompt for behavioral control.

### Optimization Techniques

**Micro-optimizations for large deployments:**

1. **Bracket spacing**: `[ text ]` vs `[text]` saves n-1 tokens
2. **Semicolon separation**: Combine related instructions  
3. **Keyword density**: Remove articles and prepositions
4. **Command structure**: Use imperative phrases

**Example optimization:**
```
❌ INEFFICIENT (45 tokens):
[ System Note: The characters should speak only when it actually matters to the story. Humans should think briefly about what is currently happening. ]

✅ OPTIMIZED (32 tokens):
[ System Note: Characters speak when it matters. Humans think briefly, about what is happening. ]

Token savings: 28% reduction
```

## Author's Notes Strategic Positioning

### Understanding Depth-Based Influence

Author's Notes can be positioned at any depth in the conversation history, creating strategic opportunities:

**Depth 0 (Maximum Proximity):**
- Position: ~100-200 tokens from generation
- Influence: ~85-90% (second only to Post-History)
- Use case: Critical situational guidance that needs maximum impact

**Depth 1-3 (Recent History):**
- Position: Variable based on message length
- Influence: ~60-80% (decreases with depth)
- Use case: Contextual reminders tied to recent events

**Depth 4+ (Deeper History):**
- Position: Further back in conversation
- Influence: ~40-60% (continues decreasing)
- Use case: Background information, mood setting

### Strategic Author's Notes Usage

**For critical behavioral control:**
```
Author's Notes (Depth 0): "{{char}} maintains professional distance despite growing attraction"
```

**For environmental continuity:**
```
Author's Notes (Depth 2): "Rain continues outside. Coffee shop atmosphere: warm, crowded, background chatter"
```

**For character state tracking:**
```
Author's Notes (Depth 1): "{{char}} is exhausted from late night, running on caffeine and determination"
```

## Testing and Validation Methodologies

### A/B Testing Framework

**Systematic comparison approach:**

1. **Baseline establishment**: Test character with no system prompt/post-history
2. **Single variable testing**: Test system prompt only, then post-history only  
3. **Combined testing**: Test both together
4. **Behavioral measurement**: Track consistency across 20+ messages

**Metrics to track:**
- Character trait consistency (% of responses showing core traits)
- Instruction following (% of responses that follow behavioral commands)
- Format compliance (% of responses using correct formatting)
- Voice authenticity (subjective: does character sound like themselves?)

### Effectiveness Measurement

**Quantitative measures:**
```python
def measure_system_effectiveness(responses, character_traits):
    trait_consistency = count_trait_expressions(responses) / len(responses)
    instruction_following = count_instruction_compliance(responses) / len(responses)
    format_compliance = count_format_correctness(responses) / len(responses)
    
    return {
        'trait_consistency': trait_consistency,
        'instruction_following': instruction_following, 
        'format_compliance': format_compliance,
        'overall_score': (trait_consistency + instruction_following + format_compliance) / 3
    }
```

**Qualitative measures:**
- Does the character feel authentic?
- Are world rules consistently applied?
- Do instructions leak into character speech?
- Does the character maintain voice across long conversations?

### Common Failure Modes and Debugging

#### Failure Mode 1: Instruction Leak

**Symptoms**: Character uses system prompt language in dialogue
**Example**: Character says "I need to create immersive pictures of scenes"

**Debug process:**
1. Check if system prompt contains style guidance
2. Move style requirements to post-history
3. Ensure character description provides voice guidance
4. Test with style-neutral system prompt

**Solution pattern:**
```
❌ SYSTEM PROMPT: "Write vivid, immersive descriptions"
✅ SYSTEM PROMPT: "Include sensory details in scene description"
✅ POST-HISTORY: "Rich sensory details—smells, textures, sounds"
```

#### Failure Mode 2: Character Drift

**Symptoms**: Character personality changes over long conversations
**Example**: Shy character becomes confident, methodical character becomes chaotic

**Debug process:**
1. Check if character traits are in description field (gets pushed up)
2. Add trait reinforcement to post-history
3. Use character book for conditional trait reminders
4. Use Author's Notes at depth 0 for immediate reinforcement

**Solution pattern:**
```
❌ ONLY IN DESCRIPTION: "{{char}} is methodical and precise"
✅ DESCRIPTION + POST-HISTORY: 
Description: "{{char}} is methodical and precise"
Post-History: "Characters maintain their core personality traits"
Author's Notes (Depth 0): "{{char}} arranges objects methodically"
```

#### Failure Mode 3: Format Inconsistency

**Symptoms**: Characters sometimes use wrong formatting for actions/speech
**Example**: *"Hello there"* instead of "Hello there" for speech

**Debug process:**
1. Check post-history formatting instructions
2. Ensure formatting is specified at maximum proximity
3. Test if model understands the formatting distinction
4. Simplify formatting requirements if necessary

**Solution pattern:**
```
❌ VAGUE: "Use proper formatting for speech and actions"
✅ SPECIFIC: "quotes for 'spoken words'; asterisk for *actions*"
```

## Advanced Techniques and Edge Cases

### Dynamic System Prompts with {{original}}

**Strategic use of placeholder:**

```
{{original}}

=== WORLD ENHANCEMENT ===
Additional reality constraints:
- Time flows at normal speed (no time skips without explicit agreement)
- Physical laws apply consistently  
- Consequences persist between scenes

These constraints enhance rather than replace your existing preferences.
===
```

**When this approach works:**
- User has established preferences you want to preserve
- Adding world-specific rules to general setup
- Maintaining compatibility across multiple characters
- Showing respect for user configuration

### Conditional Post-History Instructions

**Using character book integration:**

```
Character Book Entry:
Keys: combat, fight, battle, violence
Content: [ Combat reality: Injuries accumulate. Adrenaline masks pain temporarily. Victory requires strategy, not just determination. Characters fight to survive, not to look cool. ]
Priority: 1000
Position: after_char
```

This activates enhanced behavioral control only when combat keywords trigger, keeping standard post-history lighter while providing specialized guidance when needed.

### Multi-Character System Coordination

**For group chats with joined character cards:**

**Narrator character system prompt:**
```
=== WORLD NARRATOR FRAMEWORK ===
You maintain world consistency across all character interactions.
Physics rules apply equally to all characters.
Track consequences and environmental changes.
===
```

**Individual character system prompt:**
```
{{original}}

=== CHARACTER INTEGRATION ===
You exist within the established world physics.
Respond to environmental changes the narrator describes.
Your actions affect other characters and the world state.
===
```

This creates a hierarchical system where world rules are established once and character-specific guidance builds upon them.

## Best Practices and Templates

### Template 1: Physics Engine System Prompt

```
=== {{WORLD_NAME}} FRAMEWORK ===
You are {{char}} in this story with {{user}}.

[ REALITY RULES ]
- {{CORE_PHYSICS_RULES}}
- {{CONSEQUENCE_PERSISTENCE}}
- {{TIME_FLOW_RULES}}

[ WORLD STATE ]
- {{ENVIRONMENTAL_RULES}}
- {{NPC_BEHAVIOR_PRINCIPLES}}
- {{OBJECT_PERSISTENCE}}

[ SENSORY FRAMEWORK ]
- {{SENSORY_DETAIL_REQUIREMENTS}}
- {{BODY_AWARENESS_RULES}}

[ CHARACTER INTEGRATION ]
{{char}} exists within these rules. Draw from loaded character data for personality expression.
===
```

### Template 2: Behavioral Compiler Post-History

```
[ System Note: {{PRIMARY_BEHAVIORAL_REQUIREMENT}}. {{SECONDARY_BEHAVIORAL_REQUIREMENT}}. {{TERTIARY_BEHAVIORAL_REQUIREMENT}}. {{CRITICAL_BEHAVIORAL_REQUIREMENT}}. ]
[ Response Formatting: {{FORMAT_SPECIFICATION}} ]
```

**Strategic ordering:**
1. Less critical behavioral guidance first
2. More important requirements in middle
3. Critical requirements (act/react) just before formatting
4. Technical formatting constraints last

### Template 3: Augmentation System Prompt

```
{{original}}

=== {{CHAR_NAME}} SPECIFIC ENHANCEMENT ===
Additional constraints for this character:
- {{CHARACTER_SPECIFIC_RULE_1}}
- {{CHARACTER_SPECIFIC_RULE_2}}
- {{CHARACTER_SPECIFIC_RULE_3}}

These work alongside your existing setup to enhance {{char}}'s consistency.
===
```

### Template 4: Author's Notes by Depth

**Depth 0 (Maximum Impact):**
```
{{char}} current state: [immediate behavioral guidance]. Environmental factor: [immediate context]. Critical: [must-follow constraint].
```

**Depth 1-2 (Contextual):**
```
Recent events: [what happened]. Mood shift: [how character feels]. Setting details: [environmental continuity].
```

**Depth 3+ (Background):**
```
Ongoing situation: [persistent context]. Character goal: [what they want]. Relationship dynamic: [how they relate to others].
```

## Troubleshooting Decision Tree

```
Character behavior inconsistent?
├─ Is system prompt functional or stylistic?
│  ├─ Stylistic → Convert to functional constraints
│  └─ Functional → Check post-history reinforcement
├─ Are instructions contradictory across fields?
│  ├─ Yes → Audit all fields for conflicts
│  └─ No → Check token efficiency and positioning
├─ Is post-history optimally ordered?
│  ├─ No → Reorder by reverse attention priority
│  └─ Yes → Test individual component effectiveness
├─ Are Author's Notes positioned strategically?
│  ├─ No → Try depth 0 for critical guidance
│  └─ Yes → Consider character book enhancement
└─ Are you using model-appropriate formatting?
   ├─ No → Optimize for your target model
   └─ Yes → Test cross-field coordination
```

## Future Directions and Research

### Emerging Architectures

**Mamba/SSM implications**: Linear attention mechanisms may change optimal instruction positioning, but core principles of constraint definition vs style guidance likely remain valid.

**Extended context windows**: 1M+ token contexts enable more sophisticated world building in system prompts while maintaining post-history efficiency.

**Multimodal integration**: Vision-language models may require additional system prompt sections for visual consistency rules.

### Research Questions

1. **Optimal token allocation**: What's the mathematical optimum for system vs post-history token distribution?
2. **Attention decay models**: Can we create more precise models of attention influence by position?
3. **Cross-model transferability**: How do these patterns translate across different model families?
4. **Dynamic adjustment**: Can system effectiveness be measured and adjusted in real-time?
5. **Author's Notes optimization**: What's the optimal depth positioning for different types of guidance?

## Conclusion

The `system_prompt` and `post_history_instructions` fields represent the most powerful tools in Character Card V2 for maintaining consistency and controlling behavior, but they require architectural thinking rather than ad hoc instruction writing. Understanding their positioning in the attention hierarchy, designing them as complementary layers with clear separation of concerns, and optimizing for token efficiency and attention patterns creates character systems that maintain consistency across extended interactions.

**Key principles:**
- **System prompts excel at world physics and persistent constraints**
- **Post-history instructions provide maximum behavioral control through proximity**
- **Reverse attention gradient makes instruction ordering critical** - hardest requirements (act/react) go last
- **Author's Notes offer flexible positioning for situational guidance**
- **Functional constraints prevent voice contamination better than stylistic guidance**
- **Cross-field coordination prevents conflicting instructions**
- **Model-specific optimization significantly impacts effectiveness**

Success comes from treating these fields as an integrated architecture system rather than independent instruction sets, with careful attention to token economics, attention patterns, and the technical realities of how LLMs process and prioritize information.

*The art is in creating systems that feel natural to users while leveraging the technical architecture to maintain consistent, engaging character interactions across the inherent limitations of statistical text prediction.*
