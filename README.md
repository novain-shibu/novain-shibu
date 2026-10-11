# Novain Shibu - hidden slowdowns found in AI / Python libraries

I find performance cliffs (time or memory that grows much faster than it should) in widely used libraries,
prove them on real public data, and send a fix with tests. I use AI agents for the search; every number
below was measured and checked by me.

GitHub: https://github.com/novain-shibu

## Shipped
| Library | What | Result | Link |
|---|---|---|---|
| huggingface_hub (Hugging Face) | `hf cache prune` / `hf cache rm` slowed down quadratically with files per revision | 10k files: 79.8 s -> 3.3 s. Fixed and released in v2.2.0, crediting my suggested fix | [issue #5100](https://github.com/huggingface/huggingface_hub/issues/5100), [fix #5118](https://github.com/huggingface/huggingface_hub/pull/5118) |

## Under review (not merged yet)
| Library | What | Evidence | Link |
|---|---|---|---|
| PyTorch (Meta) | `load_state_dict` scans every key for every child module; explains a 5-year-old open issue | Real pytorch-forecasting model at 10,100 variables: 159.8 s -> 2.8 s, identical result | [comment on #66787](https://github.com/pytorch/pytorch/issues/66787#issuecomment-6104965595) |
| datasets (Hugging Face) | Loading datasets with many columns is quadratic in columns | Real 20,531-column dataset: load 15-25 min -> ~30 s, identical data, 1,969 tests pass | [issue #8776](https://github.com/huggingface/datasets/issues/8776), [PR #8777](https://github.com/huggingface/datasets/pull/8777) |
| sentence-transformers (Hugging Face) | IR evaluator uses ~4x memory since v5.7.0 | Real NQ data, 32k queries: 20.55 GB -> 5.46 GB, identical metrics | [PR #4148](https://github.com/huggingface/sentence-transformers/pull/4148) |
| uvicorn (FastAPI's server) | `--reload` directory check is quadratic (2-minute startups reported by users) | 59-310x faster, same result on 3,000 cases | [PR #3184](https://github.com/Kludex/uvicorn/pull/3184) |
| FAISS (Meta) | Binary flat index used one thread for single-query search | 2.65-3.16x synthetic, 2.02x on real binary embeddings | [PR #5701](https://github.com/facebookresearch/faiss/pull/5701) |

## How
An automated harness that (1) changes settings that must not change the answer (batch size, chunk size,
order, dtype) and diffs outputs, and (2) measures time and peak memory as input size doubles, compared
with the physical minimum. Findings are re-checked on real public datasets before anything is reported.
