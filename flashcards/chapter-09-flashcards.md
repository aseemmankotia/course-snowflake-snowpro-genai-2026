## Chapter 9: Snowpark Container Services for Gen AI Workloads — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Default AUTO_SUSPEND_SECS for a Snowpark Container Services compute pool? | 3600 seconds (1 hour) of inactivity. Exam tip: unlike warehouses (default 600s), compute pools default to a longer idle window — lower it to cut GPU costs. |
| 2 | GPU_NV_L compute pool node — GPU count and memory? | Multiple NVIDIA A100 GPUs with 80GB memory each. Exam tip: largest GPU family, used for big open-source LLM inference/fine-tuning. |
| 3 | GPU_NV_S compute pool node — GPU count and type? | Single NVIDIA A10G GPU, 24GB memory. Exam tip: smallest/cheapest GPU option, fine for small custom models or embedding models. |
| 4 | When should you use a native Cortex function instead of SPCS? | When the required model (e.g., llama3, mistral, snowflake-arctic) is already available via Cortex — no infra, no container, pay-per-use. Exam tip: 'supported model + no custom code' → Cortex. |
| 5 | When must you use Snowpark Container Services instead of Cortex functions? | Custom/open-source model not in Cortex's supported list, custom runtime/dependencies, or need for full control over serving code and GPU frameworks. Exam tip: 'model not available in Cortex' is the trigger phrase for SPCS. |
| 6 | Job service vs long-running service in SPCS? | EXECUTE JOB SERVICE runs to completion then stops (batch/training). CREATE SERVICE stays running to serve a persistent endpoint. Exam tip: batch scoring = job service; real-time inference API = long-running service. |
| 7 | Command to run a one-time batch container job on a compute pool? | EXECUTE JOB SERVICE IN COMPUTE POOL <pool> FROM SPECIFICATION <spec>. Exam tip: distinct from CREATE SERVICE, which creates a persistent endpoint. |
| 8 | Command to view the endpoint URL of a running SPCS service? | SHOW ENDPOINTS IN SERVICE <service_name>. Exam tip: returns the ingress URL used to call the service from outside Snowflake. |
| 9 | What lets an SPCS container make outbound calls to a third-party API? | An External Access Integration bound to the service, referencing a network rule that allows the destination host/port. Exam tip: without it, outbound network calls from the container are blocked by default. |
| 10 | What does a service specification YAML for SPCS define? | Containers (image, resources, env vars), endpoints (port, protocol, public flag), and optional volumes/storage mounts. Exam tip: this YAML is analogous to a Kubernetes pod spec. |
| 11 | How is compute pool usage billed? | Per-second billing (with a minimum) in Snowflake credits based on node instance family, charged the entire time the pool is running/not suspended — even if idle. Exam tip: set a low AUTO_SUSPEND_SECS or manually suspend to avoid paying for idle GPU nodes. |
| 12 | Commands to get a custom model container image into Snowflake? | CREATE IMAGE REPOSITORY, then `docker login` and `docker push <repo_url>/<image>:<tag>`. Exam tip: images live in a Snowflake-hosted image repository, referenced by the service spec. |
