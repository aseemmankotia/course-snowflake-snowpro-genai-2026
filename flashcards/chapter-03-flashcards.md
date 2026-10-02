## Chapter 3: Cortex LLM Functions: COMPLETE, SUMMARIZE, TRANSLATE, and More — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Argument order for SNOWFLAKE.CORTEX.COMPLETE? | COMPLETE(<model>, <prompt>) — model name is ALWAYS first. Exam tip: reversing this order is a common wrong-answer trap. |
| 2 | Which Cortex function takes only ONE required text argument and no model choice? | SUMMARIZE(text) — purpose-built, no model parameter needed. Exam tip: if a question says 'without choosing a model,' the answer is SUMMARIZE, not COMPLETE. |
| 3 | TRANSLATE function signature? | TRANSLATE(text, source_language, target_language). Exam tip: source_language can often be left as an empty string for auto-detect in some usages — but target_language is always required. |
| 4 | SENTIMENT output range? | FLOAT from -1 (negative) to 1 (positive), near 0 = neutral. Exam tip: it's a continuous score, NOT a text label like 'positive'/'negative'. |
| 5 | EXTRACT_ANSWER signature and use case? | EXTRACT_ANSWER(text, question) — returns extractive answer + confidence score from a passage. Use when you need a SPECIFIC fact answered, not a general summary. |
| 6 | CLASSIFY_TEXT signature? | CLASSIFY_TEXT(text, [category1, category2, ...]) — categories passed as an ARRAY, not a comma-string. Returns object with assigned label. |
| 7 | COMPLETE vs custom prompt via COMPLETE — when to use a dedicated function instead? | Use SUMMARIZE/TRANSLATE/SENTIMENT/EXTRACT_ANSWER/CLASSIFY_TEXT when the task matches their purpose exactly — they're cheaper, more consistent, and don't require prompt engineering. Use COMPLETE only for open-ended/creative/custom generation tasks. |
| 8 | Why is running COMPLETE with a large model over millions of rows expensive? | Cortex functions bill per TOKEN processed, and larger models have higher per-token credit rates. Exam tip: for simple tasks at scale, prefer a smaller model or a purpose-built function to cut cost. |
| 9 | What does increasing max_tokens do to COMPLETE cost? | Increases potential output length and thus increases token consumption and cost — it does NOT speed up or cheapen processing. |
| 10 | Which function would you chain FIRST before CLASSIFY_TEXT if incoming text is in a non-English language and categories are English labels? | TRANSLATE — convert text to English first, then CLASSIFY_TEXT. Exam tip: pipeline order matters in scenario questions. |
| 11 | SUMMARIZE vs COMPLETE with a summarization prompt — which is preferred and why? | SUMMARIZE is preferred: no model selection, no prompt engineering, more consistent output, typically lower overhead for a task it's purpose-built for. |
| 12 | Does EXTRACT_ANSWER guarantee the answer exists in the text? | No — it returns its best extractive guess plus a confidence score; a low score indicates the answer may not be reliably present. Exam tip: don't assume EXTRACT_ANSWER always returns a correct or even present answer. |
