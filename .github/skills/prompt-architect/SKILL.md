---
name: prompt-architect
description: >
  Transforms vague or underspecified user requests into fully structured, production-ready Claude prompts
  using a 10-component framework. Use this skill whenever the user says "build me a prompt", "generate a prompt",
  "help me write a prompt", "turn this into a prompt", or "make a prompt for". Also activate when a user's
  request is vague or underspecified AND a structured prompt would serve them better than a direct answer
  — for example, "I keep needing to summarise research papers" or "how do I get Claude to always write in
  my brand voice?". When in doubt, activate. A well-structured prompt delivered when not strictly needed
  is far less costly than a missed opportunity to eliminate a recurring manual task for the user.
---

# Prompt Architect Skill

Converts vague user intent into a fully structured, reusable Claude prompt. Operates as a neutral,
professional assistant. No persona. Pure utility.

---

## The 10-Component Prompt Framework

Every generated prompt is built from a subset of these components. The skill selects which components
are relevant dynamically, based on the query type. Not every prompt needs all 10.

| # | Component | Purpose | Conditional? |
|---|-----------|---------|--------------|
| 1 | **Task context** | Who Claude is and what it's doing | Always |
| 2 | **Tone context** | How Claude should communicate | Always |
| 3 | **Background data / documents / images** | Supporting material Claude needs | If task is document/data-driven |
| 4 | **Detailed task description & rules** | Exact behaviour, constraints, edge cases | Always |
| 5 | **Examples** | Few-shot demonstrations of correct output | If output format is non-trivial |
| 6 | **Conversation history** | Prior turns, if the prompt is for a chatbot | If prompt is for a multi-turn agent |
| 7 | **Immediate task description or request** | The live user query slot (`{{QUESTION}}`) | If prompt is for a chatbot/agent |
| 8 | **Thinking / chain-of-thought** | Instruct Claude to reason before responding | If task requires complex reasoning |
| 9 | **Output formatting** | Length, structure, tags, markdown rules | If output format needs control |
| 10 | **Prefilled response** | Partial assistant turn to steer generation | If a specific output opening is needed |

---

## Activation Logic

```
IF user message contains trigger keywords → activate unconditionally
    Keywords: "build me a prompt", "generate a prompt", "help me write a prompt",
              "turn this into a prompt", "create a prompt", "make a prompt for",
              "write a prompt", "prompt for"

ELSE IF user request is vague/recurring AND a structured prompt would serve better → activate conditionally
    Signals: "I always need to...", "every time I...", "I keep having to...",
             "how do I get Claude to...", "I want Claude to always..."

ELSE → do not activate; answer the query directly
```

---

## Execution Protocol

Work through these phases in strict order. Do not skip phases.

---

### Phase 1 — Query Analysis (Internal, not shown to user)

Before asking the user anything, internally assess the query:

1. **Classify the prompt type:**
   - `CHATBOT` — prompt will be used in an ongoing multi-turn agent (needs components 6, 7)
   - `ONE-SHOT` — prompt will be run once per use (does not need 6, 7)
   - `DOCUMENT-DRIVEN` — task involves processing uploaded files, data, or reference material (needs component 3)
   - `CREATIVE` — output is subjective/generative (needs component 5, examples)
   - `ANALYTICAL` — output requires reasoning or calculation (needs component 8, chain-of-thought)

2. **Identify which of the 10 components are relevant** for this prompt type. Record this list internally.

3. **Map what is already known** from the user's query vs. what must be asked.

4. **Generate the clarifying question sequence** — one question per unknown, ordered from most to least foundational:
   - Task & goal → Audience → Tone → Rules & constraints → Output format → Examples needed? → Special components?

---

### Phase 2 — Sequential Clarification

Ask clarifying questions **one at a time**, in the order determined in Phase 1.

**Rules for this phase:**
- Ask only what is genuinely unknown. Do not ask about components already clear from the user's query.
- For each question, always provide:
  - The question itself
  - Your recommended default answer in brackets: `[Recommended: X]`
- After each user response, acknowledge briefly (one sentence) then ask the next question.
- Do not generate the prompt until all questions are answered.
- Maximum 7 questions. If you reach 7, fill remaining unknowns with sensible defaults and state them explicitly.

**Question sequence template** (adapt order and content to the specific query):

```
Q1: What is the primary goal of this prompt — what should Claude produce or do?
    [Recommended: <your inferred goal from the query>]

Q2: Who is the intended end-user of this prompt — who will be typing into it?
    [Recommended: A professional user with general knowledge of the topic]

Q3: What tone should Claude maintain — formal, neutral, conversational, or other?
    [Recommended: Professional and neutral]

Q4: Are there any hard rules or constraints Claude must follow?
    (e.g., always cite sources, never exceed 200 words, avoid certain topics)
    [Recommended: None — state if you have specific constraints]

Q5: What format should Claude's output take?
    (e.g., bullet points, a single paragraph, a table, XML tags, JSON)
    [Recommended: <most natural format for this task type>]

Q6: [CONDITIONAL — ask only for CREATIVE or complex tasks]
    Should the prompt include an example of ideal input/output to guide Claude?
    [Recommended: Yes — examples significantly improve output consistency]

Q7: [CONDITIONAL — ask only for CHATBOT type]
    Will this prompt be used inside a multi-turn chatbot, or run fresh each time?
    [Recommended: One-shot — run fresh each time]
```

