## Chapter 6 Cheat Sheet: Cortex Fine-Tuning & Evaluation

### Decision Matrix
| Need | Solution | Why |
|---|---|---|
| Consistent tone/format/style | Fine-tune | Bakes behavior into model weights |
| Fresh/proprietary facts | RAG (Cortex Search) | Retrieves live data at query time |
| Quick iteration, low cost | Prompt engineering | No training, instant changes |
| Reduce long few-shot token cost | Fine-tune | Shrinks prompt after training |

Order of preference: **Prompt engineering → RAG → Fine-tuning**

### CORTEX.FINETUNE Operations
| Operation | Purpose |
|---|---|
| `'CREATE'` | Start a new fine-tuning job |
| `'DESCRIBE'` | Check status of a job by ID |
| `'SHOW'` | List fine-tuning jobs |
| `'CANCEL'` | Cancel a running job |

### CREATE Syntax
```sql
SELECT SNOWFLAKE.CORTEX.FINETUNE(
  'CREATE',
  'my_model_name',
  'llama3-8b',
  'SELECT prompt, completion FROM train_tbl',
  'SELECT prompt, completion FROM val_tbl'  -- optional
);
```
Training data must be a SQL query returning `prompt` and `completion` columns.

### Calling a Fine-Tuned Model
```sql
SELECT SNOWFLAKE.CORTEX.COMPLETE('my_model_name', 'your prompt here');
```
Same COMPLETE function as base models — call by name.

### Numbers & Limits
| Item | Value |
|---|---|
| Min training examples | 10 (documented floor; hundreds+ recommended) |
| Validation data | Optional |

### Supported Base Model Family (fine-tunable)
- Open-weight models hosted in Cortex (e.g., **Llama 3 family** such as llama3-8b)
- NOT fine-tunable: proprietary hosted models (GPT-4o, Claude, Gemini)

### Evaluation Metrics
| Metric | Best For |
|---|---|
| ROUGE | Summarization (recall-oriented overlap) |
| BLEU | Translation (precision-oriented overlap) |
| Perplexity | Statistical fit of model to text, not task accuracy |
| Human eval | Subjective quality, style, correctness |

### Cost & Time Tradeoffs
- Fine-tuning = training compute cost (one-time) + ongoing inference cost
- Prompt engineering = $0 setup, but long prompts raise per-call token cost
- RAG = setup cost (indexing/search), lower per-call token cost than giant prompts
- Fine-tuning takes the longest to set up (data prep + training time); prompt engineering is instant

### Cortex Playground
- No-code UI in Snowsight to test/compare prompts across multiple LLMs
- NOT used to launch fine-tuning jobs (use SQL FINETUNE function for that)

### Exam Traps
1. Confusing RAG (knowledge) with fine-tuning (behavior/style) — the exam loves this scenario swap.
2. Assuming any Cortex COMPLETE model can be fine-tuned — only specific open-weight models qualify.
3. Mixing up minimum (10) vs recommended (hundreds+) training example counts.
