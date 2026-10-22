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
  🧪 <a href="#research"><b>4 published research notes with DOIs</b></a>
</p>

<p align="center">
  Featured in <b>The Hindu BusinessLine</b> &nbsp;·&nbsp; <b>Moneycontrol</b> &nbsp;·&nbsp; <b>Malayala Manorama</b> &nbsp;·&nbsp; <b>Mathrubhumi</b> &nbsp;·&nbsp; <b>Samsung Newsroom</b>
</p>

---

<a name="research"></a>
## 🧪 Research

| Project | What it is | Key result | |
|---|---|---|---|
| **SeenRead** [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23471690.svg)](https://doi.org/10.5281/zenodo.23471690) | Proves, word by word and without OCR, whether the text an AI or search engine extracts from a PDF is the text a human sees (font-remap lies, hidden text, garbled Indian-language text) | Fresh blind tests: 99.98% of words exact, 0 harmful errors in the latest; beats Tesseract OCR (92-98%) in English, Hindi, Bengali, Tamil | [details ↓](#seenread) |
| **FinePDFs audit** [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23432200.svg)](https://doi.org/10.5281/zenodo.23432200) | Audit of Hugging Face's FinePDFs AI training dataset, traced back to the PDFs | ~11B tokens of bad text, rough extrapolation (AI OCR invented ~4.8B in 7 Indian and Sri Lankan scripts; ~6B broken Persian filed under small languages); 94% of 791 sampled Indian government regional-language PDFs have no correct text (scans or garbled); in a controlled test, Word 365's Save as PDF garbled ~15 scripts (same text and font: Edge loses no letters, Word loses/garbles many: letter F1 97-99.5% vs 34-90%); recovery without OCR: 99% exact words | [details ↓](#finepdfs-audit) |
| **Unshape** [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23418945.svg)](https://doi.org/10.5281/zenodo.23418945) | Exact text from broken Indian-language PDFs, no OCR: 10 Indian scripts + Sinhala (v0.2.0) | 99.4% exact words on Word 365 PDFs; 98.5% on blind real PDFs from one publisher (PyMuPDF 72.4%) | [details ↓](#unshape) |
| **spectrace** [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23413796.svg)](https://doi.org/10.5281/zenodo.23413796) | Predicting speculative-decoding gains without running the drafter | Matched real engines within 0.02–0.31% | [details ↓](#spectrace) |
| **resync-apply** [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23393220.svg)](https://doi.org/10.5281/zenodo.23393220) | Lossless copy drafting for AI code editors | 1.29× fewer model passes than Snowflake's SuffixDecoding | [details ↓](#resync-apply) |
| **prompt-injection-relocation** | Stress test for AI-agent prompt-injection defenses | A gate that stopped 88% of attacks dropped to 28–62% when attacks moved channel | [details ↓](#prompt-injection) |

---

## 🔬 Open-source performance & correctness work

I find code that gets slower (or hungrier for memory) much faster than it should as inputs grow, or that gives wrong answers where two implementations should agree, prove it, and send a fix with tests. AI agents help me search; **every number below was re-measured by me**, because most claims don't survive verification.

> **1 fix shipped by Hugging Face** &nbsp;·&nbsp; **1 bug fixed by the OmegaConf maintainer** &nbsp;·&nbsp; **10 PRs merged**, into uvicorn (FastAPI's server), redis-py (the official Redis client, 3 PRs), nats.py (the NATS Python client), Hugging Face datasets, xarray (NumFOCUS, 2 PRs), Prefect and Black (PSF) &nbsp;·&nbsp; **9 more under review** at uvicorn, Apache Airflow, ONNX, OGX, Meta (PyTorch, FAISS), Hugging Face, dask and CNCF Jaeger

| | Project | Result | Status |
|:-:|---|---|---|
| ✅ | **Hugging Face Hub**<br><sub>`hf cache prune` / `hf cache rm`</sub> | **79.8 s → 3.3 s**<br><sub>10,000-file cache</sub> | ![shipped](badges/shipped-5100.svg) <br><sub>Fixed in 36 h, crediting my suggested fix · [issue #5100](https://github.com/huggingface/huggingface_hub/issues/5100) · [fix #5118](https://github.com/huggingface/huggingface_hub/pull/5118)</sub> |
| ✅ | **uvicorn** (FastAPI's server)<br><sub>`--reload` startup</sub> | **120 s → 0.39 s**<br><sub>2,000 directories; users reported 2-min startups</sub> | ![merged](badges/pr-3184.svg) <br><sub>Merged by the maintainer · [PR #3184](https://github.com/Kludex/uvicorn/pull/3184)</sub> |
| ✅ | **redis-py** (official Redis client)<br><sub>sorted-set scores in the default mode</sub> | **Correctness fix**<br><sub>scores came back with 17 digits (`0.1` → `0.10000000000000001`); on a real Redis server 74,109 of 120,618 scores were wrong, 116 after the fix</sub> | ![merged](badges/pr-4427-merged.svg) <br><sub>Merged by the maintainer · [PR #4427](https://github.com/redis/redis-py/pull/4427)</sub> |
| ✅ | **redis-py** (official Redis client)<br><sub>sorted-set scores with `decode_responses=True`</sub> | **Correctness fix**<br><sub>scores came back as bytes where protocol 2 returns text (`zmpop`, `zdiff`, `zrank`...); checked on a real Redis server and a 3-node cluster</sub> | ![merged](badges/pr-4432-merged.svg) <br><sub>Merged by the maintainer · reported in [issue #4428](https://github.com/redis/redis-py/issues/4428) at the maintainer's request · fixed by [PR #4432](https://github.com/redis/redis-py/pull/4432)</sub> |
| ✅ | **redis-py** (official Redis client)<br><sub>MultiDB clients (`MultiDBClient` and its pipeline)</sub> | **Crash fix**<br><sub>`slowlog_get()`, `acl_setuser()` and other commands raised `AttributeError` on MultiDB clients, sync and asyncio; checked on a real Redis server</sub> | ![merged](badges/pr-4442-merged.svg) <br><sub>Merged by the maintainer · reported in [issue #4440](https://github.com/redis/redis-py/issues/4440) at the maintainer's request · fixed by [PR #4442](https://github.com/redis/redis-py/pull/4442)</sub> |
| ✅ | **nats.py** (NATS Python client, CNCF)<br><sub>JetStream pull `fetch()` after a server restart</sub> | **Hang fix**<br><sub>fetch stayed stuck after a restart or reconnect (forever with no timeout); on a real server 0 of 20 messages arrived, 20 of 20 in 0.3 s after the fix</sub> | ![merged](badges/pr-1065-merged.svg) <br><sub>Merged by the maintainer · [PR #1065](https://github.com/nats-io/nats.py/pull/1065) · fixes [issue #504](https://github.com/nats-io/nats.py/issues/504), open since 2023</sub> |
| ✅ | **Hugging Face datasets**<br><sub>loading datasets with many columns</sub> | **15–25 min → ~30 s**<br><sub>real 20,531-column dataset, identical output</sub> | ![merged](badges/pr-8777-merged.svg) <br><sub>Merged by the maintainer · reported in [issue #8776](https://github.com/huggingface/datasets/issues/8776) · fixed by [PR #8777](https://github.com/huggingface/datasets/pull/8777)</sub> |
| ✅ | **xarray** (NumFOCUS)<br><sub>`.str` methods on text from pandas</sub> | **Crash fix**<br><sub>10 string methods (`split`, `pad`, `format`...) crashed on object-dtype text, e.g. after `DataFrame.to_xarray()`; found by differential fuzzing</sub> | ![merged](badges/pr-11722-merged.svg) <br><sub>Merged by the maintainer · fix with 10 tests · [PR #11722](https://github.com/pydata/xarray/pull/11722)</sub> |
| ✅ | **xarray** (NumFOCUS)<br><sub>`.str.replace` / `.str.contains` with `case=False`</sub> | **Correctness fix**<br><sub>`replace(..., case=False, regex=False)` silently ignored `case=False`, and `contains` crashed on object-dtype text</sub> | ![merged](badges/pr-11747-merged.svg) <br><sub>Merged by the maintainer · fix with tests · [PR #11747](https://github.com/pydata/xarray/pull/11747)</sub> |
| ✅ | **Prefect** (workflow orchestration)<br><sub>saving blocks that hold a list of credentials</sub> | **Silent data-corruption fix**<br><sub>18 of 20 saved entries came back wrong; 0 after the fix</sub> | ![merged](badges/pr-23404-merged.svg) <br><sub>Merged by the maintainer · reported with a fix and tests in [issue #23403](https://github.com/PrefectHQ/prefect/issues/23403) · fixed by [PR #23404](https://github.com/PrefectHQ/prefect/pull/23404)</sub> |
| ✅ | **Black** (PSF, the Python formatter)<br><sub>`# fmt: skip` on an opening bracket</sub> | **Crash fix**<br><sub>Black failed with "produced invalid code" since 26.10.0; bisected to the change that caused it</sub> | ![merged](badges/pr-5538-merged.svg) <br><sub>Merged by the maintainer · fix with a test · [PR #5538](https://github.com/psf/black/pull/5538)</sub> |
| ✅ | **OmegaConf** (config library under Hydra)<br><sub>`OmegaConf.missing_keys()`</sub> | **Correctness fix**<br><sub>keys containing `.` or `[ ]` were reported with wrong paths, so two missing keys could merge into one</sub> | ![fixed](badges/fixed-1423-maintainer.svg) <br><sub>Reported by me, fixed by the maintainer the next day · [issue #1423](https://github.com/hydra-ecosystem/omegaconf/issues/1423) · [fix #1428](https://github.com/hydra-ecosystem/omegaconf/pull/1428)</sub> |
| 🐞 | **uvicorn** (FastAPI's server)<br><sub>new zttp HTTP engine</sub> | **Crash fix**<br><sub>`HEAD` requests with non-ASCII URLs got no response; found by differential fuzzing</sub> | ![open](badges/pr-3201.svg) <br><sub>Bug from the day before, reported with a fix · [PR #3201](https://github.com/Kludex/uvicorn/pull/3201)</sub> |
| 🌀 | **Apache Airflow**<br><sub>Graph view for large task groups</sub> | **10.3 s → 1.15 s**<br><sub>two task groups of 800 tasks, identical output</sub> | ![open](badges/pr-74574.svg) <br><sub>[PR #74574](https://github.com/apache/airflow/pull/74574)</sub> |
| 🧮 | **ONNX** (Linux Foundation AI)<br><sub>reference evaluator, `ReduceSumSquare`</sub> | **Correctness fix**<br><sub>32-bit integer input came back as 64-bit, against the spec; found by differential fuzzing</sub> | ![open](badges/pr-8600.svg) <br><sub>Fix with tests · [PR #8600](https://github.com/onnx/onnx/pull/8600)</sub> |
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

## 🧪 Research details

<a name="seenread"></a>
### SeenRead: proving that the text a machine reads from a PDF is the text a human sees &nbsp;[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23471690.svg)](https://doi.org/10.5281/zenodo.23471690)

PDFs can show people one text while giving machines another: a font that draws "1" where its table says "9", an edited
text table, white or invisible text. AI pipelines, CV screeners, search engines and screen readers read the machine
version. SeenRead identifies every glyph by its exact outline in a trusted font and accepts a text only if the shaping
engine lays it out into the same glyphs - so for every word it proves SAME, DIFFERENT (with the visible text) or UNKNOWN,
and lists text that machines read but humans cannot see. No OCR, no guessing; works for Indian scripts too.

- Two fresh pre-registered blind tests (120 new documents each, English, Hindi, Bengali, Tamil): **99.98%** of words
  exact; the latest with **0 harmful errors**; all hidden text found, no false alarms
- Same files with Tesseract OCR: 91.8-98.2% of words; an OCR-based lie detector caught 79-95% of lies with up to 4% false alarms
- Honest limits: synthetic documents so far; needs the trusted fonts; real-world PDFs are the next test

[README](https://github.com/novain-shibu/seenread) · [v0.1.0 release](https://github.com/novain-shibu/seenread/releases/tag/v0.1.0) · AGPL-3.0 or commercial licence ([open for talks](https://github.com/novain-shibu/seenread/blob/main/COMMERCIAL.md))

<a name="finepdfs-audit"></a>
### FinePDFs audit: invented and broken text in an AI training dataset, government PDFs without correct text, and Word's PDF export &nbsp;[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23432200.svg)](https://doi.org/10.5281/zenodo.23432200)

FinePDFs (Hugging Face) is a 3-trillion-token dataset of text extracted from PDFs, used to train AI models. I audited its
complex-script languages and traced every problem back to the original PDFs.

- **Fabricated text:** most documents in several scripts went through an AI OCR model; its stored text almost never
  matched an independent OCR of the same PDF in 7 scripts (Malayalam 18/18, Telugu 17/17, Kannada 17/17, Tamil 6/7,
  Sinhala 15/15, Odia 19/19, Punjabi 14/16), while Hindi, Bengali, Marathi, Nepali and Gujarati were fine. Checked by
  hand: a ministry pension scheme became "a platform for farmers in Kerala"; roughly 4.8 billion tokens if the sampled
  rates hold (rough extrapolation), and no signal in the dataset flags it
- **Small languages get the garbage:** the Southern Balochi (99%), Kashmiri (94%), Gilaki (73%) and Mesopotamian Arabic (69%)
  configs are mostly broken Persian or standard Arabic text, about 6.1 billion tokens (rough); big languages stay clean
- **Government PDFs:** 93.8% of 791 randomly sampled Indian government (gov.in / nic.in) PDFs in nine regional languages
  have no correct text (73% had no usable text layer, mostly scans; 21% garbled text layers, mostly Word-made in a checked subset;
  6% clean), checked on 29 originals; no improvement between 2017-19 and 2023-25. A sample of what Common Crawl and FinePDFs kept
- **A cause, a Word bug:** with the same text and the same font, Microsoft Edge's PDF loses no letters (a few doubled vowel signs) while Word 365's
  Save as PDF loses and garbles many (letter F1 34-90%: Khmer, Myanmar, Malayalam, Hindi); Word's export keeps 7-65% of words readable in 10 Indian
  scripts and also garbles Divehi and Lao (about 15 scripts, over a billion speakers), while Arabic, Hebrew and Thai survive
- **Recovery without OCR:** reversing font shaping recovered **99.1%** of words on held-out PDFs in 10 scripts, vs 86-91%
  for free OCR and 61% for the AI OCR the dataset used (research version; now released as [Unshape](https://github.com/novain-shibu/unshape), a few seconds per page on one CPU core)

[Report](https://github.com/novain-shibu/finepdfs-audit/blob/main/REPORT.md) · [Code & results](https://github.com/novain-shibu/finepdfs-audit) · [Reported to Hugging Face](https://github.com/huggingface/finepdfs/issues/4) · report CC BY 4.0, code Apache-2.0

<a name="unshape"></a>
### Unshape: exact text from broken Indian-language PDFs, without OCR &nbsp;[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23418945.svg)](https://doi.org/10.5281/zenodo.23418945)

Copy-paste, search, screen readers and AI pipelines often get garbled text from Hindi and other Indian-language PDFs. Unshape
recovers the text from the glyphs themselves: it searches for the text whose font shaping reproduces the PDF's glyphs and
keeps a stretch only if it re-renders exactly. When the embedded font has lost its shaping tables (Word exports), it
matches the glyphs to the original font the user supplies.

- Word 365 PDFs in 10 Indian scripts + Sinhala: **99.4% exact words** (first frozen run, 7123/7164)
- Blind real PDFs from one publisher: **98.5%** (PyMuPDF 72.4%); every blind test is listed in the README
- Honest limits: needs the original font; Mangal (the most common Word Hindi font) not tested yet; no scans

[README](https://github.com/novain-shibu/unshape) · [v0.2.0 release](https://github.com/novain-shibu/unshape/releases/tag/v0.2.0) · AGPL-3.0 or commercial licence ([open for talks](https://github.com/novain-shibu/unshape/blob/main/COMMERCIAL.md))

<a name="resync-apply"></a>
### Resync-and-Anchor Copy Drafting for Lossless Code Fast-Apply &nbsp;[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23393220.svg)](https://doi.org/10.5281/zenodo.23393220)

AI code editors apply an edit by having a model rewrite the whole file, most of it copied. I built a lossless
"copy-guesser" that lets the model confirm long copied spans in one pass, and compared it with Snowflake's
SuffixDecoding, the strongest open method (built into vLLM).

- **1.29× fewer model passes** than SuffixDecoding on edits never used for tuning (95% CI 1.22–1.37), output identical to normal decoding
- **24–37× faster than normal decoding** on a free T4 GPU; against SuffixDecoding the wall-clock was a tie on that GPU, and the report explains why
- A laptop simulator that predicted the GPU pass counts **exactly** in three runs, plus 7 ideas that failed, with numbers

[Report (PDF)](https://github.com/novain-shibu/resync-apply/releases/download/v1.0.0/REPORT.pdf) · [Code](https://github.com/novain-shibu/resync-apply) · Apache-2.0

<a name="spectrace"></a>
### spectrace: predicting speculative-decoding gains without running the drafter &nbsp;[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23413796.svg)](https://doi.org/10.5281/zenodo.23413796)

Speculative decoding speeds up a large model by letting a small "drafter" guess ahead; choosing the right drafter usually
means running each one. I tested an offline way to predict a drafter's gain from outputs the model has already produced
(the estimator was published first by Zhao & Cai, arXiv:2610.10411; I derived it independently and credit them), checked it
against real engines, and applied it to choosing drafters during RL training.

- Predictions matched real engines: **27.36 vs 27.28** passes (Hugging Face, sampling); **931 = 931** (greedy); the engine's own probabilities matched within **0.02–0.31%**
- Picked the best of 3 drafters correctly for 2 models, within **0.2–1.2%**, about **140× cheaper** than measuring (measured 144×); held up across a real RL-trained model
- An exact replay check for vLLM's speculative-decoding counters independently caught two known metric bugs, one of them ~10× off

[Report](https://github.com/novain-shibu/spectrace/blob/main/REPORT.md) · [Code](https://github.com/novain-shibu/spectrace) · Apache-2.0

<a name="prompt-injection"></a>
### Prompt-injection defenses for AI agents

[**prompt-injection-relocation**](https://github.com/novain-shibu/prompt-injection-relocation): a stress test that moves logged prompt-injection attacks into other channels an agent reads (email, files, calendar, web).

- A defense gate that stopped **88%** of attacks where they were originally placed dropped to **28–62%** in 10 of 16 channel types
- A redesigned gate held **91–100%** in 14 of 16 (weak spot: delegated sources)
- Offline replay, reproducible in about 5 minutes

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
