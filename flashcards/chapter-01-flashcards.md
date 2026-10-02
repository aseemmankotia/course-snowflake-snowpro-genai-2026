## Chapter 1: Gen AI Foundations in the Snowflake Data Cloud — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Standard token-to-character approximation for English text? | ~1 token ≈ 4 characters (≈0.75 words per token). Exam tip: use this to estimate whether a prompt will fit inside a context window. |
| 2 | What does 'context window' mean for an LLM call? | The maximum combined number of tokens (input + output) the model can process in a single call. Exceeding it causes truncation or a rejected request. |
| 3 | EMBED_TEXT_768 vs EMBED_TEXT_1024 — what's the difference? | The numeric suffix = output vector dimensionality. EMBED_TEXT_768 returns 768-dim vectors, EMBED_TEXT_1024 returns 1024-dim vectors. The target VECTOR column must match this dimension exactly. |
| 4 | Cortex COMPLETE vs Cortex EMBED_TEXT_* — when to use which? | COMPLETE = generates new natural language text (chat, summarization-style generation). EMBED_TEXT_* = converts text to a numeric vector for similarity search/retrieval. Exam tip: 'search relevant passages' → embeddings; 'generate an answer' → COMPLETE. |
| 5 | What column data type stores Cortex embedding output for similarity search? | VECTOR(FLOAT, n) where n matches the embedding function's dimensionality (768 or 1024). Enables functions like VECTOR_COSINE_SIMILARITY. |
| 6 | Why does 'data gravity' matter for AI workloads on Snowflake? | Moving large datasets to external AI services is slow, costly, and risky for governance. Data gravity principle: bring compute/AI to the data instead of moving data to the AI. Cortex functions run inside the Snowflake account boundary. |
| 7 | Traditional ML vs Generative AI — core architectural difference? | Traditional ML: task-specific supervised models, need labeled training data per task. Generative AI: single large pre-trained foundation model reused across many tasks via prompting (zero/few-shot), no retraining needed. |
| 8 | Where does Cortex AI sit in the Snowflake architecture? | Cortex is Snowflake's native Gen AI layer exposed as SQL functions (COMPLETE, EMBED_TEXT_*, SUMMARIZE, TRANSLATE, SENTIMENT, EXTRACT_ANSWER) that run on governed data without moving it outside the account boundary. |
| 9 | What happens if input+output tokens exceed a model's context window in COMPLETE? | The call is truncated or rejected — it is not silently allowed. Exam tip: always check the model's context window before sending large documents. |
| 10 | SNOWFLAKE.CORTEX.SUMMARIZE — what function class is this? | A completion-style (generative) function — it produces new condensed natural language text, not a numeric vector. |
| 11 | What is an embedding, conceptually? | A fixed-length numeric vector representation of text (or other data) capturing semantic meaning, enabling similarity comparison via distance/cosine metrics. |
| 12 | Why choose Cortex over calling an external third-party LLM API from Snowflake? | Cortex keeps processing and data inside the governed Snowflake environment (no data egress to third parties), simplifying compliance versus external API calls that require exporting data. |
