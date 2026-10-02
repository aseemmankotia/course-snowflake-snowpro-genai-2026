## Ch.11 Cheat Sheet — Governing, Securing, Costing Gen AI on Snowflake

### RBAC for Cortex
| Object | Required grant |
|---|---|
| LLM functions (COMPLETE, TRANSLATE, SUMMARIZE, SENTIMENT, EXTRACT_ANSWER) | `GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE <r>` |
| Cortex Search service | `USAGE` on the search service object + access to underlying data |
| Cortex Analyst semantic model | `USAGE` on the semantic model / stage + underlying table access |

**Exam rule:** CORTEX_USER only unlocks the *function*. Row/masking policies on underlying tables still apply — enforcement is at the data layer, not the AI layer.

### Data Governance with Gen AI Queries
| Control | Restricts | Applies through Cortex Analyst/Search? |
|---|---|---|
| Row Access Policy | Which rows returned | Yes |
| Masking Policy | Column values shown | Yes |

**Rule:** Mask PII (SSN, email, DOB) at the column BEFORE it's concatenated into a prompt string — masking after the LLM call is too late, the raw value already left the boundary.

### Cross-Region & Data Residency
- Parameter: `CORTEX_ENABLED_CROSS_REGION`
- Default: `DISABLED` (inference stays in-account region)
- Other values: `ANY_REGION`, or explicit region list
- **Exam trap:** if a model isn't available in-region and this stays DISABLED, the function call fails — but enabling it broadly risks sending PII prompt data cross-border.

### Cost Monitoring
| Need | Where to look |
|---|---|
| Token/credit usage per Cortex LLM call | `SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY` |
| Warehouse compute cost | `WAREHOUSE_METERING_HISTORY` (separate billing from LLM tokens) |

**Exam trap:** Warehouse size/auto-suspend tuning does NOT reduce Cortex LLM token cost — those are billed independently per token/model.

### Cost Control Levers
1. Use the **smallest capable model** for the task (classification/extraction ≠ needs flagship model).
2. Cache or batch repetitive prompts where possible.
3. Right-size warehouses/compute pools separately for orchestration workloads (not LLM billing).
4. Set query/prompt limits to avoid runaway loops calling LLM functions repeatedly.

### Responsible AI
| Concern | Mitigation |
|---|---|
| Harmful/unsafe generated content | Cortex Guard (output moderation) |
| Hallucination | Disclose AI-generated content; require human review for high-stakes decisions |
| Bias | Evaluate model outputs across demographic slices; document known limitations |
| PII exposure in prompts | Mask/tokenize before prompt assembly, restrict cross-region routing |

**Key exam distinction:** Cortex Guard = output safety filter. Masking Policy = input/stored data protection. Row Access Policy = row-level visibility. They solve different problems and are often combined, not substitutes for one another.
