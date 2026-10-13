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
  🏆 National Champion, Techletics 4.0 &nbsp;·&nbsp; 🏅 Samsung Solve for Tomorrow 2026, Top 40 &nbsp;·&nbsp; 📜 2 provisional patents &nbsp;·&nbsp; 📄 IEEE co-author
</p>

---

## 🔬 Open-source performance & correctness work

I find code that gets slower (or hungrier for memory) much faster than it should as inputs grow, or that gives wrong answers where two implementations should agree, prove it, and send a fix with tests. AI agents help me search; **every number below was re-measured by me**, because most claims don't survive verification.

> **1 fix shipped by Hugging Face** &nbsp;·&nbsp; **1 PR merged into uvicorn** (FastAPI's server) &nbsp;·&nbsp; **10 more under review** at uvicorn, Apache Airflow, Celery, marimo, Meta (PyTorch, FAISS), Hugging Face, dask and CNCF Jaeger

| | Project | Result | Status |
|:-:|---|---|---|
| ✅ | **Hugging Face Hub**<br><sub>`hf cache prune` / `hf cache rm`</sub> | **79.8 s → 3.3 s**<br><sub>10,000-file cache</sub> | ![shipped](badges/shipped.svg) <br><sub>Fixed in 36 h, crediting my suggested fix · [issue #5100](https://github.com/huggingface/huggingface_hub/issues/5100) · [fix #5118](https://github.com/huggingface/huggingface_hub/pull/5118)</sub> |
| ✅ | **uvicorn** (FastAPI's server)<br><sub>`--reload` startup</sub> | **120 s → 0.39 s**<br><sub>2,000 directories; users reported 2-min startups</sub> | ![merged](badges/pr-3184.svg) <br><sub>Merged by the maintainer · [PR #3184](https://github.com/Kludex/uvicorn/pull/3184)</sub> |
| 🐞 | **uvicorn** (FastAPI's server)<br><sub>new zttp HTTP engine</sub> | **Crash fix**<br><sub>`HEAD` requests with non-ASCII URLs got no response; found by differential fuzzing</sub> | ![open](badges/pr-3201.svg) <br><sub>Bug from the day before, reported with a fix · [PR #3201](https://github.com/Kludex/uvicorn/pull/3201)</sub> |
| 🌀 | **Apache Airflow**<br><sub>Graph view for large task groups</sub> | **10.3 s → 1.15 s**<br><sub>two task groups of 800 tasks, identical output</sub> | ![open](badges/pr-74574.svg) <br><sub>[PR #74574](https://github.com/apache/airflow/pull/74574)</sub> |
| 🔗 | **Celery**<br><sub>building long task chains</sub> | **54.3 s → 0.024 s**<br><sub>4,000-task chain, ~2,000× faster</sub> | ![open](badges/pr-10850.svg) <br><sub>[PR #10850](https://github.com/celery/celery/pull/10850)</sub> |
| 📓 | **marimo** (Python notebooks)<br><sub>re-running a cell in large notebooks</sub> | **243 ms → 20 ms**<br><sub>1,000-cell notebook, every run and UI interaction; 19× at 2,000 cells</sub> | ![open](badges/pr-11196.svg) <br><sub>[PR #11196](https://github.com/marimo-team/marimo/pull/11196)</sub> |
| ⚡ | **Hugging Face datasets**<br><sub>datasets with many columns</sub> | **15–25 min → ~30 s**<br><sub>real 20,531-column dataset</sub> | ![open](badges/pr-8777.svg) <br><sub>[issue #8776](https://github.com/huggingface/datasets/issues/8776) · [PR #8777](https://github.com/huggingface/datasets/pull/8777)</sub> |
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

## 🛡️ Research: prompt-injection defenses for AI agents

[**prompt-injection-relocation**](https://github.com/novain-shibu/prompt-injection-relocation): a stress test that moves logged prompt-injection attacks into other channels an agent reads (email, files, calendar, web).

- A defense gate that stopped **88%** of attacks where they were originally placed dropped to **28–62%** in 10 of 16 channel types
- A redesigned gate held **91–100%** in 14 of 16 (weak spot: delegated sources)
- Offline replay, reproducible in about 5 minutes

---

## 🌊 AcoustiNode Systems

In 2018 I watched floodwater swallow the roads around Chalakudy. Eight years later I'm still working on that problem.

**AcoustiNode** is a VHF radio telemetry network for dams and reservoirs. It moves water-level and flow data between upstream, dam-site and downstream points over a link that stays up when cellular and internet go down, which is exactly when release decisions matter most.

- Built the prototype myself: sensors, Raspberry Pi and Arduino edge nodes, radio encoding, Python
- Demonstrated at **Peechi Dam** to the KERI Director, Joint Director and Irrigation Department officials
- Co-inventor, provisional patent for a **dam cascade management system** (with Idukki engineering college)

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
