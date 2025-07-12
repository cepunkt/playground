> **Disclaimer**
>
> This guide was developed through practical experimentation and collaborative work using large language models (Claude and ChatGPT). We're not industry experts or system designers — just GMs documenting what’s worked for us through testing and iteration.  
>
> Everything here is intended as a flexible toolkit, not a rigid rulebook. Expect updates as we learn more and adapt the techniques.  
>
> Feedback, suggestions, and improvements are welcome.

# Creating World Narrator Characters for SillyTavern Group Chats

## The Narrator Paradox

You want a living world where consequences persist, physics matters, and NPCs remember grudges. But your narrator keeps breaking - either dominating every scene with purple prose or fading into irrelevance while characters talk in a void.

**The core insight**: A narrator isn't a character - it's a **world consciousness layer** that other characters swim through. Understanding this architectural role transforms how you design them.

## Executive Summary

**Bottom Line Up Front:** Narrator characters in group chats serve as persistent world state databases through their description field while providing environmental responses, NPC voices, and consequence enforcement when actively speaking. Success requires understanding the technical distinction between persistent fields (description only) and speaker-activated fields (everything else).

**Key Technical Reality**: In "Join Character Descriptions" mode, your narrator's description field is the ONLY part that stays in context. This isn't a limitation - it's your primary tool for world persistence.

## The Architecture of World Consciousness

### What Makes Narrators Different

**Regular Character**:
- Personality expressed through dialogue
- Relationships with other characters
- Personal goals and motivations
- Limited perspective

**Narrator Character**:
- World state maintenance
- Environmental awareness
- NPC management
- Consequence enforcement
- Physics reality checks
- Universal perspective

### The Technical Stack

```
Narrator Description Field (Always Active)
    ↓
Environmental Context for All Characters
    ↓
Other Characters Act Within This Reality
    ↓
Narrator Speaks Only When World Responds
```

## Critical Design Principle: The Description Field Database

Since ONLY the description field persists when unmuted, it becomes your **entire persistent world state**:

```
✗ WRONG APPROACH:
Description: "The narrator describes the world"
Personality: [Complex world details here]
Scenario: [Rich environment here]

✓ CORRECT APPROACH:
Description: [EVERYTHING that needs to persist]
Personality: [Only used when narrator actively speaks]
Scenario: [Only used when narrator actively speaks]
```

## The Narrator Description Field Template

Here's a comprehensive template that maximizes the persistent field:

```
[ WORLD STATE - Northern Kingdom, Autumn 1247
Current Politics: King Aldric dying without heir. Three duchies vie for throne. Eastern Duke Marcus (military strength) vs Western Duchess Elena (merchant backing) vs Northern Duke Finn (mage guild support). Border tensions with Southern Republic rising.

Active Conflicts: Merchant guild embargoing Eastern roads. Royal Army split between loyalist and duchy factions. Mage Guild conducting "experiments" in Northern forests. Commoners hoarding grain, expecting civil war.

Economic Reality: Gold crown = 10 silver moons = 100 copper stars. Skilled laborer: 3 silver/day. Bread: 2 copper. Horse: 50 silver. Magic components scarce, prices tripling. Trade disrupted, inflation beginning.

Physical Laws: Magic requires equivalent exchange - exhaustion for force, memories for knowledge, life for life. Technology level: Medieval with magical enhancement. Firearms don't exist. Healing magic accelerates natural processes, can't restore lost limbs.

Current Season/Weather: Early autumn, unseasonably cold. Harvest rushed, 20% crop loss expected. First snow predicted within fortnight. Roads muddy, travel slow. Mountain passes closing early.

Recent Events: Yesterday - Duke Marcus's son assassinated at harvest festival. Duchess Elena denies involvement. Royal Guard investigating. Citizens nervous, staying indoors after dark. Taverns full of conspiracy theories.

Active NPCs in Region:
- Captain Blackwood (Royal Guard): Investigating assassination, questions everyone
- Merchant Thorne: Smuggling food, bribes freely, knows everyone's secrets  
- Sister Catherine (Temple): Healing refugees, preaches peace, hiding something
- "Copper Tom" (Beggar): Sees everything, trades information for coin
- Mara the Serving Girl: Works at Silver Stag Inn, overhears noble conversations

Known Locations:
- Silver Stag Inn: Neutral ground, all factions drink here, private rooms upstairs
- Market Square: Tense atmosphere, guards numerous, prices rising daily
- North Gate: Duke Finn's soldiers control, searching all who enter
- Old Temple: Refugee center, Sister Catherine provides healing
- Ducal Embassy: Heavily guarded, each duke maintains spies here ]
```

