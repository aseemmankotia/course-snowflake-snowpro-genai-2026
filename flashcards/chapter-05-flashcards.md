## Chapter 5: Cortex Analyst: Natural-Language Analytics on Structured Data — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What does Cortex Analyst fundamentally do? | Converts natural-language questions into SQL executed against structured Snowflake tables, using a semantic model to interpret intent. Exam tip: it's text-to-SQL, not a search/retrieval engine. |
| 2 | Cortex Analyst vs Cortex Search — one-line rule? | Analyst = structured data, text-to-SQL, live query execution. Search = unstructured/semi-structured text, semantic/hybrid retrieval, indexed with a refresh lag. |
| 3 | Which Cortex Analyst semantic model YAML section stores curated question+SQL pairs for accuracy tuning? | verified_queries — this is the Verified Query Repository (VQR); matched against incoming questions before new SQL is generated. |
| 4 | What YAML element maps alternate natural-language phrasing to a specific column? | synonyms — e.g., mapping 'sales' or 'income' to a `revenue` column so users don't need exact column names. |
| 5 | Semantic model: dimension vs measure? | Dimension = categorical attribute for grouping/filtering (region, category). Measure/fact = numeric value meant to be aggregated (revenue, count, sum). |
| 6 | What defines join paths between logical tables in a Cortex Analyst semantic model? | The relationships section — defines how tables connect (e.g., fact-to-dimension keys) so Cortex Analyst can generate correct joins. |
| 7 | When is unstructured RAG (Cortex Search) preferred over Cortex Analyst? | When the source data is free text (documents, reviews, tickets) and the goal is retrieving relevant passages, not computing aggregates over rows/columns. |
| 8 | How does data freshness differ between Cortex Analyst and Cortex Search? | Cortex Analyst executes generated SQL live against source tables (always current). Cortex Search relies on a search service index refreshed on a configurable lag (can be slightly stale). Exam tip: freshness is a key differentiator. |
| 9 | Known limitation of Cortex Analyst for complex analytics? | Accuracy drops with highly complex multi-table joins, deeply nested subqueries, or complex window-function logic. Fix: simplify toward a star-schema-like semantic model and add verified queries. |
| 10 | How do you integrate Cortex Analyst into a custom chatbot/app outside Snowsight? | Call the Cortex Analyst REST API 'message' endpoint, passing the semantic model reference plus the user's natural-language question; the API returns generated SQL/results. |
| 11 | Best practice for iterating/testing a semantic model? | Continuously test with real user questions, identify wrong/incomplete SQL, add corrected examples to verified_queries, refine synonyms and dimension/measure definitions, and simplify overly complex table structures. |
| 12 | Combined structured + unstructured Q&A architecture pattern? | Use Cortex Analyst for the structured/SQL path and Cortex Search for the unstructured/text path, with an orchestrating agent routing each user question to the appropriate service. |
