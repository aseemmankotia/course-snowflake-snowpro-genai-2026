# Cortex Analyst — Cheat Sheet

## What It Does
Converts natural-language questions → SQL, executed **live** against structured Snowflake tables, guided by a **semantic model** (YAML). Not a search/retrieval engine.

## Cortex Analyst vs Cortex Search
| Feature | Cortex Analyst | Cortex Search |
|---|---|---|
| Data type | Structured tables | Unstructured / semi-structured text |
| Output | Generated SQL + results | Ranked relevant passages/chunks |
| Mechanism | Text-to-SQL via semantic model | Hybrid (vector + keyword) search over an indexed service |
| Freshness | Always current (live SQL execution) | Depends on service refresh lag (indexed, may be slightly stale) |
| Best for | Aggregations, metrics, dashboards-style questions | RAG, document Q&A, free-text search |
| Setup | Semantic model YAML (tables, dims, measures) | `CREATE CORTEX SEARCH SERVICE ... ON <col> ... TARGET_LAG = ...` |

## Semantic Model YAML — Key Sections
| Section | Purpose |
|---|---|
| `tables` | Logical tables mapped to base tables, with description |
| `dimensions` | Categorical attributes for grouping/filtering |
| `measures` / `facts` | Numeric values intended for aggregation |
| `time_dimensions` | Date/time columns for time filtering/grouping |
| `relationships` | Join paths between logical tables |
| `synonyms` | Alternate natural-language terms mapped to columns |
| `verified_queries` | Curated question + SQL pairs (Verified Query Repository) matched before new SQL is generated |

## Accuracy Tuning Workflow
1. Draft semantic model (tables, dims, measures, synonyms).
2. Test with real business questions.
3. Identify wrong/incomplete SQL outputs.
4. Add corrected examples to `verified_queries`.
5. Simplify complex joins → favor star-schema-like structure.
6. Iterate — repeat testing after each change.

## Known Limitations
- Accuracy degrades with many-table joins, deep nested subqueries, complex window functions.
- Best performance on star-schema-style semantic models (fact + dimension tables).
- Not designed to search free-text content — use Cortex Search for that.

## Integration
- **REST API**: Cortex Analyst message endpoint — pass semantic model reference + NL question → returns SQL/results. Used for custom apps/chatbots.
- **Snowsight / Streamlit-in-Snowflake**: built-in UI option for internal analytics use cases.
- **Combined agent pattern**: route structured questions → Cortex Analyst; unstructured questions → Cortex Search; orchestrated by an agent layer.

## Exam Traps
- Don't confuse Cortex Analyst (structured, SQL) with Cortex Search (unstructured, retrieval) — this is the #1 tested distinction.
- `verified_queries` improves accuracy for *known* questions; it does not fix fundamentally over-complex semantic models — simplify structure first.
- Cortex Analyst queries data live; it does not use a vector index like Cortex Search.
