## Chapter 11: Governing, Securing, and Costing Gen AI on Snowflake — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Which database role must be granted to allow a role to call Cortex LLM functions (COMPLETE, TRANSLATE, SUMMARIZE)? | SNOWFLAKE.CORTEX_USER. Exam tip: GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE <role> — without it, calls fail even with SELECT on the data. |
| 2 | Account parameter controlling whether Cortex inference can run in a different region than the account? | CORTEX_ENABLED_CROSS_REGION. Default value: DISABLED. Exam tip: set to ANY_REGION or a specific region list to permit cross-region model routing. |
| 3 | Does a row access policy on a base table still apply when the table is queried via Cortex Analyst or Cortex Search? | Yes — RAPs enforce at the underlying table regardless of the querying layer. Exam tip: governance lives at the data layer, not the AI interface. |
| 4 | Snowflake feature that filters/moderates potentially unsafe LLM-generated output text? | Cortex Guard. Exam tip: distinct from masking policies/RAPs — it screens generated content, not stored data. |
| 5 | Which ACCOUNT_USAGE view tracks token/credit consumption per Cortex LLM function call? | CORTEX_FUNCTIONS_USAGE_HISTORY. Exam tip: use this to find which functions/models drive the highest Gen AI spend. |
| 6 | Row access policy vs masking policy — what does each control? | Row access policy = filters which ROWS are visible. Masking policy = obscures/replaces COLUMN VALUES. Exam tip: both can apply simultaneously to a table used by Cortex Analyst. |
| 7 | Best practice for reducing Cortex LLM cost on simple tasks like classification or short extraction? | Use the smallest model capable of the task instead of the largest flagship model, since billing scales with tokens processed and model tier. |
| 8 | Is Cortex LLM function token billing part of virtual warehouse credit consumption? | No — LLM function calls are billed separately based on tokens processed, independent of warehouse size. Exam trap: resizing the warehouse does not reduce LLM cost. |
| 9 | Where must PII masking be enforced when building a prompt from table data for Cortex COMPLETE? | At the source column via a masking policy, so the masked value is what gets assembled into the prompt string — masking after the LLM call is too late. |
| 10 | Recommended responsible AI practice for high-stakes Gen AI outputs (e.g., financial/medical summaries)? | Require human-in-the-loop review before acting on the output, and disclose that content is AI-generated and may contain hallucinations. |
| 11 | What privilege is required (beyond CORTEX_USER) for a role to use a Cortex Search service object? | USAGE privilege on the CORTEX SEARCH SERVICE object itself, plus access to the underlying data it indexes. |
| 12 | Main governance risk of broadly enabling cross-region Cortex inference? | Prompt data (possibly containing PII) is processed outside the account's home region, which can violate data residency/compliance requirements — mitigate by restricting CORTEX_ENABLED_CROSS_REGION to approved regions only. |
