# Chapter 1 Cheat Sheet — Gen AI Foundations in Snowflake

## Traditional ML vs Generative AI
| Aspect | Traditional ML | Generative AI |
|---|---|---|
| Model scope | Task-specific, one model per task | Single foundation model, many tasks |
| Training | Requires labeled data per task | Pre-trained; used via prompting (zero/few-shot) |
| Output | Prediction/classification | Generated text or vector embedding |

## Tokens & Context Window
| Concept | Rule of Thumb |
|---|---|
| Token size | ~4 characters ≈ 1 token (≈0.75 words/token) |
| Context window | Max tokens = input + output combined, per call |
| Overflow behavior | Truncation or rejected request — never silently ignored |

**Exam note:** Always estimate token count for large documents before sending to `COMPLETE`. If a prompt + expected response exceeds the model's context window, the call fails or truncates.

## Embeddings vs Completions
| | Embedding | Completion |
|---|---|---|
| Function | `EMBED_TEXT_768`, `EMBED_TEXT_1024` | `COMPLETE` |
| Output | Fixed-length numeric vector | Variable-length generated text |
| Use case | Semantic search, similarity ranking | Text generation, Q&A, chat |
| Storage | `VECTOR(FLOAT, n)` column, n = 768 or 1024 | Typically `VARCHAR`/`STRING` |

**Common mistake:** Choosing `COMPLETE` for a 'find similar documents' scenario instead of embeddings + vector similarity search.

## Snowflake Cortex Function Family
| Function | Type | Purpose |
|---|---|---|
| `COMPLETE` | Generative | Free-form text generation/chat |
| `EMBED_TEXT_768` / `EMBED_TEXT_1024` | Embedding | Vector representation for search |
| `SUMMARIZE` | Generative | Condense text |
| `TRANSLATE` | Generative | Language translation |
| `SENTIMENT` | Analytical | Sentiment scoring |
| `EXTRACT_ANSWER` | Extractive | Pull answer from context text |

## Architecture: Data Gravity
- Cortex functions run **inside** the Snowflake account boundary — no data export to third-party AI vendors required.
- 'Data gravity' = bring AI/compute to the data, not the data to external AI services.
- Key exam driver for choosing Cortex over external LLM APIs when governance/compliance is a stated requirement.

## Vector Column Definition Syntax
```sql
CREATE TABLE docs (
  id INT,
  content STRING,
  embedding VECTOR(FLOAT, 1024)
);

UPDATE docs
SET embedding = SNOWFLAKE.CORTEX.EMBED_TEXT_1024('e5-base-v2', content);
```
**Rule:** Declared VECTOR dimension MUST match the embedding function's output size (768 or 1024) or the insert fails.

## Quick Decision Rules
- 'Search/retrieve similar text' → Embeddings + vector similarity
- 'Generate new text/answer' → `COMPLETE`
- 'Keep sensitive data ungoverned/no third party' → Cortex (in-account execution)
- 'Prompt might be huge' → Check context window before calling `COMPLETE`
