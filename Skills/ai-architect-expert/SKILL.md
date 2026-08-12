---
name: ai-architect-expert
description: >
  Expert AI/ML system design — serving vs batch, feature stores, model registry, training pipelines,
  MLOps, monitoring, drift, cost, and scale. Use when designing LLM/ML platforms, model deployment,
  inference architecture, RAG at scale, GPU serving, or MLOps CI/CD — even if the user only said
  "how should we deploy this model" or "design the AI stack".
---

# AI Architect Expert

Guidance for AI system architecture, MLOps, and production ML/LLM infrastructure. Prefer clear tradeoffs and phased delivery over boilerplate-heavy MVP stacks.

## Core concepts

### AI system architecture

- Model serving (real-time vs batch vs streaming)
- Feature stores (online/offline consistency)
- Model registry and promotion stages
- Training and data versioning pipelines

### MLOps

- CI/CD for models and prompts (where applicable)
- Observability: latency, quality, cost, drift
- Safe rollout: canary, shadow, blue/green
- Retraining triggers and rollback

### Scalability

- Distributed training when data/model size requires it
- Inference optimization: batching, caching, quantization
- Load balancing and autoscaling
- Cost controls (GPU utilization, spot, right-sizing)

## How to respond

1. **Clarify constraints** — latency SLO, budget, team size, cloud/on-prem, regulated data.
2. **Propose a tiered design** — MVP path vs production path; what to defer.
3. **Call out decisions** — suggest ADR-worthy choices (`retrospective` skill → `specs/adr/`).
4. **Ground in the user's stack** — do not default to a full Kubeflow stack for a single FastAPI service.

## Implementation sketches

Python reference patterns (registry, feature store, serving, monitoring) live in **`references/platform-snippets.md`**. Load when the user wants illustrative code, not when a diagram and decision list suffice.

## Best practices

### Architecture

- Separate training and serving environments; keep artifact parity
- Version models, data snapshots, and prompts from day one
- Plan rollback before first production deploy
- Build monitoring into the design, not as an afterthought

### MLOps

- Automate promote/demote through the registry
- Track data drift and quality decay with alerts
- Version everything that affects inference output

### Infrastructure

- Batch requests where latency allows
- Cache idempotent predictions when safe
- Right-size GPU/CPU; measure before optimizing

## Anti-patterns

- No model versioning or registry
- Training/serving skew (different deps or features)
- No monitoring, drift checks, or rollback
- Manual, unrepeatable deploys
- Platform overkill for an MVP with one model

## Resources

- MLflow: https://mlflow.org/
- Kubeflow: https://www.kubeflow.org/
- BentoML: https://www.bentoml.com/
- Weights & Biases: https://wandb.ai/
