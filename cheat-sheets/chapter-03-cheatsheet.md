## Chapter 3 Cheat Sheet: Cortex LLM Functions

### Function Signatures
| Function | Signature | Purpose |
|---|---|---|
| COMPLETE | `COMPLETE(model, prompt)` | Open-ended generation, requires model choice |
| SUMMARIZE | `SUMMARIZE(text)` | Condense text, no model param |
| TRANSLATE | `TRANSLATE(text, source_lang, target_lang)` | Language conversion |
| SENTIMENT | `SENTIMENT(text)` | Polarity score -1 to 1 |
| EXTRACT_ANSWER | `EXTRACT_ANSWER(text, question)` | Extractive Q&A, returns answer + score |
| CLASSIFY_TEXT | `CLASSIFY_TEXT(text, [cat1, cat2, ...])` | Categorization into custom labels |

### Function vs Custom Prompt — Decision Rule
- Task matches a dedicated function exactly (summarize/translate/sentiment/extract/classify) → **use the dedicated function**. Cheaper, deterministic, no prompt maintenance.
- Task is open-ended, creative, or doesn't map to a dedicated function → **use COMPLETE** with a custom prompt and explicit model choice.

### Argument Order Traps
- COMPLETE: **model first, prompt second** — most commonly reversed in wrong answers.
- CLASSIFY_TEXT: **text first, category array second** — categories must be an array literal, not a string.
- EXTRACT_ANSWER: **text first, question second**.
- TRANSLATE: **text, source_lang, target_lang** — in that order.

### SENTIMENT Output
- Returns FLOAT range **-1 (negative) to 1 (positive)**.
- NOT a text label (no 'positive'/'neutral'/'negative' string returned).

### Cost Behavior (Conceptual — verify current rates in official docs)
- Cortex functions bill based on **tokens processed**.
- Larger/more capable models (e.g., larger COMPLETE models) cost **more credits per token** than smaller models or purpose-built functions.
- Increasing `max_tokens` in COMPLETE **increases** potential cost — it does not speed up or reduce cost.
- At scale, prefer: (1) smaller model when acceptable, (2) purpose-built function over COMPLETE + custom prompt when task matches.

### Common Pipeline Pattern
1. TRANSLATE (normalize language) →
2. CLASSIFY_TEXT or SENTIMENT (analyze) →
3. SUMMARIZE or EXTRACT_ANSWER (condense/extract) →
4. COMPLETE (generate final response/action, if needed)

### Exam Traps
- Choosing COMPLETE with a hand-written prompt when a dedicated function (SUMMARIZE/TRANSLATE/etc.) is the better, cheaper, more consistent answer.
- Reversing argument order in any function call.
- Assuming SENTIMENT returns a text label instead of a numeric score.
- Assuming EXTRACT_ANSWER always finds a correct answer — it returns a best-guess with a confidence score, not a guarantee.
