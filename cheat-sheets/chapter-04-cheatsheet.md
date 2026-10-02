## Cortex Search Cheat Sheet

### What It Is
A fully managed **hybrid retrieval service** (vector + keyword/BM25) built on a Snowflake table/view. Automatically generates embeddings, maintains the index, and blends semantic + lexical relevance in one ranked result — no manual embedding pipeline required.

### Cortex Search vs. Manual Vector Search
| Feature | Cortex Search | Manual VECTOR column |
|---|---|---|
| Embedding generation | Automatic | Manual (you call embedding fn) |
| Refresh | Automatic via TARGET_LAG | Manual re-embed/update |
| Relevance | Hybrid (semantic + lexical) | Semantic only (cosine sim) |
| Filtering | Native via ATTRIBUTES | Manual WHERE clause |

### Creating a Service (syntax skeleton)
```sql
CREATE CORTEX SEARCH SERVICE my_service
  ON description               -- searchable/embedded column
  ATTRIBUTES product, severity -- filterable metadata (NOT embedded)
  WAREHOUSE = WH_XS
  TARGET_LAG = '1 hour'
  AS SELECT description, product, severity FROM tickets;
```
- `ON` column = the text that gets embedded + lexically indexed
- `ATTRIBUTES` columns = used for filtering only, not searched semantically
- `TARGET_LAG` = freshness knob (like Dynamic Tables); lower = fresher, more compute
- `WAREHOUSE` = compute backing both refresh and query execution

### Querying
```sql
SELECT SNOWFLAKE.CORTEX.SEARCH_PREVIEW(
  'my_service',
  '{"query":"password reset error",
    "columns":["description","product"],
    "filter":{"@eq":{"severity":"High"}},
    "limit":5}'
);
```

### RAG Wiring Pattern
1. Query Cortex Search → get top-k relevant passages
2. Build prompt: `context + user question`
3. Pass prompt to `SNOWFLAKE.CORTEX.COMPLETE(model, prompt)`
4. Return grounded answer

**Never** skip step 1→2 wiring — that's the #1 exam trap (calling COMPLETE with no retrieved context).

### Tuning Levers
| Symptom | Fix |
|---|---|
| Stale results | Lower TARGET_LAG |
| Slow query latency | Scale up/out WAREHOUSE |
| Irrelevant results mixing keyword/semantic | Adjust query phrasing; rely on built-in hybrid scoring (no manual weighting exposed) |
| Need scoped results | Add/adjust `filter` on ATTRIBUTES columns |

### Exam Callouts
- Filtering happens via **ATTRIBUTES + filter JSON**, not a trailing SQL WHERE clause.
- Only the `ON` column is embedded; `ATTRIBUTES` are metadata only.
- Hybrid search is **automatic** — no per-query mode switch needed.
- TARGET_LAG = freshness; WAREHOUSE size = performance. Don't confuse the two levers.