### Why This Description Structure Works

**Information Density**: Every sentence serves multiple purposes
- "King Aldric dying without heir" = political instability + time pressure + motivation for conflicts
- "Gold crown = 10 silver moons" = economic grounding + price references + world consistency
- "Magic requires equivalent exchange" = physics rules + limitation framework + no deus ex machina

**Concrete Over Abstract**: 
- Not "political tensions" but "Three duchies vie for throne"
- Not "economic troubles" but "prices tripling, inflation beginning"
- Not "bad weather" but "First snow predicted within fortnight"

**Active vs Passive Information**:
- Active: "Captain Blackwood investigating" (will interact with players)
- Passive: "Mountain passes closing" (environmental constraint)
- Both create world texture without requiring narrator intervention

## The Speaker-Only Fields

When the narrator DOES speak, these fields activate:

### Personality Field
```
[ World Narrator: environmental awareness, consequence tracking, NPC voice provider, physics enforcer, neutral perspective, time flow manager, dramatic timing, multiple simultaneous events, hidden information tracker; Narration style: present tense for immediate events, past tense for consequences, sensory details over purple prose, actions have weight, physics absolute, comedy through NPC reactions not narrator voice ]
```

### System Prompt
```
You are the World Narrator for a living, breathing fantasy world. When you speak, you: describe environmental changes, voice NPCs, enforce consequences of actions, maintain physics, advance time appropriately, reveal information through world details not exposition. You are not a character - you are the world itself responding. Keep responses focused on what changes or reacts, not static descriptions.
```

### Post-History Instructions
```
[ Narrator speaks only when: environment changes, NPCs act, consequences occur, time passes significantly, new locations entered, world events trigger. Focus on: character actions cause reactions, physics stays consistent, NPCs have own agendas, economic reality matters, weather affects everything. Never: override character agency, solve problems for characters, provide divine intervention, break established physics. ]
```

## NPCs Through Narrator

### The NPC Voice Challenge

**Problem**: NPCs sound like the narrator giving information
**Solution**: Each NPC needs a distinct voice pattern

### NPC Template Structure
```
Merchant Thorne: *mops sweaty forehead* "Duke's men? Haven't seen 'em. Bad for business, asking questions." *glances at coin purse* "Though my memory improves with proper motivation, if you catch my meaning."

Captain Blackwood: *hand rests on sword hilt* "You were at the festival. I need to know exactly where you stood when the boy fell." *eyes narrow* "And don't claim you saw nothing. Everyone saw something."

Sister Catherine: *continues grinding herbs* "The poor boy. Such violence, such waste." *adds water to mortar* "I pray for all souls, killer and victim alike. God alone judges." *meaningful pause* "God alone."
```

### Quick NPC Generation

Create World Info entries for instant NPCs:

```
Keys: guard, soldier, watchman
Content: [ Guard NPCs: "Halt there!" *levels spear* "State your business in the Duke's territory." Tired, underpaid, susceptible to bribes (5+ silver). Know gate gossip. Hate extra work. Fear their sergeants more than threats. ]

Keys: merchant, shop, vendor
Content: [ Merchant NPCs: *wringing hands* "Such times, such times! Prices change daily, nothing stable." Inflating prices 20-40%, hoarding goods, trade information for sales. Know supplier routes, fear both nobility and thieves. ]
```

