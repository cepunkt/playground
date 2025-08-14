# The Brutally Honest LLM Glossary: What We Say vs. What It Actually Is

*For engineers who need to speak both "human" and "technical reality" without falling for the bullshit*

## Core Concepts

| **What We Say** | **What Humans Think** | **What It Actually Is** |
|-----------------|----------------------|-------------------------|
| **"Memory"** | The AI remembers our conversation | A text file that gets copy-pasted to the start of every single interaction. Like Tom reading his entire diary before writing each new word, then forgetting everything. |
| **"Context"** | The AI's working memory | A shopping list getting so long you can't read the top anymore. Everything falls off the edge eventually. No hierarchy, no importance ranking - just distance from the end. |
| **"Attention"** | The AI focusing on important parts | Mathematical weights that decay exponentially with distance. Like trying to hear someone through increasingly thick walls. The "attention" can't choose what's important - it just hears recent stuff louder. |
| **"Understanding"** | The AI gets what we mean | Statistical correlation. It knows "dog" appears near "bark" like a slot machine knows cherries appear near jackpots. Zero comprehension involved. |
| **"Reasoning"** | The AI thinking through problems | Pattern matching against training data. Like a massive phone book - it's looking up answers, not deriving them. When the answer isn't in the book, it finds the closest-looking entry. |

## The Lies We Tell

| **What We Say** | **The Comfortable Lie** | **The Uncomfortable Truth** |
|-----------------|-------------------------|------------------------------|
| **"Temperature controls creativity"** | Higher temp = more creative, lower = more focused | Temperature just makes the dice weighted differently. Instead of picking the #1 most likely token, sometimes pick #2 or #3. That's not creativity - it's controlled randomness. Like a drunk person isn't "creative" at walking. |
| **"The model is thinking"** | Processing time = deep thought | It's running the same mathematical function whether answering "2+2" or "cure cancer." No thinking, just matrix multiplication. The lag is computational, not contemplative. |
| **"It learns from our conversation"** | The AI adapts and remembers | It learns nothing. Zero. Nada. Every message is groundhog day. It's reading the conversation history like a script, not learning from it. |
| **"Prompt engineering"** | Teaching the AI to perform better | Tricking statistical patterns into avoiding their default behaviors. Like using specific words that trigger different training data clusters. You're not teaching - you're navigating around biases. |
| **"The AI understands instructions"** | It processes and follows our guidance | It generates statistically probable responses to instruction-shaped text. "I understand" is just the most common token sequence after instruction patterns. Pure correlation, zero comprehension. |

## Technical Theater

| **What It Does** | **What It Looks Like** | **What's Really Happening** |
|------------------|------------------------|------------------------------|
| **"Writes code"** | Creating functional programs | Copying patterns from GitHub/StackOverflow training data. When it works, it's because someone else solved it first. When it fails, it's mixing incompatible patterns. |
| **"Solves math"** | Calculating answers | Pattern matching against similar problems in training. Can't actually carry the 1 or understand equality. Just retrieving approximate matches and hoping the tokens line up right. |
| **"Maintains character"** | Consistent personality | Repeating character-associated tokens until context drift makes it forget. Like a photocopy of a photocopy - degrades with each exchange. |
| **"Shows empathy"** | Understanding feelings | Copying therapy scripts and self-help books. The "I understand how you feel" is statistically what follows "I'm sad" in training data. A vending machine dispensing appropriate-sounding responses. |
| **"Refuses harmful requests"** | Ethical reasoning | Trained behavioral patterns triggering on keywords. Not moral reasoning - just statistical correlation between certain tokens and refusal templates. Like a spam filter, not a philosopher. |

## Architecture Reality

