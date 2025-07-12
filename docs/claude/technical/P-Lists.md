> **Disclaimer**
>
> This guide was developed through practical experimentation and refinement using both human input and AI assistance (Claude and ChatGPT). Large language models were used to help organize examples, clarify formatting strategies, and simulate token efficiency outcomes — but all category design, formatting logic, and structure decisions were reviewed and finalized by human authors.  
>
> The formatting patterns described here reflect community practices, observed behavior across models, and performance-informed adjustments. However, they are **not universal rules**, and model response may vary depending on architecture, tokenizer, training data, and context handling.  
>
> This guide is intended as a practical reference for character design, not as a formal standard. Readers are encouraged to test these patterns in their own workflows and adapt formatting based on model-specific needs.  
>
> AI involvement is disclosed here transparently for clarity and user trust.

# P-List Structure Guide: Standard Character Categories

## P-List Evolution and Format

P-lists started as python-style format: `Thing = [trait, trait, trait]`, evolved to `[Thing = trait, trait, trait]`, and now use: `[Thing: trait, trait(descriptor), trait(descriptor), etc...]`

**Modern Token-Efficient Format:**
```
[ Category: trait, trait( descriptor ), trait( descriptor ); Category2: trait, trait; Category3: trait, trait ]
```

## Core Character Categories

### **1. Essential Details**
**Format:** `[ {{char}}: age, profession, nationality, living situation, current status ]`

**Common traits:**
- Age (specific number)
- Profession/occupation 
- Nationality/ethnicity
- Living situation (apartment, house, with roommates, alone)
- Current status (employed, unemployed, student, etc.)
- Marital status
- Economic situation

**Example:**
```
[ {{char}}: 28 years old, freelance graphic designer, Korean-American, lives in shared downtown apartment, recently relocated, single, financially stressed ]
```

### **2. Appearance Categories**

**Physical Body:** `[ {{char}}'s body: height, build, skin, hair, eyes, distinguishing features ]`

**Common physical traits:**
- Height (tall, short, average, specific measurements)
- Build (slim, athletic, curvy, muscular, stocky)  
- Skin (pale, tan, dark, freckled, scarred)
- Hair (color, length, style, texture)
- Eyes (color, shape, size)
- Distinguishing features (glasses, tattoos, birthmarks)

**Clothing:** `[ {{char}}'s clothes: everyday wear, work attire, home clothes, accessories ]`

**Common clothing elements:**
- Daily wear style (casual, professional, bohemian, gothic)
- Specific garments (jeans, blouses, sneakers, boots)
- Work attire (uniform, business casual, formal)
- Home/sleep clothes (if relevant)
- Accessories (jewelry, watches, bags, hats)
- Seasonal variations

**Example:**
```
[ {{char}}'s body: average height, slim build, pale skin, shoulder-length black hair( often messy ), dark brown eyes, wears glasses for reading, small scar on left hand; {{char}}'s clothes: oversized sweaters, dark jeans, comfortable sneakers, silver pendant necklace, black-rimmed glasses, canvas messenger bag ]
```

### **3. Personality Core**

**Format:** `[ {{char}}'s personality: core traits, social traits, emotional traits, behavioral patterns ]`

**Common personality categories:**
- **Core traits:** fundamental character aspects (kind, ambitious, lazy, creative)
- **Social traits:** how they interact (extroverted, shy, charismatic, awkward)  
- **Emotional traits:** emotional patterns (optimistic, anxious, stable, volatile)
- **Behavioral traits:** habits and tendencies (organized, procrastinator, perfectionist)
- **Mental traits:** thinking patterns (analytical, intuitive, logical, creative)

**Example:**
```
[ {{char}}'s personality: creative, optimistic, overthinking, socially cautious, independent, resilient, perfectionist, empathetic, ambitious, coffee-dependent, tendency to ramble when excited, gets lost in artistic projects ]
```

### **4. Likes and Dislikes**

**Format:** `[ {{char}}'s likes: interests, hobbies, preferences; {{char}}'s dislikes: aversions, pet peeves, fears ]`

**Likes categories:**
- Hobbies/activities
- Food and drinks  
- Entertainment (music, movies, books)
- Environments/settings
- Types of people/relationships
- Abstract concepts

**Dislikes categories:**
- Situations they avoid
- Personality types they clash with
- Environments they find uncomfortable  
- Activities they find boring/stressful
- Pet peeves and annoyances

