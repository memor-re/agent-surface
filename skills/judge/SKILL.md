---
name: "judge"
description: "Council-level consistency judge. Runs a query through multiple models (hosted + Ollama fallback) and measures split-brain: whether the models agree on recall, attribution, or factual claims. Surfaces divergence as a structured finding. Wraps the Ollama-backed consistency judge in packages/memory-evals (Intelligence Spine). Route here when cross-model validation, split-brain measurement, or model-quality-regression scoring is needed."
---

# judge

This is the public invocation surface for a memor.re skill. Its behavior is
served by the memor.re hosted runtime and is not distributed in this mirror.

To use it, invoke the hosted memor.re Council MCP and route to this skill by
name. See https://memor.re for access.