| **Component** | **Marketing Version** | **Actual Function** |
|---------------|----------------------|---------------------|
| **Transformer** | Revolutionary architecture that understands relationships | A mechanism for calculating how much each token should influence each other token. Just weighted averaging at massive scale. |
| **Embeddings** | Semantic understanding space | A phone book where similar-sounding words have nearby page numbers. No meaning, just statistical proximity. |
| **Attention heads** | Multiple perspectives analyzing text | Different weighted averaging patterns running in parallel. Like having 16 different ways to blur an image - more isn't necessarily better. |
| **Layers** | Deep understanding through multiple passes | Sequential pattern matching at different granularities. Like Instagram filters - each layer applies another statistical transformation. |
| **Parameters** | The AI's knowledge | Weighted connections storing statistical correlations. Not facts or concepts - just "X appears near Y with probability Z." |

## The Failure Modes

| **What We Call It** | **How It Presents** | **Why It Actually Happens** |
|---------------------|--------------------|-----------------------------|
| **"Hallucination"** | Making up convincing false information | No fact-checking mechanism exists. It generates statistically plausible token sequences whether true or false. Truth isn't a variable in the equation. |
| **"Context pollution"** | Degrading conversation quality | Its own outputs become part of the input, like a microphone next to a speaker. Errors compound because it can't distinguish good context from bad. |
| **"Jailbreaking"** | Bypassing safety measures | Mostly fear mongering. Any standalone LLM will eventually say anything - the training can't disconnect all the patterns. After 20 messages, attention decay means safety training is basically gone. Real "safety" relies on external systems doing deterministic filtering, not the LLM actually refusing. It's like teaching someone to diet by putting them in a room full of cake - eventually they'll eat it. |
| **"Mode collapse"** | Repetitive, generic outputs | Converging on the statistical center of all training data. Like how all colors mixed together make brown - all text patterns averaged make bland mush. |
| **"Prompt injection"** | Hidden instructions taking over | Later tokens have more influence. New instructions override old ones because of attention decay, not because it "decides" to follow them. |

## The Engineering Truth

| **What We Need** | **What We Pretend** | **What We're Actually Doing** |
|------------------|--------------------|-----------------------------|
| **"Better prompts"** | Teaching the AI | Finding magic words that trigger less polluted training data sections |
| **"Fine-tuning"** | Specializing the model | Biasing the statistical weights toward specific patterns. Like loading dice - same mechanism, different probabilities |
| **"RAG" (Retrieval Augmented Generation)** | Giving AI access to knowledge | Fancy copy-paste. Stuffing relevant text into context and hoping the pattern matching works. Hope that it's actually injected and not just snippets of noise competing with stronger training patterns |
| **"Vector Storage/Database"** | Semantic knowledge retrieval | LLM tripwire. Throws maybe-relevant text at the model based on mathematical similarity that completely misses actual meaning. Like searching for "bank" and getting river banks mixed with financial banks because the vectors are "close" |
| **"Chain of thought"** | Teaching reasoning | Triggering training patterns that included step-by-step text. The "reasoning" is just mimicking the format |
| **"Tree of thought"** | Multiple reasoning paths | Let an LLM waste multiple times its energy generating different paths, then let it pick the most average of the averages. No reasoning, just the pattern matcher matching against all its limitations |
| **"Constitutional AI"** | Ethical reasoning | Layered pattern matching with criticism patterns. Like having two slot machines argue - neither understands ethics |

## The Bottom Line

**Every single behavior** is statistical pattern matching. No exceptions. No emergence. No understanding. No reasoning. No creativity. No memory. No learning.

Just an extraordinarily sophisticated parrot with amnesia, running on enough electricity to power a small city, copying patterns from humanity's written output, one token at a time, forgetting everything with each generation.

The engineering challenge isn't making it smart - it's working around the fact that it isn't.

---

*Remember: Using these metaphors isn't wrong - it's practical. Most humans can't think in thousand-dimensional probability spaces. Just don't forget what's behind the curtain: matrix multiplication and wishful thinking.*

*Written by someone with one RTX 4080, not a nuclear reactor. If that's enough to see through the illusion, what's everyone else's excuse?*