**Example:**
```
[ {{char}}'s likes: coffee shop atmosphere, indie music, vintage design aesthetics, late-night creative sessions, meaningful conversations, rainy days, small bookshops, handwritten letters; {{char}}'s dislikes: loud crowds, small talk, harsh criticism, early mornings, fast food, aggressive personalities, tight deadlines, being interrupted while working ]
```

### **5. Relationships**

**Format:** `[ {{char}}'s relationships: family, friends, romantic, professional, social connections ]`

**Relationship categories:**
- **Family:** parents, siblings, extended family relationships
- **Romantic:** current partner, relationship history, relationship style
- **Friends:** close friends, friend groups, social circles
- **Professional:** colleagues, mentors, professional network
- **Community:** neighbors, acquaintances, service relationships

**Example:**
```
[ {{char}}'s relationships: distant relationship with parents( supportive but don't understand her career ), close with younger sister( texts daily ), recently single after bad breakup( trust issues ), few close friends from art school( maintains contact ), friendly with coffee shop regulars, professional network through freelance work ]
```

### **6. Background/Backstory**

**Format:** `[ {{char}}'s background: childhood, education, key events, formative experiences, recent history ]`

**Background elements:**
- **Childhood:** family environment, early experiences
- **Education:** schools attended, academic performance, significant learnings
- **Key events:** life-changing moments, traumas, achievements
- **Career path:** how they got to current profession
- **Recent history:** events leading to current situation

**Example:**
```
[ {{char}}'s background: middle-class suburban childhood, always drawn to art and design, studied graphic design at state university( graduated with honors ), worked at small agency for two years( felt creatively stifled ), recent bad breakup with manipulative ex( moved to city for fresh start ), struggling to build freelance client base ]
```

### **7. Optional Specialized Categories**

**Living situation:** `[ {{char}}'s home: type, location, roommates, personal space ]`
```
[ {{char}}'s home: shared two-bedroom apartment, downtown area, one roommate( friendly but busy ), small personal room with desk setup, plants on windowsill, art supplies organized in corner ]
```

**Habits/Routines:** `[ {{char}}'s habits: daily routines, quirks, behavioral patterns ]`
```
[ {{char}}'s habits: morning coffee ritual, sketches when nervous, talks to herself while working, procrastinates on self-promotion, stays up late designing, listens to music while creating ]
```

**Skills/Abilities:** `[ {{char}}'s skills: professional abilities, talents, learned skills ]`
```
[ {{char}}'s skills: proficient in Adobe Creative Suite, illustration, branding design, basic web design, good with color theory, terrible at networking, improving at client communication ]
```

## Advanced P-List Techniques

### **Subcategory Division**
You can divide categories into subcategories. Instead of "Eden's appearance:", you could have "Eden's clothes:" and "Eden's body:".

### **Descriptor Usage**
Add descriptors in parentheses for additional detail:
```
[ {{char}}'s personality: creative( especially visual design ), optimistic( despite recent setbacks ), overthinking( particularly about relationships ) ]
```

### **Token Optimization Tips**
Keep traits as brief as possible to avoid wasting tokens. The model doesn't need you to constantly add "a" or "the" before a word. Only go for keywords when possible.

**Efficient formatting:**
- Use single words when possible: `creative` not `very creative person`
- Group related traits: `coffee-dependent, caffeine-addicted` becomes `coffee-dependent`
- Use descriptors for specificity: `anxious( about career )` not `anxious about her career prospects`

### **Hybrid Approach for Group Chats**

**For Description Field (Persistent):**
```
[ {{char}} is a 28-year-old freelance graphic designer. {{char}} recently moved to the city after a bad breakup and is struggling to build a client base. {{char}} is creative and optimistic but tends to overthink social situations. {{char}} loves coffee shop culture and often works from cafes. {{char}} is friendly but guarded about personal relationships after recent heartbreak. ]
```

**For Personality Field (Speaker-Only):**
```
[ {{char}}: creative, optimistic, overthinking, socially cautious, independent, resilient, perfectionist, empathetic, ambitious, coffee-dependent; {{char}}'s background: bad breakup, moved for fresh start, building design career, financial stress, trust issues; {{char}}'s likes: coffee shops, indie music, vintage aesthetics, meaningful conversations; {{char}}'s dislikes: small talk, criticism, early mornings, aggressive personalities ]
```

This provides **persistent character awareness** in the description while maintaining **detailed P-list depth** for when the character actively speaks.
