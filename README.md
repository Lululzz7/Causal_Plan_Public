# Repository Overview

This repository contains the source code needed for benchmark evaluation, data generation, QA filtering, causal trace generation, supervised fine-tuning, and reward scoring.

Benchmark data is not stored in this repository. Evaluation scripts accept an external benchmark data directory through `BENCHMARK_DATA_ROOT`.

## Contents

```text
causal_trace_generation/
  Task-specific causal reasoning trace generation and validation utilities.

evaluation/
  Standalone MCQ and open-QA benchmark evaluation code without benchmark data.

four_stage_generation/
  Single-step and multi-step four-stage plan and action generation pipelines.

qa_generation/
  QA generators and the active 20-task prompt registry.

qa_filtering/
  Qwen3.5-397B-A17B two-axis QA scoring and Gemini physical-logic filtering code.

rl_training/
  Standalone Causal Learner reward code for RL training.

sft_training/
  Standalone SWIFT-based supervised fine-tuning entry points without training data.
```

## Evaluation

Place the external benchmark data directory at `benchmark_data/` under the repository root, or set `BENCHMARK_DATA_ROOT` explicitly.

Validate prompts, data structure, media references, and dry-run evaluation logic:

```bash
cd evaluation
bash validate_benchmark_prompts_and_data.sh
```

Run the full evaluation:

```bash
cd evaluation
BENCHMARK_DATA_ROOT=<BENCHMARK_DATA_DIR> bash run_full_benchmark_evaluation.sh
```

## Data Policy

This code release intentionally excludes benchmark data, training data, model checkpoints, runtime caches, generated outputs, machine-specific job scripts, and environment-specific activation commands.

## Notes

All prompt content required by the active generation, filtering, evaluation, SFT, and reward code is stored in Python or shell source files. Markdown files are kept only as README files.