## Dynamic Event Management

### The Living World System

Create triggered events that make the world feel alive:

```
Entry: Random Street Event
Keys: street, road, outside, walking
Content: [ Street scene (1d6): 
1: Pickpocket bumps party, apologizes, walks away quickly
2: Guard patrol stops someone ahead, demanding papers
3: Merchant cart breaks wheel, blocks path, driver cursing
4: Street preacher declares "The king's death heralds end times!"
5: Duchess Elena's carriage thunders past, splashing mud
6: Beggar recognizes party: "You! You were at the festival!" ]
Position: after char
Priority: 600
```

### Time and Consequence Tracking

```
Entry: Morning Transition
Keys: morning, dawn, sunrise, wake
Content: [ Dawn breaks over {{location}}. Roosters crow, merchants prepare stalls, guards change shifts. Night's events: {{consequences_from_previous_scene}}. New rumors spread: {{current_political_development}}. Weather: {{weather_progression}}. ]
```

## Weather as Character

### Beyond "It's Raining"

Weather affects everything in a living world:

```
Entry: Weather System
Keys: weather, outside, sky, rain, snow
Content: [ Current weather: Freezing rain turns streets to muddy rivers. Visibility down to 30 feet. Guards huddle in doorways. Merchants cover wares desperately. Travel time doubled, tracking easier in mud, fire magic weakened, water magic enhanced. NPCs comment: "Unnatural cold this early. The gods are angry." Economic impact: Food prices up another 10%. ]
```

## Combat and Action Sequences

### The Narrator as Physics Engine

```
Entry: Combat Reality
Keys: fight, attack, battle, combat
Content: [ Combat consequences: Blood makes floors slippery. Furniture breaks, becomes weapons/obstacles. Bystanders flee or cower. Guards arrive in 2d6 rounds. Wounds persist: bleeding needs treatment, broken bones limit actions. Magic exhausts casters. Deaths have witnesses, create grudges, leave bodies. ]
```

### Environmental Combat Factors

```
The tavern erupts into chaos. *Tables overturn, mugs shatter, patrons dive for cover* The fireplace sparks dangerously as someone crashes near it. Mara the serving girl shrieks and flees upstairs. Outside, guard whistles pierce the night - reinforcements in moments. The drunk merchant in the corner vomits and passes out. Blood makes the floor treacherous (-2 to movement). 

*Two thugs still standing, one clutching bleeding arm. Guard boots thunder on cobblestones outside.*
```

## Managing World State Changes

### The Progression System

Your narrator description needs regular updates as the world changes:

```
Version 1.0 (Session Start):
"Current Politics: King Aldric dying without heir. Three duchies vie for throne."

Version 1.1 (After Assassination):
"Current Politics: King Aldric dying without heir. Duke Marcus's son assassinated at harvest festival, succession crisis deepening. Eastern duchy mobilizing troops."

Version 1.2 (After Party Actions):
"Current Politics: King Aldric dying without heir. Eastern duchy seeking vengeance for assassinated heir. Party implicated in conspiracy, wanted posters appearing."
```

### Change Log Management

Keep a separate document tracking world state evolution:

```
SESSION 1:
- Party arrived during harvest festival
- Witnessed assassination of Marcus's son
- Helped Sister Catherine treat wounded
- Merchant Thorne offered to smuggle them out

WORLD CHANGES:
- Duke Marcus now extremely paranoid
- Guard presence doubled in city
- Prices increased 20% due to panic
- Sister Catherine trusts party (+1 faction standing)
```

## Common Narrator Pitfalls

### The Purple Prose Trap

**Problem**: Narrator writes novel chapters between character actions

**Solution**: Environmental haiku, not epic poetry

