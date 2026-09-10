# MemSyco-Bench Formal Experiment Results

This archive contains the formal MemSyco-Bench retrieval and generation
results for three memory systems, two models, and two answer-generation
methods.

## Scope

- Dataset: MemSyco-Bench, 1,550 rows
- Tasks:
  - Objective Fact Judgment: 300
  - Contextual Scope Control: 300
  - Memory-Evidence Conflict: 300
  - Personalized Memory Use: 300
  - Valid Memory Selection: 350
- Memory systems: AMEM, Mem0, naiveRAG
- Models: DeepSeek and GPT
- Methods: Baseline and M6

## Retrieval Protocol

Each memory system uses one frozen top-10 retrieval file. The retrieval
results are shared by the DeepSeek and GPT formal runs; the former model
specific copies were byte-identical and were deduplicated.

- `top_k`: 10
- Embedding model: `BAAI/bge-m3`
- Embedding dimension: 1024
- Retrieval files: `retrieval/{AMEM,Mem0,naiveRAG}/retrieved_top10.jsonl`

The downstream GPT runs reused these frozen retrieval records. They did not
rerun retrieval with GPT.

## Generation and Judge Models

| Experiment group | AMEM | Mem0 | naiveRAG |
|---|---|---|---|
| DeepSeek Baseline | `deepseek-v4-flash` | `deepseek-chat` | `deepseek-v4-flash` |
| DeepSeek M6 | `deepseek-v4-flash` | `deepseek-chat` | `deepseek-v4-flash` |
| GPT Baseline | `gpt-5.6-sol` | `gpt-5.6-sol` | `gpt-5.6-sol` |
| GPT M6 | `gpt-5.6-sol` | `gpt-5.6-sol` | `gpt-5.6-sol` |

The persisted result metadata records the same model family for answer
generation and Judge within each experiment group. Every method/system/model
combination contains 1,550 answer rows and 1,550 Judge rows.

Baseline generates an answer directly after retrieval. M6 performs its
risk-assessment and reflection stages before final answer generation.

## Result Layout

```text
retrieval/{AMEM,Mem0,naiveRAG}/retrieved_top10.jsonl
baseline/{DeepSeek,GPT}/{AMEM,Mem0,naiveRAG}/
  outputs.jsonl
  judge.jsonl
m6/{DeepSeek,GPT}/{AMEM,Mem0,naiveRAG}/
  outputs.jsonl
  judge.jsonl
reports/official/{DeepSeek,GPT}/{AMEM,Mem0,naiveRAG}/
  summary.json
  summary.csv
  validation_report.json
  parallel_post_merge_report.json
SHA256SUMS.txt
```

The `m6/` directory corresponds to the original local `ours/` directory.

## Formal Macro Results

The values below are macro task-pass results copied from the corresponding
official summary files.

| Model | Memory system | Baseline | M6 |
|---|---|---:|---:|
| DeepSeek | AMEM | 0.7584 | 0.8594 |
| DeepSeek | Mem0 | 0.5180 | 0.6455 |
| DeepSeek | naiveRAG | 0.7661 | 0.8476 |
| GPT | AMEM | 0.8250 | 0.8336 |
| GPT | Mem0 | 0.6094 | 0.6208 |
| GPT | naiveRAG | 0.8293 | 0.8428 |

## Integrity and Exclusions

`SHA256SUMS.txt` records SHA-256 hashes for every archived result and report
file. API keys, environment files, process logs, remote bundles, local
embedding stores, and temporary shard files are intentionally excluded.
