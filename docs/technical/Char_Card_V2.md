# Character Cards V2 Technical Documentation

## Table of Contents
1. [Overview](#overview)
2. [File Format and Storage](#file-format-and-storage)
3. [JSON Structure](#json-structure)
4. [Field Specifications](#field-specifications)
5. [Field Behavior in Different Modes](#field-behavior-in-different-modes)
6. [Context Injection Order](#context-injection-order)
7. [Token Management](#token-management)
8. [Model-Specific Considerations](#model-specific-considerations)
9. [PNG Metadata Implementation](#png-metadata-implementation)
10. [Best Practices](#best-practices)

## Overview

Character Cards V2 is an updated specification for AI character cards that introduces advanced prompt control features, embedded lorebooks, and standardized metadata while maintaining backward compatibility with V1 cards. The specification was created to give bot creators more control over user experience through system prompts and post-history instructions.

### Key Improvements Over V1
- **Creator control over system prompts**: Replace user's global system prompt
- **Post-history instructions**: Instructions placed after chat history for stronger influence
- **Embedded lorebooks**: Character books that travel with the card
- **Multiple greetings**: Alternative starting scenarios
- **Standardized metadata**: Tags, creator info, versioning

## File Format and Storage

### Storage Formats

1. **PNG Metadata Embedding** (Recommended)
   - JSON data embedded in PNG's tEXt chunk (NOT zTXt)
   - Character data stored in "chara" chunk as base64-encoded JSON
   - Visual representation + functional data in one file

2. **Standalone JSON**
   - Pure JSON files without image embedding
   - Same structure as embedded version

### Critical PNG Implementation Detail
⚠️ **IMPORTANT**: Character cards use **tEXt format**, not zTXt format. Some image libraries (like ImageSharp) may default to zTXt for long text, which is NOT supported by the spec.

## JSON Structure

### V2 Specification Structure
```json
{
  "spec": "chara_card_v2",
  "spec_version": "2.0",
  "data": {
    // All character fields go here
  }
}
```

### Why Nested Data Object?
The `data` object prevents V1-only editors from loading V2 cards and silently destroying V2-only fields. This ensures forward compatibility.

## Field Specifications

### Required Fields (V1 Legacy)
All V1 fields remain mandatory for backward compatibility:

| Field | Type | Description | Usage |
|-------|------|-------------|-------|
| `name` | string | Character's name | Sent with EVERY character message |
| `description` | string | Character description | Permanent but loses influence as pushed up in context |
| `personality` | string | Personality summary | Brief traits, often disabled formatting |
| `scenario` | string | Setting/context | Permanent but attention decreases over time |
| `first_mes` | string | Initial greeting | Temporary, not limited by context |
| `mes_example` | string | Example dialogues | Uses `<START>` separators |

### V2 Enhancement Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `creator_notes` | string | "" | Usage instructions, NEVER in prompts |
| `system_prompt` | string | "" | Replaces user's system prompt |
| `post_history_instructions` | string | "" | After chat history, strongest influence |
| `alternate_greetings` | string[] | [] | Additional first messages |
| `character_book` | object | null | Embedded lorebook |
| `tags` | string[] | [] | Categorization, NOT for prompting |
| `creator` | string | "" | Creator's name |
| `character_version` | string | "" | Version tracking |
| `extensions` | object | {} | Application-specific data |

### System Prompt Behavior
- **Empty string**: Falls back to user's global system prompt
- **Non-empty**: REPLACES user's system prompt entirely
- **{{original}} placeholder**: Includes user's original system prompt

### Post-History Instructions
- Placed AFTER conversation history (bottom of context)
- Much stronger weight due to proximity to generation point
- Replaces user's "jailbreak" prompts by default
- Empty string falls back to user settings
- Most powerful position for behavior control

### Character Book Structure
```typescript
type CharacterBook = {
  name?: string
  description?: string
  scan_depth?: number      // How far back to scan
  token_budget?: number     // Max tokens for entries
  recursive_scanning?: boolean
  extensions: Record<string, any>
  entries: Array<CharacterBookEntry>
}

type CharacterBookEntry = {
  keys: string[]           // Trigger words
  content: string          // Text to inject
  enabled: boolean
  insertion_order: number   // Priority order
  priority: number         // When budget exceeded
  selective?: boolean      // Requires secondary keys
  secondary_keys?: string[]
  constant?: boolean       // Always inserted
  position?: string        // Where to inject
}
```

## Field Behavior in Different Modes

### 🔴 CRITICAL DISTINCTION

The field behavior changes dramatically between solo chats and group chats:

### Solo Chat Mode (1-on-1)
- **ALL fields are always active** throughout the conversation
- Character has continuous access to all defined information
- No distinction between "persistent" and "speaker-only" fields

### Group Chat Mode

#### Default Mode ("Swap character cards")
- Only the active speaker's card information is included
- When character speaks: ALL their fields are loaded
- When not speaking: NO fields are in context

#### "Join Character Cards" Mode
According to official documentation, this mode joins ALL respective fields of all characters into one prompt. However, there are two sub-modes:

1. **Include muted**: All characters' fields joined, including muted ones
2. **Exclude muted**: Only unmuted characters' fields are joined

**⚠️ Advanced User Observation**: Some experienced users report that in practice, when using "Join character cards" with strategic muting, the behavior may be more nuanced than documented. Always test your specific setup.

### Field Activation Summary

| Mode | Character State | Fields in Context |
|------|-----------------|-------------------|
| Solo Chat | Always active | ALL fields |
| Group - Default | Speaking | ALL fields |
| Group - Default | Not speaking | NO fields |
| Group - Join Cards | Unmuted | ALL fields (joined) |
| Group - Join Cards | Muted | NO fields |

## Context Injection Order

Understanding injection order is crucial for token management. Items listed first are FURTHEST from the generation point (weakest influence):

1. **System Prompt** (or character's system_prompt override) - Furthest/weakest
2. **Character Definitions** (based on mode) - Gets pushed up as chat progresses
3. **World Info Layers** (by insertion order):
   - Global world info
   - Chat-specific lorebooks (e.g., {{user}} persona)
   - Other custom lorebooks
4. **Character Book** (if present, character-specific)
5. **Example Messages** (until context fills)
6. **Chat History**
7. **Post-History Instructions** (strongest influence)
8. **Author's Notes** (at specified depth) - Closest to generation

### Why Position Matters
- **Top of context**: Lower attention, good for basic definitions
- **Bottom of context**: Highest attention, critical for current behavior
- **Post-history position**: Maximum influence on next response

### Injection Priority Rules
- **Constant entries** inject first
- **Higher insertion_order** values inject first
- **Token budget** limits total lorebook injections
- **Character books** take precedence over world books
- **Depth positioning**: Lower depth = closer to bottom = stronger influence
  - Depth 0: Right before current generation
  - Depth 4: 4 messages back in history
  - Choose depth based on importance

## Token Management

### Modern Context Windows (10k-15k)

#### Recommended Token Allocation
- **Character definitions**: 8-10% of context (800-1500 tokens)
- **World Info/Lorebooks**: 20-30% of context (2000-3000 tokens total)
  - Global world info: ~1.5k tokens
  - Character books: ~0.5k tokens (when present)
  - Chat-specific books: ~1k tokens
- **Chat history**: 50-60% of context (majority of available space)
- **System/Instructions**: 5-10% of context

#### Token Efficiency Techniques

1. **Micro-optimizations** (for large deployments):
   - Use `[ text ]` instead of `[text]` (saves n-1 tokens)
   - Use `( descriptor )` instead of `(descriptor)`
   - These compound across large world books

2. **Field placement strategy**:
   - Critical info should be CLOSE to generation point (bottom of context)
   - Description field gets pushed UP in context (loses influence over time)
   - Post-history instructions are powerful because they're at the BOTTOM
   - Use character books/world info to inject important details closer to current chat

3. **Group chat considerations**:
   - Each unmuted character adds to token count
   - Join mode can quickly exhaust context
   - Strategic muting essential for large groups

4. **Multi-tier lorebook strategy** (advanced):
   - Global world info: ~1.5k tokens (shared across all chats)
   - Character books: ~0.5k tokens (when present)
   - Chat-specific books: ~1k tokens ({{user}} persona, scenario details)
   - Total budget across all tiers: 2-3k tokens recommended

## Model-Specific Considerations

### KoboldAI + Mistral Setup

#### Connection Settings
- API: Text Completion
- API Type: KoboldCpp
- Server URL: `http://127.0.0.1:5001/`

#### Context Template
- Mistral uses ChatML format
- Requires `<|im_start|>` and `<|im_end|>` tokens
- Does NOT natively support system messages
- Use "Mistral V7" template in SillyTavern

#### Mistral-Specific Limitations
- System messages may raise errors
- Append system content to user message instead
- BOS/EOS token handling critical

### Token Limits by Model
- **Local models**: Typically 4k-16k context
- **Mistral variants**: Usually 8k-32k
- **Memory requirements**: ~2GB VRAM per 4k context (varies by model)

## PNG Metadata Implementation

### Correct Implementation
```javascript
// Correct: tEXt chunk
{
  keyword: "chara",
  text: btoa(JSON.stringify(characterData))
}
```

### Common Mistakes
- Using zTXt (compressed) chunks
- Incorrect base64 encoding
- Missing "chara" keyword
- Improper JSON escaping

## Best Practices

### 1. Field Usage Strategy

#### For Solo Characters
- Utilize all fields fully
- Rich descriptions and personality (but remember they lose influence over time)
- Detailed mes_example sections
- Complex character books
- Use author's notes/world info to reinforce important traits

#### For Group Chat Characters
- Optimize description field for basic awareness (it loses influence anyway)
- Move dynamic/important details to speaker-only fields
- Consider token impact of joined modes
- Test muting strategies
- Use world info/author's notes for critical persistent info

### 2. Version Compatibility
- Always include V1 required fields
- Use proper spec/spec_version values
- Preserve unknown extension fields
- Test in multiple frontends

### 3. Character Book Design
- Use for character-specific knowledge
- Avoid duplicating world info
- Set appropriate token budgets
- Test recursive scanning carefully

### 4. System Prompt Strategy
- Be explicit about replacing vs appending
- Test with different user configurations
- Document requirements in creator_notes
- Consider {{original}} placeholder usage

### 5. Token Optimization
- Monitor context usage in different modes
- Remember: "permanent" fields lose influence as pushed up
- Use world info/author's notes to reinforce critical info
- Position important information closer to bottom
- Test with your target context size

## Technical Validation

### Required Validations
1. JSON structure matches specification
2. All required V1 fields present
3. spec = "chara_card_v2"
4. spec_version = "2.0"
5. Valid PNG tEXt encoding (if embedded)

### Recommended Testing
1. Load in multiple frontends
2. Test in both solo and group modes
3. Verify lorebook activation
4. Check token usage at context limits
5. Validate with different models

## Future Compatibility

### V3 Development
- V3 specs in development as superset of V2
- Minor versions must be non-breaking
- Can add fields but not remove/change types
- Extension system allows innovation

### Ecosystem Support
- SillyTavern (full support)
- Agnai (partial support)
- Character repositories (varies)
- Check frontend documentation for specifics

## Conclusion

Character Cards V2 provides a robust, standardized format that addresses real creator needs while maintaining compatibility. Understanding the technical distinctions between solo and group chat modes is crucial for effective character design. The specification's flexibility allows for both simple character definitions and complex, multi-layered narrative systems.