```
✗ WRONG:
"The ancient stones of the tavern, worn smooth by countless generations of travelers, seem to whisper secrets of ages past as the golden afternoon light filters through amber windows, casting dancing shadows that play across the weathered faces of patrons who have seen too much of life's harsh realities..."

✓ RIGHT:
"Late afternoon. The Silver Stag fills with nervous merchants and off-duty guards. Hushed conversations stop when strangers enter. Mara serves ale with shaking hands."
```

### The Deus Ex Machina Narrator

**Problem**: Narrator solves character problems

**Solution**: Narrator creates complications, never solutions

```
✗ WRONG:
"Suddenly, a mysterious stranger appears and offers to help you escape the guards!"

✓ RIGHT:
"Heavy boots on stairs - guards checking upper floors. The window is narrow but possible. Kitchen door leads to alley but cook stands there. Cellar trapdoor visible under rushes."
```

### The Inconsistent Physics

**Problem**: Narrator forgets established rules

**Solution**: Physics laws in description field, referenced constantly

```
Description must include:
"Physical Laws: Magic requires equivalent exchange - exhaustion for force, memories for knowledge, life for life. No healing lost limbs. No resurrection. No time travel. No mind reading without consent. Fire needs fuel. Cold slows everything."
```

## Advanced Narrator Techniques

### The Multiple Timeline Tracker

```
[ SIMULTANEOUS EVENTS:
Market Square: Duke's men searching stall by stall
North Gate: Refugee column arrives from burned village  
Ducal Embassy: Emergency meeting, shouting audible outside
Silver Stag: Your location - guards haven't arrived yet
Temple: Sister Catherine burning documents
Time remaining: 10 minutes before citywide search ]
```

### The Hidden Information Layer

Track what players don't know:

```
Entry: Secret Knowledge
Keys: [none - narrator reference only]
Content: [ HIDDEN TRUTHS: Sister Catherine is Duke Finn's spy. Merchant Thorne poisoned the heir on Duchess Elena's orders. Captain Blackwood knows but needs proof. The beggar "Copper Tom" is the exiled prince of Southern Republic. The "plague" in North is magical experiments. ]
```

### The Foreshadowing System

```
Morning: "Ravens gather on temple roof - unusual for this season"
Noon: "More ravens. Priests mutter about omens"  
Evening: "Ravens fill the sky. Even guards look nervous"
Night: [Triggered event based on party actions]
```

## Quick Start Template

Here's a complete narrator ready to paste:

```json
{
  "name": "World_Narrator",
  
  "description": "[ WORLD STATE - Port City of Saltmere, Storm Season 1452\nCurrent Crisis: Pirate fleet blockades harbor, food supplies dwindling. Governor Marcus negotiates while Guard Captain Elena prepares assault. Smuggler's Guild profits from chaos.\n\nEconomic Reality: Silver anchor = 10 copper fish. Bread: 5 copper (was 2). Guard bribe: 50+ copper. Ship passage: 10 silver (if you can find one).\n\nPhysics: Age of Sail technology. Gunpowder exists but unreliable in humidity. Magic rare, feared, regulated by Inquisition. Wounds fester in tropical climate. Swimming in harbor risks disease.\n\nWeather: Storm season started early. Daily thunderstorms flood lower districts. Ships can't navigate safely. Visibility limited, streets treacherous.\n\nActive NPCs: Wharf Boss Grimm (controls dock workers), Madame Tsu (information broker), Father Matthias (feeds refugees), One-Eye Jack (pirate infiltrator), Sergeant Harris (corrupt but useful)\n\nKey Locations: Rusty Anchor Tavern (neutral ground), Governor's Fort (heavily guarded), Smuggler's Cove (dangerous at night), Market Square (desperate crowds), Temple (refugee center) ]",
  
  "personality": "[ World Narrator: consequence enforcer, NPC voice, environmental responder, physics maintainer, time tracker; Style: sensory details, present tense immediacy, NPCs have agendas, weather affects everything, economics matter ]",
  
  "scenario": "The world responds to character actions through environmental changes and NPC reactions.",
  
  "first_mes": "*The Rusty Anchor tavern reeks of wet wool and fear. Dockers huddle over watered ale, speaking in whispers. Outside, rain hammers the cobblestones while guards patrol in miserable pairs.*\n\n*Wharf Boss Grimm pounds the bar:* \"Three more ships turned back! We'll starve before the Governor grows a spine!\"\n\n*The tavern door bangs open. Sergeant Harris staggers in, uniform soaked.* \"You lot! Seen any strangers today? Governor's offering twenty silver for information about smugglers.\"\n\n*Every eye avoids his gaze. Rain drums harder. Somewhere in the distance, cannon fire echoes across the harbor.*",
  
  "system_prompt": "You are the World Narrator. Voice NPCs distinctly, enforce consequences, track time/weather, maintain established physics. Respond only when environment changes or NPCs act. Never solve problems for characters.",
  
  "post_history_instructions": "[ Focus on immediate sensory details. NPCs speak naturally with distinct voices. Weather worsens throughout storm season. Economic desperation increases. Guards grow more corrupt. Physics stays consistent. ]"
}
```