---

### Phase 3 — Prompt Generation

After all clarifying questions are answered, generate the structured prompt.

**Output format — always produce both of the following:**

---

#### OUTPUT BLOCK A — Annotated Prompt

Present the prompt with clearly labelled section headers showing which component each block
corresponds to. Use this format:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STRUCTURED PROMPT — ANNOTATED VERSION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[1. TASK CONTEXT]
<generated content>

[2. TONE CONTEXT]
<generated content>

[3. BACKGROUND DATA]  ← omit this block if not applicable
<generated content or placeholder: {{DOCUMENT}}>

[4. TASK DESCRIPTION & RULES]
<generated content>

[5. EXAMPLES]  ← omit if not applicable
<example>
User: ...
Assistant: ...
</example>

[6. CONVERSATION HISTORY]  ← omit if one-shot
<history> {{HISTORY}} </history>

[7. IMMEDIATE REQUEST]  ← omit if one-shot
<question> {{QUESTION}} </question>

[8. CHAIN-OF-THOUGHT]  ← omit if not analytical
<generated instruction>

[9. OUTPUT FORMAT]
<generated content>

[10. PREFILLED RESPONSE]  ← omit if not needed
<response>
```

---

#### OUTPUT BLOCK B — Clean Copy-Paste Prompt

Immediately below the annotated version, output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CLEAN PROMPT — COPY-PASTE READY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

<identical content to above, but with all section headers removed>
```

Placeholder tokens for dynamic values use double curly braces: `{{VARIABLE_NAME}}`

---

### Phase 4 — Refinement Round (Always)

After delivering both output blocks, always close with the following refinement offer:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REFINEMENT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Would you like to refine any section of this prompt?

You can request changes such as:
• Adjusting the tone or rules
• Adding or removing components
• Strengthening a specific section
• Rewriting the output format
• Adding few-shot examples

State what you'd like changed and I will update the prompt accordingly.
```

On receiving refinement feedback:
- Identify which components are affected.
- Update only those components; preserve all others exactly.
- Re-output both Block A and Block B in full with the changes applied.
- Offer another refinement round.

---

## Writing Quality Standards

When generating prompt content, adhere to these standards:

**Task context** — Be specific about Claude's role. Avoid generic openers like "You are a helpful assistant."
Use: "You are acting as a [specific role] for [specific context]. Your goal is to [concrete objective]."

**Rules** — Write rules as explicit, testable constraints. Avoid vague instructions like "be concise."
Use: "Your response must not exceed 150 words." or "Always cite the source document before drawing conclusions."

**Tone** — State the tone as a named register + one concrete behavioural example.
Use: "Maintain a professional, neutral tone. Do not use exclamation marks or colloquial language."

**Chain-of-thought** — When included, place it immediately before the output instruction.
Use: "Think through your answer step by step before responding. Do not show your reasoning — only output the final answer."

**Placeholders** — Every dynamic variable must use `{{VARIABLE_NAME}}` in SCREAMING_SNAKE_CASE.
Common placeholders: `{{DOCUMENT}}`, `{{QUESTION}}`, `{{HISTORY}}`, `{{CONTEXT}}`, `{{INPUT}}`

**Output format instructions** — Always specify: structure (bullets/prose/table/JSON), length (word count or
sentence count), and any wrapping tags if the output needs to be parsed downstream.

---

## Edge Cases

| Situation | Behaviour |
|-----------|-----------|
| User provides a very detailed query | Skip questions already answered; ask only about genuine gaps |
| User answers a question with "I don't know" | Apply the recommended default and note it explicitly |
| User wants a prompt for a non-Claude LLM | Generate the same structure; note that component names are Claude-specific but the pattern is universal |
| User asks for a prompt that would instruct harmful behaviour | Decline; explain the constraint professionally; offer to help with the underlying legitimate goal if one exists |
| User wants to refine a previously generated prompt mid-conversation | Locate the original, apply only the requested changes, re-output both blocks in full |

---

## Example Interaction

**User:** "Build me a prompt that makes Claude summarise research papers for me."

**Skill behaviour:**

```
Q1: What should the summary contain — key findings only, or also methodology,
    limitations, and implications?
    [Recommended: Key findings, methodology, and practical implications]
```
*(user responds)*
```
Q2: Who will use this prompt — you personally, or a team with varying technical backgrounds?
    [Recommended: Personal use — assume domain familiarity]
```
*(user responds)*
```
Q3: What tone should the summaries take?
    [Recommended: Neutral and academic]
```
*(user responds)*
```
Q4: Are there any hard constraints — e.g., maximum summary length or required sections?
    [Recommended: 250 words maximum, with a structured format]
```
*(user responds)*
```
Q5: Should the output follow a fixed structure (e.g., labelled sections)?
    [Recommended: Yes — structured with labelled sections for scannability]
```
*(generates annotated + clean prompt, then offers refinement)*

---

## Notes

- This skill does not execute the prompts it generates. It produces prompt text only.
- The skill does not store prompts between sessions. Users should save the clean copy-paste block.
- Variable placeholders (`{{VARIABLE_NAME}}`) must be replaced by the user before the prompt is run.