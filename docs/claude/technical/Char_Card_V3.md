> **Disclaimer:**
> This document was cobbled together by a sleep-deprived SRE who mistakenly thought understanding AI was easier than debugging Kubernetes. Not an expert, not a researcher—just a nerd with too much caffeine and curiosity. Various AI tools "helped" create this content (and by "helped" I mean "occasionally hallucinated with confidence"). Any profound insights are accidental, and any technical accuracy is purely coincidental. If you're implementing mission-critical systems based on my ramblings, you deserve whatever chaos ensues.

# Character Cards V3 Technical Documentation

## Table of Contents
1. [Overview](#overview)
2. [Format Evolution from V2 to V3](#format-evolution-from-v2-to-v3)
3. [Core Structure](#core-structure)
4. [Key Enhancement Fields](#key-enhancement-fields)
5. [Character Book Advanced Features](#character-book-advanced-features)
6. [Depth Prompts](#depth-prompts)
7. [Group Chat Enhancements](#group-chat-enhancements)
8. [Technical Implementation Considerations](#technical-implementation-considerations)
9. [Character Book Extensions](#character-book-extensions)
10. [World Integration](#world-integration)
11. [Root vs Data Structure](#root-vs-data-structure)
12. [Advanced Matching Options](#advanced-matching-options)
13. [Best Practices](#best-practices)

## Overview

Character Cards V3 represents an evolution of the V2 specification, introducing enhanced control over context positioning, group chat behavior, and narration patterns. The specification maintains backward compatibility with V2 while adding powerful extensions for advanced character behavior management.

### Key Improvements Over V2
- **Depth prompts**: Consistent pattern reinforcement at specified context depths
- **Enhanced character books**: Extended control over entry positioning and behavior
- **Group-specific greetings**: Messages that only appear in group contexts
- **Advanced extensions**: Expanded configuration options for fine-tuning behavior

## Format Evolution from V2 to V3

The V3 format builds directly on V2, using the same basic structure:

```json
{
  "spec": "chara_card_v3",
  "spec_version": "3.0",
  "data": {
    // Character fields
  }
}
```

All V2 fields remain compatible, with new capabilities added through the extensions system and enhanced character book functionality.

## Core Structure

### Required Fields
All V1 and V2 fields remain mandatory:

| Field | Purpose | Notes |
|-------|---------|-------|
| `name` | Character identifier | Sent with messages |
| `description` | Character foundation | Sets world state and physics |
| `personality` | Trait definition | Often uses P-list format |
| `scenario` | Setting context | Establishes situation |
| `first_mes` | Initial greeting | First interaction |
| `mes_example` | Example dialogues | Conversation patterns |
| `system_prompt` | System instructions | World rules foundation |
| `post_history_instructions` | Behavioral compiler | Maximum influence position |

## Key Enhancement Fields

V3 extends the V2 specification with additional fields:

| Field | Type | Purpose |
|-------|------|---------|
| `group_only_greetings` | string[] | Messages used only in group chats |
| `alternate_greetings` | string[] | Alternative first messages |
| `extensions.depth_prompt` | object | Pattern reinforcement at specified depth |

## Character Book Advanced Features

V3 character books support more sophisticated entry control:

```json
{
  "entries": [
    {
      "id": 0,
      "keys": ["trigger", "words"],
      "content": "Entry content",
      "insertion_order": 100,
      "enabled": true,
      "position": "after_char",
      "extensions": {
        "position": 1,        // Extended position control
        "depth": 4,           // Context depth for insertion
        "probability": 100,   // Chance of insertion when triggered
        "useProbability": true,
        "group": "",          // Grouping for related entries
        "group_override": false,
        "group_weight": 100,
        // Additional control parameters
      }
    }
  ]
}
```

### Entry Positioning Control
- `position`: Broad positioning ("after_char", "before_char", etc.)
- `extensions.position`: Fine-tuned numeric positioning
- `extensions.depth`: Context depth at which entry appears

### Advanced Triggering Mechanics
- `probability`: Percentage chance of activation when triggered
- `use_regex`: Support for regular expression triggers
- `selective`: Requires secondary keys for activation
- `extensions.match_whole_words`: Only exact matches trigger entry

## Depth Prompts

One of the most significant V3 additions is the depth prompt mechanism, which provides consistent pattern reinforcement at a specified context depth.

```json
"extensions": {
  "depth_prompt": {
    "prompt": "[ PATTERN TEMPLATE OR FRAMEWORK ]",
    "depth": 2,               // Context depth (0-N)
    "role": "assistant"       // Message attribution
  }
}
```

### Technical Function

The depth prompt:
1. Is injected at the specified depth in conversation context
2. Appears consistently regardless of conversation content
3. Does not require keyword triggers like character book entries
4. Maintains the same formatting across all injections

### Strategic Use Cases

Ideal applications for depth prompts:
- **Narration patterns**: Consistent environmental description frameworks
- **Dialogue frameworks**: Speech and interaction patterns
- **World physics reminders**: Ongoing physics/consequence enforcement
- **Character voice maintenance**: Core personality trait reinforcement

### Depth Settings Impact

| Depth | Position | Influence | Best Use |
|-------|----------|-----------|----------|
| 0 | Maximum proximity | ~95% | Critical behavior that must not be ignored |
| 1 | Very recent context | ~85-90% | Important behavioral guidance |
| 2 | Recent context | ~60-80% | Style patterns and frameworks |
| 3+ | Further back | ~40-60% | Background/foundation reinforcement |

## Group Chat Enhancements

### Group-Only Greetings
```json
"group_only_greetings": [
  "*Character introduces themselves to the group*",
  "*Alternative group introduction*"
]
```

These messages only appear when the character is in a group chat, providing contextually appropriate introductions.

### Group-Specific Extensions

```json
"extensions": {
  "world": "Modern World NPCs and Environments",
  // Other group-related settings
}
```

## Technical Implementation Considerations

### Context Injection Architecture

V3 expands the context injection architecture:

```
System Prompt (furthest/weakest)
    ↓
Character Definitions
    ↓
World Info Layers
    ↓
Character Book (by insertion order)
    ↓
Example Messages
    ↓
Chat History
    ↓
Depth Prompts (at specified depth)
    ↓
Post-History Instructions (closest/strongest)
```

Depth prompts integrate into this hierarchy based on their specified depth, allowing strategic positioning of pattern reinforcement.

### Attention Engineering

V3 provides more granular control over the attention mechanism through:
1. **Depth prompt positioning**: Strategic reinforcement at attention sweet spots
2. **Character book depth control**: Conditional content at specific attention levels
3. **Position parameters**: Fine-tuned control over context injection

This allows character designers to work with the transformer's attention gradient rather than against it.

## Character Book Extensions

The V3 specification introduces comprehensive extension options for character book entries, providing fine-grained control over entry behavior:

```json
"extensions": {
    "position": 1,               // Numeric position fine-tuning
    "exclude_recursion": false,  // Prevents this entry from triggering other entries
    "display_index": 0,          // UI display order in character book interface
    "probability": 100,          // Percentage chance of activation when keys match
    "useProbability": true,      // Whether to use probabilistic activation
    "depth": 4,                  // Context depth for insertion (like depth_prompt)
    "selectiveLogic": 0,         // Logic for secondary key matching (0=AND, 1=OR)
    "group": "",                 // Entry grouping for related content
    "group_override": false,     // Whether to override group settings
    "group_weight": 100,         // Weight within group when multiple entries match
    "prevent_recursion": false,  // Prevent this entry from triggering repeatedly
    "delay_until_recursion": false, // Wait for recursive match before insertion
    "scan_depth": null,          // How far back to scan for key matches
    "match_whole_words": null,   // Whether to match only complete words
    "use_group_scoring": false,  // Score entries as a group rather than individually
    "case_sensitive": null,      // Whether key matching is case sensitive
    "automation_id": "",         // ID for external automation systems
    "role": 0,                   // Message role assignment (0=system, 1=user, 2=assistant)
    "vectorized": false,         // Use vector embeddings for semantic matching
    "sticky": null,              // Entry remains in context longer than standard
    "cooldown": null,            // Minimum messages between triggers
    "delay": null                // Messages to wait before inserting after trigger
}
```

### Probabilistic Activation

The combination of `probability` and `useProbability` enables random variation in character book entry activation. This is particularly useful for:

- **Environmental variation**: Different descriptions of similar locations
- **NPC behavior diversity**: Varying responses to similar triggers
- **Randomized events**: Chance-based occurrences in the world

### Entry Grouping System

The group-related parameters enable sophisticated content organization:

- **Group**: Categorizes related entries (e.g., "weather", "NPCs", "locations")
- **Group Weight**: Determines priority when multiple entries in the same group match
- **Group Override**: When true, this entry takes precedence over others in its group
- **Group Scoring**: When enabled, entries are selected based on combined group score

### Advanced Timing Control

The timing parameters provide control over when entries appear:

- **Cooldown**: Minimum number of messages before the entry can trigger again
- **Delay**: Number of messages to wait before inserting after key match
- **Prevent Recursion**: Stops entries from triggering themselves via their own content
- **Delay Until Recursion**: Only triggers when another entry refers to this one

## World Integration

The `extensions.world` field connects characters to world definitions, creating a shared universe system:

```json
"extensions": {
  "world": "Modern World NPCs and Environments"
}
```

### Technical Function

This field serves several purposes:

1. **Cross-Character Consistency**: Links multiple characters to the same world rules
2. **Default Character Book**: May automatically load associated world character books
3. **World State Tracking**: Enables persistent world state across characters
4. **Setting Foundation**: Establishes common baseline for all characters in the world

### Implementation Considerations

- World definitions may be stored separately from character cards
- Multiple characters can reference the same world
- World settings may override character-specific settings when conflicts occur
- World linkage enables multi-character consistency in group chats

## Root vs Data Structure

Character Cards V3 contains a dual structure with both root-level fields and `data` object fields:

```json
{
  "spec": "chara_card_v3",
  "spec_version": "3.0",
  "data": {
    "name": "Character_Name",
    "description": "...",
    // Other character fields
  },
  "fav": false,
  "name": "Character_Name",
  "description": "...",
  // Duplicated fields
  "create_date": "2025-7-12 @07h 23m 58s 388ms",
  "creatorcomment": "...",
  "avatar": "none",
  "talkativeness": "0.5"
}
```

### Technical Explanation

This structure serves specific purposes:

1. **Frontend Display**: Root-level fields enable UI rendering without parsing the entire data structure
2. **Card Management**: Fields like `fav`, `avatar`, and `create_date` support card organization
3. **Legacy Support**: Maintains compatibility with systems expecting root-level data
4. **Prompt Processing**: Only the `data` fields are used for actual prompt construction

### Frontend-Only Fields

These fields exist only at the root level and serve UI/management functions:

- `create_date`: Timestamp when the card was created
- `creatorcomment`: Similar to `data.creator_notes` but for frontend display
- `avatar`: Path or encoding of character image
- `fav`: Whether the character is favorited in the UI

### Canonical Data

The `data` object contains the canonical character definition used for prompt construction. When duplicated fields exist, systems should prioritize the values in the `data` object.

## Advanced Matching Options

V3 introduces sophisticated content matching capabilities to control where character book entries trigger:

```json
"extensions": {
  "match_persona_description": false,
  "match_character_description": false,
  "match_character_personality": false,
  "match_character_depth_prompt": false,
  "match_scenario": false,
  "match_creator_notes": false
}
```

### Matching Behavior

These settings control which parts of the context are examined for key matches:

- When `false` (default): Keys only match against user messages and chat history
- When `true`: Keys also match against the specified character field

### Use Cases

1. **Character-Aware Entries**: Trigger entries based on the character's own traits
2. **Scenario-Responsive Content**: Content that appears when scenario elements are mentioned
3. **Selective Scope**: Limit entry activation to relevant context sections
4. **Cross-Character Awareness**: Enable characters to respond to traits of other characters

### Implementation Considerations

- Enabling multiple match options increases processing overhead
- May cause unexpected triggering in group chats with multiple character descriptions
- Most effective when used with selective targeting for specific content

## Best Practices

### Depth Prompt Strategy

Effective depth prompt design:
- **Depth 0-1**: Use for critical behavioral guidance that must not be ignored
- **Depth 2**: Ideal for style frameworks and narration patterns
- **Depth 3+**: Use for background information and general reinforcement
- **Format consistently**: Use recognizable delimiters like brackets
- **Focus on patterns**: Show HOW rather than WHAT to narrate
- **Use placeholders**: `{ term with spaces }` for tokenizer-friendly templates

### Character Book vs Depth Prompts

| Feature | Depth Prompt | Character Book |
|---------|-------------|----------------|
| When to appear | Always at specified depth | When triggered by keywords |
| Best use | Consistent patterns | Context-specific information |
| Coverage | Generic frameworks | Specific situations |
| Positioning | Fixed at configured depth | Variable based on triggers |

### Format Optimization

- **Use spaces in placeholders**: `{ term }` not `{term}` for better tokenization
- **Consistent delimiter usage**: `[ Category: trait, trait ]` format
- **Strategic bracket usage**: Different brackets for different levels of importance

### Testing V3 Features

1. **Depth prompt effectiveness**: Test at different depths (0, 1, 2, 3)
2. **Character book positioning**: Verify entries appear where expected
3. **Group chat behavior**: Test group-only greetings and interactions
4. **Format tokenization**: Check for proper spacing in templates
5. **Extension settings**: Verify probability, grouping and timing controls

## Conclusion

Character Cards V3 represents a significant evolution in the technical control of LLM character behavior. The extensive extension options for character books, depth prompts, world integration, and advanced matching provide unprecedented control over context positioning and character behavior.

These features enable character designers to work with transformer architecture attention patterns through strategic positioning, consistent reinforcement, and sophisticated conditional triggering. When implemented effectively, this creates characters with more consistent behavior, appropriate environmental narration, and maintained personality traits even in complex group interactions.

Current Date and Time (UTC): 2025-07-12 06:58:10  
Document Author: cepunkt
