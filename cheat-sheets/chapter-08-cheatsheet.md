## Chapter 8 Cheat Sheet: Chunking, RAG Patterns, Unstructured Data Prep

### RAG Pipeline Stages
| Stage | Purpose | Failure if skipped/misconfigured |
|---|---|---|
| Ingest | Load raw docs (PDF, DB, web) | Missing/incomplete corpus |
| Chunk | Split into retrievable units | Broken context, oversized inputs |
| Embed | Convert chunks to vectors | Poor semantic search |
| Retrieve | Find top-k relevant chunks (+ metadata filter) | Irrelevant context to LLM |
| Generate | LLM produces answer from retrieved context | Hallucination if retrieval is bad |

**Exam note:** Evaluate retrieval (precision/recall/hit-rate) BEFORE adding generation — generation can mask retrieval failures.

### Chunking Strategy Comparison
| Strategy | Best for | Tradeoff |
|---|---|---|
| Fixed-size | Uniform plain text | Fast/simple, ignores structure |
| Recursive / structure-aware | Docs with headings, sections, procedures | Preserves logical boundaries |
| Semantic | Complex, topic-shifting content | Higher compute cost, better relevance |

### Chunk Size & Overlap Guidance
- Starting chunk size: ~200–500 tokens (adjust to embedding model context limit).
- Overlap: ~10–20% of chunk size to preserve cross-boundary context.
- Always leave margin below the embedding model's hard token limit (tokenizer overhead, prepended metadata).

**Common trap:** 0% overlap → facts split across chunk boundaries become unretrievable.
**Common trap:** Chunk size too small → loses context; too large → dilutes relevance and may exceed token limits.

### Metadata Tagging
- Tag chunks at ingestion with: source, date, department, region, doc type.
- Enables **filtered retrieval**: restrict vector search to matching metadata before/during similarity search.
- Does NOT happen at generation time — filtering is a retrieval-stage operation.

### Structured vs Unstructured Data Prep
| Data type | Prep steps | Typical use |
|---|---|---|
| Structured (SQL/CSV) | Clean nulls/duplicates, normalize schema, feature framing | Direct query, tool/function calling |
| Unstructured (PDF/text) | Extract → clean → chunk → embed → tag | RAG retrieval |

**Common trap:** Chunking and embedding structured rows the same way as free text — usually wrong approach.

### PDF / Semi-Structured Handling (light overview)
- Use a layout-aware parser to separate tables, columns, and reading order before chunking.
- Naive text extraction on multi-column PDFs interleaves content out of order.
- Images/tables need dedicated extraction logic, not raw text scraping.

### Evaluation Before Generation
- Build a labeled query set with known-correct chunks.
- Measure retrieval precision/recall/hit-rate at top-k.
- Do not judge pipeline health solely on fluent LLM output — it can mask irrelevant retrieved context.