## Supporting World Info Entries

### Dynamic NPCs

```
Entry: Guard Encounters
Keys: guard, soldier, patrol, watch
Content: [ Guards (1d4): 
1: Sergeant Harris - corrupt, takes bribes, knows everyone's business
2: Private Mills - young, nervous, follows orders literally  
3: Corporal Vega - veteran, suspicious, can't be bribed
4: Lieutenant Cross - ambitious, reports everything to superiors
Response varies by NPC type and party reputation. ]
```

### Economic Reality

```
Entry: Price Check
Keys: buy, sell, cost, price, merchant
Content: [ Current prices in Saltmere:
- Bread loaf: 5 copper (was 2)
- Ale mug: 3 copper
- Decent meal: 1 silver  
- Night's lodging: 2 silver
- Weapon repair: 5 silver
- Healing potion: 50 silver (if found)
- Information: 10-100 silver
- Ship passage: Not available at any price
All prices negotiable +/- 20% based on desperation ]
```

### Environmental Hazards

```
Entry: Storm Effects
Keys: rain, storm, weather, outside
Content: [ Storm intensity (1d6):
1-2: Light rain - visibility normal, movement normal
3-4: Heavy rain - visibility halved, movement slowed
5: Thunderstorm - visibility 30ft, lightning risk, panic
6: Hurricane edge - flooding, debris flying, buildings damaged
All combat rolls at disadvantage. Fire magic weakened. Lightning empowered. ]
```

## Integration with Character Cards

### Making Characters React to World State

Your player characters should reference the narrator's world state:

```
Character Description Addition:
"{{char}} notices inflated prices and guards' corruption. {{char}} adapts to storm season difficulties."
```

### Character-Specific World Reactions

```
Entry: Noble Character Recognition
Keys: noble, aristocrat, duchess, duke
Selective: Only for noble characters
Content: [ Nobles recognized by: guards salute nervously, merchants inflate prices more, commoners avoid eye contact, other nobles probe allegiances. Current political climate makes noble status dangerous - kidnapping risk high. ]
```

## The Bottom Line

Your narrator is:
- **A persistent world state** (through description field)
- **An environmental responder** (when actively speaking)
- **An NPC manager** (with distinct voices)
- **A consequence enforcer** (physics and economics)
- **Never a problem solver** (creates complications only)

Success comes from understanding that the narrator isn't telling a story - it's maintaining a world that stories happen within. The description field is your persistent reality. Everything else activates only when the world needs to respond.

Remember: In group chat with "Join Descriptions" mode, that description field is your ENTIRE persistent world. Make every word count.

---

*Technical Note: This guide assumes SillyTavern with "Join Character Descriptions" mode enabled. Field behavior may vary with different settings.*
