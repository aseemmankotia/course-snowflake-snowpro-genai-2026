## Chapter 9 Cheat Sheet: Snowpark Container Services (SPCS)

### SPCS vs Cortex — When to Use Which
| Situation | Use |
|---|---|
| Model supported natively (llama3, mistral, arctic, etc.) | Cortex function (COMPLETE, EMBED_TEXT, etc.) |
| Custom / open-source model not in Cortex | SPCS |
| Need custom runtime, libraries, or GPU framework control | SPCS |
| Simple prompt/completion, no infra management desired | Cortex |
| Full control over serving code/API contract | SPCS |

### Compute Pool Instance Families
| Family | Hardware | Typical Use |
|---|---|---|
| CPU_X64_XS/S/M/L | CPU only | Lightweight containers, preprocessing |
| HIGHMEM_X64_S/M/L | High-memory CPU | Memory-intensive non-GPU workloads |
| GPU_NV_S | 1x NVIDIA A10G (24GB) | Small models, embeddings |
| GPU_NV_M | Multiple A10G GPUs | Mid-size model inference |
| GPU_NV_L | Multiple A100 GPUs (80GB each) | Large open-source LLM inference/fine-tuning |

### Job Service vs Long-Running Service
| Aspect | Job Service | Long-Running Service |
|---|---|---|
| Command | EXECUTE JOB SERVICE | CREATE SERVICE |
| Lifecycle | Runs to completion, then stops | Stays up until explicitly dropped/suspended |
| Use case | Batch scoring, training, ETL | Persistent inference endpoint / API |
| Cost pattern | Pay only during execution | Pay while running (even idle, unless suspended) |

### Key Commands
```sql
CREATE COMPUTE POOL my_pool
  MIN_NODES = 1 MAX_NODES = 2
  INSTANCE_FAMILY = GPU_NV_S
  AUTO_SUSPEND_SECS = 300;

CREATE IMAGE REPOSITORY my_repo;
-- docker login <repo_url>; docker push <repo_url>/model:v1

CREATE SERVICE my_service
  IN COMPUTE POOL my_pool
  FROM SPECIFICATION $$ ... $$;

EXECUTE JOB SERVICE
  IN COMPUTE POOL my_pool
  FROM SPECIFICATION $$ ... $$;

SHOW COMPUTE POOLS;
SHOW ENDPOINTS IN SERVICE my_service;
ALTER COMPUTE POOL my_pool SUSPEND;
```

### Service Specification YAML — Core Sections
- `containers`: image, resource requests/limits, env vars
- `endpoints`: name, port, protocol, `public: true/false`
- `volumes` (optional): stage mounts, ephemeral/local storage

### Networking & Egress
- Public endpoints require `public: true` in the endpoint spec plus appropriate network policy.
- Outbound calls from containers are **blocked by default** — require an **External Access Integration** bound to a **network rule**.

### Cost Control
- Compute pools bill **per second** (with a minimum) based on instance family, for all time **running**, including idle time.
- Default `AUTO_SUSPEND_SECS` = **3600** — lower this for GPU pools to avoid idle billing.
- Prefer **job services** over long-running services for infrequent batch workloads.

### Exam Traps
- Assuming Cortex can serve *any* custom model — it cannot; SPCS is required.
- Forgetting compute pools bill while idle if not suspended.
- Confusing job service (batch, terminates) with long-running service (persistent endpoint).
- Expecting outbound network access to work without an External Access Integration.
