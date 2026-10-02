# Chapter 2 Cheat Sheet: Prompting, Model Selection, LLM Limitations

## Prompting Patterns
| Technique | Definition | When to Use |
|---|---|---|
| Zero-shot | No examples, instructions only | Simple, well-understood tasks |
| Few-shot | 1+ example input/output pairs in prompt | Need consistent format/structure |
| System prompt | Persistent role, tone, constraints | Set behavior once for whole session |
| User prompt | The actual question/task | Carries per-call request |

**Exam note:** Exam scenarios often ask which prompting technique fixes an inconsistent-format problem → answer is usually few-shot, not a bigger model.

## Model Selection Tradeoffs
| Factor | Small Model | Large Model |
|---|---|---|
| Latency | Lower | Higher |
| Cost (credits) | Lower | Higher |
| Reasoning/accuracy on complex tasks | Weaker | Stronger |
| Best fit | High-volume, simple tasks (FAQ, classification) | Complex reasoning, long-context analysis |

**Rule of thumb:** Match model size to task complexity. Exam traps reward 'right-sized' answers over 'always pick the biggest model'.

## Generation Parameters
| Parameter | Effect | Exam Tip |
|---|---|---|
| temperature | Higher = more random/creative; lower = more deterministic | Use low temperature for factual/extraction tasks |
| top_p | Nucleus sampling — restricts token pool by cumulative probability | Alternative randomness control; don't confuse with temperature |
| max_tokens | Caps output length | Too low truncates answers; too high raises cost/latency |

## LLM Limitations & Mitigations
| Limitation | Description | Mitigation |
|---|---|---|
| Hallucination | Confident but false/fabricated output | RAG (ground with retrieved docs), lower temperature, clear system prompt constraints |
| Knowledge cutoff | No awareness of events after training data cutoff | RAG / inject current data at query time; do not rely on fine-tuning alone for recency |
| Non-determinism | Same prompt may yield different outputs | Set temperature near 0 for repeatability |

**Common exam trap:** Assuming a larger model 'solves' hallucination or recency — it reduces but never eliminates hallucination, and does NOT solve knowledge cutoff. Only grounding techniques (RAG) address recency.

## Quick Decision Rules
- Simple task + high volume + cost-sensitive → smallest capable model
- Complex multi-step reasoning + accuracy priority → largest capable model
- Need consistent, repeatable output → temperature ≈ 0
- Need creative/varied output → higher temperature
- Need current/external facts → RAG, not prompting or model size alone
- Need consistent output format → few-shot examples in prompt
- Need persistent persona/rules across calls → system prompt, not user prompt
