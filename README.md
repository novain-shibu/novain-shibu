<h1 align="center">Novain Shibu</h1>

<p align="center">
  <b>Founder, AcoustiNode Systems</b> · <b>Performance bug hunter in open-source AI infrastructure</b><br>
  Embedded systems · AI · IoT
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/novain-shibu"><img src="https://img.shields.io/badge/LinkedIn-novain--shibu-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <img src="https://img.shields.io/badge/Open_to-internships_%26_collaborations-2ea44f?style=for-the-badge" alt="Open to internships and collaborations">
</p>

<p align="center">
  🏆 National Champion, Techletics 4.0 &nbsp;·&nbsp; 🏅 Samsung Solve for Tomorrow 2026, Top 40<br>
  📜 2 provisional patents &nbsp;·&nbsp; 📄 IEEE co-author<br>
  🧪 Published research with a DOI: <a href="https://doi.org/10.5281/zenodo.23393220"><b>speculative decoding for AI code editors</b></a>
</p>

<p align="center">
  Featured in <b>The Hindu BusinessLine</b> &nbsp;·&nbsp; <b>Moneycontrol</b> &nbsp;·&nbsp; <b>Malayala Manorama</b> &nbsp;·&nbsp; <b>Mathrubhumi</b> &nbsp;·&nbsp; <b>Samsung Newsroom</b>
</p>

---

## 🧪 Research

### Resync-and-Anchor Copy Drafting for Lossless Code Fast-Apply &nbsp;[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23393220.svg)](https://doi.org/10.5281/zenodo.23393220)

AI code editors apply an edit by having a model rewrite the whole file, most of it copied. I built a lossless
"copy-guesser" that lets the model confirm long copied spans in one pass, and compared it with Snowflake's
SuffixDecoding, the strongest open method (built into vLLM).

- **1.29× fewer model passes** than SuffixDecoding on edits never used for tuning (95% CI 1.22–1.37), output identical to normal decoding
- **24–37× faster than normal decoding** on a free T4 GPU; against SuffixDecoding the wall-clock was a tie on that GPU, and the report explains why
- A laptop simulator that predicted the GPU pass counts **exactly** in three runs, plus 7 ideas that failed, with numbers

