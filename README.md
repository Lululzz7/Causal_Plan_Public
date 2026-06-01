# Causal Planning Code Release

This repository contains the source code for benchmark evaluation, data generation, QA filtering, causal trace generation, supervised fine-tuning, and reward scoring for multimodal causal-planning research.

Benchmark data, training data, model checkpoints, generated outputs, and runtime caches are not stored in this repository. Evaluation scripts accept an external benchmark data directory through `BENCHMARK_DATA_ROOT`.

## Repository Layout

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

## External Data Layout

For evaluation, provide an external benchmark data directory with this layout:

```text
benchmark_data/
  mcq/
  qa/
  multimodal_data/
```

Place the external benchmark data directory at `benchmark_data/` under the repository root, or set `BENCHMARK_DATA_ROOT` explicitly.

## Quick Validation

Install the evaluation dependencies:

```bash
cd evaluation
python -m pip install -r requirements.txt
```

Validate prompt definitions, data structure, media references, and dry-run evaluation logic:

```bash
cd evaluation
BENCHMARK_DATA_ROOT=<BENCHMARK_DATA_DIR> \
bash validate_benchmark_prompts_and_data.sh
```

The validation script compiles the evaluation code, checks task-specific open-QA judge prompts, verifies MCQ and open-QA rows, checks media availability, and runs dry-run evaluation without model API calls.

## Full Evaluation

```bash
cd evaluation
BENCHMARK_DATA_ROOT=<BENCHMARK_DATA_DIR> bash run_full_benchmark_evaluation.sh
```

The full evaluation script runs MCQ evaluation and open-QA generation plus rubric judging. Model aliases and provider settings are defined in `evaluation/open_qa_model_registry.json`.

## Main Entry Points

```text
evaluation/run_full_benchmark_evaluation.sh
evaluation/validate_benchmark_prompts_and_data.sh

four_stage_generation/single_step/generate_single_step_four_stage_dataset.py
four_stage_generation/multi_step/run_multi_step_four_stage_pipeline.py

qa_generation/qa_generators/stage_one_qa_generator/generate_stage_one_qa_parallel.py
qa_generation/qa_generators/stage_two_qa_generator/run_stage_two_qa_generation.sh

qa_filtering/score_existing_qa_qwen_two_axis.py
qa_filtering/score_existing_qa_gemini_physical_logic.py
qa_filtering/filter_existing_qa_physical_logic_audit.py

causal_trace_generation/run_task_specific_causal_trace_generation.sh

sft_training/prepare_swift_sft_dataset.py
sft_training/run_swift_sft.sh

rl_training/causal_learner_reward/compute_score.py
```

## Module Summaries

- `evaluation/`: evaluates MCQ and open-QA benchmark items. Open-QA scoring uses task-specific rubric judge prompts.
- `four_stage_generation/`: generates structured plans, localizes steps, refines keyframes, and produces atomic action records.
- `qa_generation/`: contains the active 20-task QA prompt registry and retained QA generation entry points.
- `qa_filtering/`: contains Qwen two-axis scoring for visual grounding and logical coherence plus Gemini physical-logic scoring and binary auditing.
- `causal_trace_generation/`: adds task-specific causal reasoning traces to generated QA rows and validates trace contracts.
- `sft_training/`: converts QA JSONL rows to SWIFT-compatible SFT data and provides a SWIFT launch wrapper.
- `rl_training/`: provides the Causal Learner reward code, including rule-based scoring and optional multimodal judge-backed scoring.

## Notes

All prompt content required by the active generation, filtering, evaluation, SFT, and reward code is stored in Python or shell source files. Markdown files are kept only as README files.

Model-backed generation, filtering, and evaluation require the relevant Azure OpenAI or OpenAI-compatible endpoint credentials documented in each module README.