[Report (PDF)](https://github.com/novain-shibu/resync-apply/releases/download/v1.0.0/REPORT.pdf) · [Code](https://github.com/novain-shibu/resync-apply) · Apache-2.0

### Prompt-injection defenses for AI agents

[**prompt-injection-relocation**](https://github.com/novain-shibu/prompt-injection-relocation): a stress test that moves logged prompt-injection attacks into other channels an agent reads (email, files, calendar, web).

- A defense gate that stopped **88%** of attacks where they were originally placed dropped to **28–62%** in 10 of 16 channel types
- A redesigned gate held **91–100%** in 14 of 16 (weak spot: delegated sources)
- Offline replay, reproducible in about 5 minutes

---

## 🔬 Open-source performance & correctness work

I find code that gets slower (or hungrier for memory) much faster than it should as inputs grow, or that gives wrong answers where two implementations should agree, prove it, and send a fix with tests. AI agents help me search; **every number below was re-measured by me**, because most claims don't survive verification.

> **1 fix shipped by Hugging Face** &nbsp;·&nbsp; **1 bug fixed by the OmegaConf maintainer** &nbsp;·&nbsp; **5 PRs merged**, into uvicorn (FastAPI's server), redis-py (the official Redis client, 2 PRs), nats.py (the NATS Python client) and Hugging Face datasets &nbsp;·&nbsp; **12 more under review** at uvicorn, Apache Airflow, Prefect, ONNX, xarray, Black (PSF), OGX, Meta (PyTorch, FAISS), Hugging Face, dask and CNCF Jaeger

| | Project | Result | Status |
|:-:|---|---|---|
| ✅ | **Hugging Face Hub**<br><sub>`hf cache prune` / `hf cache rm`</sub> | **79.8 s → 3.3 s**<br><sub>10,000-file cache</sub> | ![shipped](badges/shipped-5100.svg) <br><sub>Fixed in 36 h, crediting my suggested fix · [issue #5100](https://github.com/huggingface/huggingface_hub/issues/5100) · [fix #5118](https://github.com/huggingface/huggingface_hub/pull/5118)</sub> |
| ✅ | **uvicorn** (FastAPI's server)<br><sub>`--reload` startup</sub> | **120 s → 0.39 s**<br><sub>2,000 directories; users reported 2-min startups</sub> | ![merged](badges/pr-3184.svg) <br><sub>Merged by the maintainer · [PR #3184](https://github.com/Kludex/uvicorn/pull/3184)</sub> |
| ✅ | **redis-py** (official Redis client)<br><sub>sorted-set scores in the default mode</sub> | **Correctness fix**<br><sub>scores came back with 17 digits (`0.1` → `0.10000000000000001`); on a real Redis server 74,109 of 120,618 scores were wrong, 116 after the fix</sub> | ![merged](badges/pr-4427-merged.svg) <br><sub>Merged by the maintainer · [PR #4427](https://github.com/redis/redis-py/pull/4427)</sub> |
| ✅ | **redis-py** (official Redis client)<br><sub>sorted-set scores with `decode_responses=True`</sub> | **Correctness fix**<br><sub>scores came back as bytes where protocol 2 returns text (`zmpop`, `zdiff`, `zrank`...); checked on a real Redis server and a 3-node cluster</sub> | ![merged](badges/pr-4432-merged.svg) <br><sub>Merged by the maintainer · reported in [issue #4428](https://github.com/redis/redis-py/issues/4428) at the maintainer's request · fixed by [PR #4432](https://github.com/redis/redis-py/pull/4432)</sub> |
| ✅ | **nats.py** (NATS Python client, CNCF)<br><sub>JetStream pull `fetch()` after a server restart</sub> | **Hang fix**<br><sub>fetch stayed stuck after a restart or reconnect (forever with no timeout); on a real server 0 of 20 messages arrived, 20 of 20 in 0.3 s after the fix</sub> | ![merged](badges/pr-1065-merged.svg) <br><sub>Merged by the maintainer · [PR #1065](https://github.com/nats-io/nats.py/pull/1065) · fixes [issue #504](https://github.com/nats-io/nats.py/issues/504), open since 2023</sub> |
| ✅ | **Hugging Face datasets**<br><sub>loading datasets with many columns</sub> | **15–25 min → ~30 s**<br><sub>real 20,531-column dataset, identical output</sub> | ![merged](badges/pr-8777-merged.svg) <br><sub>Merged by the maintainer · reported in [issue #8776](https://github.com/huggingface/datasets/issues/8776) · fixed by [PR #8777](https://github.com/huggingface/datasets/pull/8777)</sub> |
| ✅ | **OmegaConf** (config library under Hydra)<br><sub>`OmegaConf.missing_keys()`</sub> | **Correctness fix**<br><sub>keys containing `.` or `[ ]` were reported with wrong paths, so two missing keys could merge into one</sub> | ![fixed](badges/fixed-1423-maintainer.svg) <br><sub>Reported by me, fixed by the maintainer the next day · [issue #1423](https://github.com/hydra-ecosystem/omegaconf/issues/1423) · [fix #1428](https://github.com/hydra-ecosystem/omegaconf/pull/1428)</sub> |
| 🐞 | **uvicorn** (FastAPI's server)<br><sub>new zttp HTTP engine</sub> | **Crash fix**<br><sub>`HEAD` requests with non-ASCII URLs got no response; found by differential fuzzing</sub> | ![open](badges/pr-3201.svg) <br><sub>Bug from the day before, reported with a fix · [PR #3201](https://github.com/Kludex/uvicorn/pull/3201)</sub> |
| 🌀 | **Apache Airflow**<br><sub>Graph view for large task groups</sub> | **10.3 s → 1.15 s**<br><sub>two task groups of 800 tasks, identical output</sub> | ![open](badges/pr-74574.svg) <br><sub>[PR #74574](https://github.com/apache/airflow/pull/74574)</sub> |
| 🔐 | **Prefect** (workflow orchestration)<br><sub>saving blocks that hold a list of credentials</sub> | **Silent data-corruption fix**<br><sub>18 of 20 saved entries came back wrong; 0 after the fix</sub> | ![open](badges/pr-23404.svg) <br><sub>Reported with a fix and tests · [issue #23403](https://github.com/PrefectHQ/prefect/issues/23403) · [PR #23404](https://github.com/PrefectHQ/prefect/pull/23404)</sub> |
| 🧮 | **ONNX** (Linux Foundation AI)<br><sub>reference evaluator, `ReduceSumSquare`</sub> | **Correctness fix**<br><sub>32-bit integer input came back as 64-bit, against the spec; found by differential fuzzing</sub> | ![open](badges/pr-8600.svg) <br><sub>Fix with tests · [PR #8600](https://github.com/onnx/onnx/pull/8600)</sub> |
| 🔤 | **xarray** (NumFOCUS)<br><sub>`.str` methods on text from pandas</sub> | **Crash fix**<br><sub>10 string methods (`split`, `pad`, `format`...) crashed on object-dtype text, e.g. after `DataFrame.to_xarray()`; found by differential fuzzing</sub> | ![open](badges/pr-11722.svg) <br><sub>Fix with 10 tests · [PR #11722](https://github.com/pydata/xarray/pull/11722)</sub> |
| 🖤 | **Black** (PSF, the Python formatter)<br><sub>`# fmt: skip` on an opening bracket</sub> | **Crash fix**<br><sub>Black failed with "produced invalid code" since 26.10.0; bisected to the change that caused it</sub> | ![open](badges/pr-5538.svg) <br><sub>Fix with a test · [PR #5538](https://github.com/psf/black/pull/5538)</sub> |
| 🧲 | **OGX** (formerly Meta's llama-stack)<br><sub>sqlite-vec vector search with a filter</sub> | **Silent wrong-results fix**<br><sub>filtered searches returned too few results, often none (0/10 → 10/10)</sub> | ![open](badges/pr-6779.svg) <br><sub>Reported with a fix and tests · [issue #6778](https://github.com/ogx-ai/ogx/issues/6778) · [PR #6779](https://github.com/ogx-ai/ogx/pull/6779)</sub> |
| 🧠 | **sentence-transformers**<br><sub>IR evaluator memory</sub> | **20.6 GB → 5.5 GB**<br><sub>real NQ data, 32k queries</sub> | ![open](badges/pr-4148.svg) <br><sub>[PR #4148](https://github.com/huggingface/sentence-transformers/pull/4148)</sub> |
| 🔎 | **FAISS** (Meta)<br><sub>binary k-NN, single query</sub> | **2.0–3.2× faster**<br><sub>2.02× on real binary embeddings</sub> | ![open](badges/pr-5701.svg) <br><sub>Plus a nested-OpenMP correctness fix · [PR #5701](https://github.com/facebookresearch/faiss/pull/5701) · [benchmarks](https://github.com/novain-shibu/faiss-binary-db-parallel)</sub> |
| 🔥 | **PyTorch** (Meta)<br><sub>`nn.Module.load_state_dict`</sub> | **159.8 s → 2.8 s**<br><sub>real forecasting model, 10,100 variables</sub> | ![analysis posted](badges/analysis.svg) <br><sub>Root cause of a 5-year-old issue · [analysis on #66787](https://github.com/pytorch/pytorch/issues/66787#issuecomment-6104965595)</sub> |
| 🧩 | **dask**<br><sub>graph optimization for long pipelines</sub> | **32.2 s → 0.80 s**<br><sub>2,000-operation chain, identical results</sub> | ![proposed](badges/proposed.svg) <br><sub>Fix for a 3-year-old open issue · [comment on #9795](https://github.com/dask/dask/issues/9795#issuecomment-6111306597)</sub> |
| 🧭 | **Jaeger** (CNCF)<br><sub>trace clock-skew adjuster</sub> | **Correctness fix**<br><sub>follows-from spans wrongly shifted</sub> | ![proposed](badges/proposed.svg) <br><sub>Issue open since 2017 · [proposal on #448](https://github.com/jaegertracing/jaeger/issues/448)</sub> |

<sub>"open", "proposed" and "posted" mean under review, not merged yet.</sub>

<details>
<summary><b>How I find them</b></summary>
<br>

1. **Scaling ladders:** run an operation at sizes 1×, 2×, 4×, 8×… and measure time and peak memory. Anything growing faster than it should is a candidate.
2. **Differential fuzzing:** send the same random inputs to two implementations that must agree (e.g. uvicorn's HTTP engines) and investigate every difference.
3. **Invariance checks:** change settings that must not change the answer (batch size, chunk size, order, dtype) and diff the outputs.
4. **Floor check:** compare real cost with the physical minimum (bytes moved, work done).
5. **Prove it on real data**, check the fixed version gives identical results, run the project's tests, then report.

One agent once told me it had found a 700× speedup "with 100% confidence". The real number was 59–310×, and my own test harness was wrong twice before I trusted it.
</details>

---

## 🌊 AcoustiNode Systems

In 2018 I watched floodwater swallow the roads around Chalakudy. Eight years later I'm still working on that problem.

**AcoustiNode** is a VHF radio telemetry network for dams and reservoirs. It moves water-level and flow data between upstream, dam-site and downstream points over a link that stays up when cellular and internet go down, which is exactly when release decisions matter most.

- Built the prototype myself: sensors, Raspberry Pi and Arduino edge nodes, radio encoding, Python
- Demonstrated at **Peechi Dam** to the KERI Director, Joint Director and Irrigation Department officials

---

## 🏞️ Dam management project

Working with **Government Engineering College Idukki** on a system for managing a cascade of dams (several dams on the same river system).

- Co-inventor, provisional patent for the **dam cascade management system**

---

## 🏆 Recognition

| | |
|---|---|
| 🥇 | **1st place**, Techletics 4.0 national competition |
| 🏅 | **Top 40 national semi-finalist**, Samsung Solve for Tomorrow 2026 |
| 📜 | **2 provisional patents** |
| 📄 | **Co-author, IEEE paper** (accepted, publication forthcoming) |
| 📰 | Featured in **Moneycontrol, The Hindu BusinessLine, EdexLive, Mathrubhumi, Manorama, Analytics India Magazine, Samsung Newsroom** and more |

---

## 🛠️ Tools

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?logo=pytorch&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?logo=cplusplus&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi-A22846?logo=raspberrypi&logoColor=white)
![Arduino](https://img.shields.io/badge/Arduino-00878F?logo=arduino&logoColor=white)
![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?logo=kaggle&logoColor=white)

---

<p align="center">
  <b>Open to internships and collaborations</b> in AI infrastructure, performance engineering and embedded/IoT,<br>
  and to being told I'm wrong about something.<br><br>
  <a href="https://www.linkedin.com/in/novain-shibu">LinkedIn</a> · Student: Computer Science with Physics, Chemistry and Maths
</p>
