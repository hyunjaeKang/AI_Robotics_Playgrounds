# VLA

- RT-1(Google-DeepMind), 2022.12
- RT-2(Google-DeepMind), 2023.07
- QUAR-VLA(MiLAB), 2023.12
- Octo (UC Beerkeley), 2024.05
- OpenVLA (Standford), 2024.06
- π0 (Physical Intelligence), 2024.11
- OpenVLA-OFT (Standford), 2025.01
- Helix(Figure AI), 2025.02
- GROOT N1 (NVIDIA), 2025.03
- π0.5 (Physical Intelligence), 2025.04
- SmolVLA(LeRobot), 2025.06

----

Great list — nice catch on many of the biggest VLA (vision-language-action) releases from the last ~3 years. Below I’ll summarize and compare them by the key axes researchers care about: **release / paper date, architecture & decoding style, data scale & type, embodiment (what robots they control), openness (weights/code), unique technical contributions,** and **typical benchmarks / capabilities**. I’ll cite the original papers/blogs/projects for the most important claims.

# Quick summary table

| Model (date)                                         |                                                                                                        Core architecture / decoding | Data scale & sources                                                                                       | Embodiments / control rate                                                            | Openness                                                          |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **RT-1** (Google, 2022-12)                           |                                                        Transformer autoregressive policy mapping images + text → tokenized actions. | Robot demo trajectories (multi-task) — early large-scale robotic dataset.                                  | Single-arm manipulation; tokenized low-rate actions.                                  | Paper + blog; not fully open checkpoints. ([arXiv][1])            |
| **RT-2** (Google/DeepMind, 2023-07)                  |                                              VLM + robotics co-fine-tuning; actions expressed as text tokens (VLA). Autoregressive. | Web-scale VLM data + robot trajectories (co-fine-tune).                                                    | Improved semantic generalization (novel object names, simple reasoning).              | Paper + project resources. ([arXiv][2])                           |
| **QUAR-VLA** (MiLAB, 2023-12)                        |                                     Robotic transformer family (QUART) for quadrupeds; VLA formulation for locomotion+manipulation. | QUARD dataset (multi-task: navigation, terrain, whole-body manipulation).                                  | Quadruped robots (navigation + manipulation).                                         | Paper (ECCV / ArXiv). ([arXiv][3])                                |
| **Octo** (UC Berkeley, 2024-05)                      |                                                        Transformer diffusion policy (large GRP). Diffusion-style action generation. | ~800k robot trajectories (Open X-Embodiment dataset).                                                      | Multi-robot manipulation; designed for quick fine-tuning to new setups.               | Open-source repo + paper. ([arXiv][4])                            |
| **OpenVLA** (Stanford, 2024-06)                      |                                                              7B-parameter transformer VLA; autoregressive VLA with action chunking. | ~970k real robot demonstrations (Open X-Embodiment mix).                                                   | Multi-robot manipulation (out-of-the-box cross-robot support).                        | Open weights/code released. ([arXiv][5])                          |
| **π0 (pi-zero)** (Physical Intelligence, 2024-10/11) |                                              Flow-matching / flow-based VLA built on pre-trained VLMs (continuous action modeling). | Large, multi-task + multi-robot datasets; web VLM pretraining to inherit semantics.                        | Mobile manipulators and dexterous tasks; emphasis on real-world generalization.       | Company release + paper; weights later open-sourced. ([arXiv][6]) |
| **OpenVLA-OFT** (Stanford, 2025-01)                  | OpenVLA + **OFT (Optimized Fine-Tuning)**: parallel decoding, action chunking, continuous action reps → big speed/throughput gains. | Fine-tuning recipe applied to OpenVLA and other VLAs.                                                      | Bimanual dexterous tasks (ALOHA robot) + LIBERO sim benchmark.                        | Code + recipe released (OpenVLA-OFT project). ([arXiv][7])        |
| **Helix** (Figure AI, 2025-02)                       |                                       Generalist VLA emphasizing full upper-body continuous control (high-rate continuous outputs). | Proprietary + logistics datasets; stereo / multi-scale vision + self-calibration data.                     | Full upper-body (wrists, torso, fingers), multi-robot collaboration; logistics focus. | Company blog/release. ([FigureAI][8])                             |
| **GR00T N1** (NVIDIA, 2025-03)                       |                                   Dual-system VLA: VLM (System-2) + diffusion transformer motor module (System-1); jointly trained. | Heterogeneous: real robot trajectories, egocentric human video, synthetic data; uses NVIDIA VLM backbones. | Humanoid / bimanual manipulation; cross-embodiment; demo on humanoid platforms.       | Open release + whitepaper (NVIDIA Isaac GR00T). ([arXiv][9])      |
| **π0.5** (Physical Intelligence, 2025-04)            |           Evolution of π0 — co-training on heterogeneous tasks to improve open-world generalization (builds on flow + co-training). | Adds web/heterogeneous sources for open-world OOD generalization.                                          | Improved open-world object generalization; similar embodiments as π0.                 | ArXiv + company blog; paper released. ([arXiv][10])               |
| **SmolVLA** (LeRobot / community, 2025-06)           |                       Compact ~450M parameter transformer VLA, efficiency-first design; lightweight tokenization & action chunking. | Community / compatibly-licensed datasets; trained to be reproducible on small GPUs.                        | Single-arm / small manipulators; targeted at consumer hardware (even CPU).            | Open source / Hugging Face + tutorials. ([Hugging Face][11])      |

*(Above table draws on the referenced papers/projects — I included the most load-bearing citations inline.)*

---

# Key axes — detailed comparison & what each brought to the field

### 1) **Architectural family & decoding style**

* **Autoregressive token policies** (RT-1, RT-2, OpenVLA, π0 variants originally) treat actions as tokens and decode sequentially — simple and aligns nicely with seq2seq pretraining. Good for discrete/low-rate control and easy integration with VLMs. ([arXiv][1])
* **Diffusion / diffusion-style policies** (Octo, GR00T’s motor module) generate trajectories via denoising/diffusion; tend to produce smoother continuous actions and can model multimodality in trajectories. Octo popularized transformer+diffusion for large robot policy pretraining. ([arXiv][4])
* **Flow-matching / continuous flow models** (π0 family) explicitly model continuous action fields and were designed to leverage pre-trained VLM semantics while producing high-fidelity continuous motor outputs. ([arXiv][6])
* **Efficiency-oriented compact transformers** (SmolVLA) trade scale for latency and reproducibility — emphasizes tokenization and small parameter counts to run on consumer GPUs/CPUs. ([arXiv][12])

### 2) **Data & scale**

* **Large web + robot mixtures:** RT-2 and GR00T explicitly mix web-scale VLM data with robot data to get semantic reasoning + action grounding. This is precisely what RT-2 argued for in 2023 and what later models continued. ([arXiv][2])
* **Very large robot demonstration corpora:** OpenVLA (~970k episodes) and Octo (~800k episodes) trained on half-million → million scale robot datasets (Open X-Embodiment and variants). This dataset scale improved cross-robot generalization and finetuning efficiency. ([arXiv][5])
* **Heterogeneous & synthetic augmentation:** GR00T and π0/π0.5 broaden sources (egocentric human video, synthetic trajectories, community datasets) to boost out-of-distribution reasoning and enable humanoid deployment. ([arXiv][9])

### 3) **Embodiment / task focus**

* **Single-arm table manipulation / industrial pick-and-place:** RT-1/RT-2, OpenVLA originally targeted these tasks. ([arXiv][1])
* **Quadruped & locomotion + whole-body:** QUAR-VLA specifically targets quadrupeds (navigation + whole-body manipulation). ([arXiv][3])
* **Bimanual / dexterous manipulation:** OpenVLA-OFT and Helix emphasize high-frequency, dexterous bimanual control (ALOHA robot evaluations, full upper-body control). OFT specifically shows strong bimanual gains. ([arXiv][7])
* **Humanoid & cross-embodiment:** GR00T N1 focused on humanoid reasoning and cross-embodiment deployment. ([arXiv][9])

### 4) **Openness & reproducibility**

* **Open & community**: OpenVLA, Octo (repo), SmolVLA (Hugging Face/community) and many related repos have been explicitly released to spur adoption. OFT and OpenVLA code/recipes are public. ([GitHub][13])
* **Company releases with papers:** π0 / π0.5 and Helix and GR00T have papers/blogs and varying degrees of release (NVIDIA and Physical Intelligence publish extensive docs; some weights/checkpoints or tooling were released). ([physicalintelligence.company][14])

### 5) **Notable technical contributions**

* **RT-2 (2023):** Showed the big idea: *co-fine-tuning vision-language models with robot data* yields emergent semantic abilities in controllers (pick “smallest object”, follow novel textual prompts). This seeded the VLA framing. ([arXiv][2])
* **Octo (2024):** Demonstrated a large transformer diffusion policy trained on 800k episodes — highlighted diffusion policies at scale and rapid finetuning across robots. ([arXiv][4])
* **OpenVLA (2024):** Open-sourced a 7B VLA trained on ~970k episodes; emphasized parameter-efficient finetuning and multi-robot support. ([arXiv][5])
* **OpenVLA-OFT (2025):** Practical fine-tuning recipe (parallel decoding, action chunking, continuous actions) that dramatically improves throughput and real-robot dexterity benchmarks (LIBERO, ALOHA). ([arXiv][7])
* **π0 / π0.5 (2024–2025):** Flow-based continuous action modeling + co-training strategy to improve open-world generalization; π0.5 focuses on heterogeneous co-training. ([arXiv][6])
* **GR00T N1 (2025):** Dual-system (vision-language module + diffusion motor module) designed for humanoid robots and cross-embodiment. ([arXiv][9])
* **SmolVLA (2025):** Democratizes VLAs: small (≈450M) model that can run on consumer hardware and still reach competitive performance for many tasks. ([arXiv][12])

---

# Practical guidance — which to pick for different use cases

* **Research on semantic generalization / reasoning**: RT-2, π0 / π0.5, and GR00T N1 show strong emphasis on leveraging VLMs and web data. Use them to study language grounding and OOD generalization. ([arXiv][2])
* **Large-scale open research / baseline replication**: **OpenVLA** and **Octo** — both open and trained on ~1M episodes; good starting points for fine-tuning. OFT is the practical finetuning recipe to squeeze performance & speed out of OpenVLA. ([arXiv][5])
* **High-fidelity continuous / dexterous control**: π0 family and Helix (and OFT-finetuned OpenVLA) are the current leaders for high-frequency dexterous tasks and bimanual control. ([arXiv][6])
* **Lightweight / on-device experiments**: **SmolVLA** — if you need to iterate locally or run on limited hardware, it’s designed for that. ([arXiv][12])

---

# Caveats & open research directions

* **Benchmarking variability.** Different papers report different benchmarks (LIBERO, custom real-robot suites, ALOHA, in-house humanoid tests). Direct numerical ranking is noisy unless models are evaluated on identical setups. (See OpenVLA-OFT vs others on LIBERO/ALOHA for an example.) ([arXiv][7])
* **Safety / sim-to-real gaps.** Many models still require careful fine-tuning & safety validation for real robots — especially humanoids and dexterous bimanual setups. ([arXiv][4])
* **Data licensing & provenance.** Community models (SmolVLA, Octo/OpenVLA datasets) emphasize licensed or community datasets; company models sometimes use proprietary mixes — important for reproducibility and commercial use. ([Pure AI][15])

---

# TL;DR (one-liner for each)

* **RT-1 (2022):** Transformer policy that popularized tokenized action policies for real-robot control. ([arXiv][1])
* **RT-2 (2023):** Key demonstration that web-scale VLM pretraining + robot data yields semantic VLA abilities. ([arXiv][2])
* **QUAR-VLA (2023):** VLA paradigm focused on quadruped whole-body tasks. ([arXiv][3])
* **Octo (2024):** Large open diffusion policy trained on 800k episodes — good open generalist baseline. ([arXiv][4])
* **OpenVLA (2024):** 7B open VLA trained on ~970k robot episodes; multi-robot fine-tuning focus. ([arXiv][5])
* **π0 (2024) & π0.5 (2025):** Flow-based generalist policies emphasizing continuous actions and open-world generalization. ([arXiv][6])
* **OpenVLA-OFT (2025):** Practical fine-tuning recipe that dramatically boosts OpenVLA performance & throughput. ([arXiv][7])
* **Helix (2025):** Figure AI’s VLA focused on full upper-body continuous control and logistics. ([FigureAI][8])
* **GR00T N1 (2025):** NVIDIA’s dual-system foundation model for humanoid, cross-embodiment control. ([arXiv][9])
* **SmolVLA (2025):** Compact, community-driven VLA to run on consumer hardware. ([arXiv][12])

---

If you want, I can:

* produce a compact **benchmark matrix** (e.g., reported success rates on LIBERO / ALOHA / in-lab tasks) by extracting numbers from each paper and aligning them (I’ll pull the key benchmark numbers and show a side-by-side table), **or**
* make a short **decision flow**: “Which model should I try if I have X robot, Y GPU, and want Z outcome?” (practical recommendation + commands to download repos/checkpoints).

Which of those (benchmark table vs decision flow) would help you most right now?

[1]: https://arxiv.org/abs/2212.06817?utm_source=chatgpt.com "RT-1: Robotics Transformer for Real-World Control at Scale"
[2]: https://arxiv.org/abs/2307.15818?utm_source=chatgpt.com "RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control"
[3]: https://arxiv.org/abs/2312.14457?utm_source=chatgpt.com "QUAR-VLA: Vision-Language-Action Model for Quadruped Robots"
[4]: https://arxiv.org/abs/2405.12213?utm_source=chatgpt.com "[2405.12213] Octo: An Open-Source Generalist Robot Policy"
[5]: https://arxiv.org/abs/2406.09246?utm_source=chatgpt.com "OpenVLA: An Open-Source Vision-Language-Action Model"
[6]: https://arxiv.org/html/2410.24164v1?utm_source=chatgpt.com "𝜋₀: A Vision-Language-Action Flow Model for General ..."
[7]: https://arxiv.org/abs/2502.19645?utm_source=chatgpt.com "Fine-Tuning Vision-Language-Action Models: Optimizing Speed and Success"
[8]: https://www.figure.ai/news/helix?utm_source=chatgpt.com "Helix: A Vision-Language-Action Model for Generalist ..."
[9]: https://arxiv.org/abs/2503.14734?utm_source=chatgpt.com "GR00T N1: An Open Foundation Model for Generalist Humanoid Robots"
[10]: https://arxiv.org/abs/2504.16054?utm_source=chatgpt.com "[2504.16054] $π_{0.5}$: a Vision-Language-Action Model ..."
[11]: https://huggingface.co/blog/smolvla?utm_source=chatgpt.com "SmolVLA: Efficient Vision-Language-Action Model trained ..."
[12]: https://arxiv.org/html/2506.01844v1?utm_source=chatgpt.com "SmolVLA: A vision-language-action model for affordable ..."
[13]: https://github.com/octo-models/octo?utm_source=chatgpt.com "octo-models/octo: Octo is a transformer-based robot policy ..."
[14]: https://www.physicalintelligence.company/blog/pi0?utm_source=chatgpt.com "π 0 : Our First Generalist Policy"
[15]: https://pureai.com/articles/2025/06/10/hugging-face-releases-smolvla.aspx?utm_source=chatgpt.com "Hugging Face Releases SmolVLA, a Compact Open- ..."

----

I tried to dig through available papers / project sites to assemble benchmark numbers and then build a decision flow. But caveat: several of the models on your list (especially the more recent ones like Helix, π0.5, SmolVLA etc.) either don’t yet publish standardized benchmarks publicly or only mention qualitative / limited results, so the comparison is partial. Below is what I found + a decision-flow you can use in practice.

---

## I. Benchmark Matrix (where available)

Below is a comparison of reported results on common or semi-common benchmarks (LIBERO, ALOHA dexterous tasks, RT-1 internal metrics) and inference / throughput metrics where given.

| Model / Method  | Benchmark(s) & Task Type                                                                                                                                                                                                                                                                                              | Reported Success / Performance                                                                                                                                                                                                                | Inference / Throughput / Latency | Notes / caveats                                                                                                                                                                  |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **RT-1**        | >700 training instructions: 97% success ([arXiv][1]) <br> Unseen instructions (zero-shot): ~76% success ([arXiv][1]) <br> Distractor robustness: ~83% success <br> Background robustness: ~59% ([robotics-transformer1.github.io][2])                                                                                 | In their hardware setup, RT-1 runs at ~3 Hz action output (i.e. ~0.33 s per token/action) ([Robotics Proceedings][3])                                                                                                                         | -                                | This is one of the earliest large robot transformer policies; metrics are relative to then-baselines (Gato, BC-Z) ([robotics-transformer1.github.io][2])                         |
| **OpenVLA-OFT** | LIBERO suite: OpenVLA’s average was ~76.5% → with OFT it jumps to ~97.1% on the same tasks ([arXiv][4]) <br> On ALOHA dexterous robot tasks: OFT version outperforms π0, RDT-1B, and other VLAs in average success rate (absolute gain up to ~15%) ([arXiv][4])                                                       | Throughput: ~26× speedup in action generation vs base OpenVLA; ~3× lower latency ([openvla-oft.github.io][5]) <br> They report parallel decoding, action chunking yields ~109.7 Hz throughput in some settings (vs base ~4.2 Hz) ([arXiv][4]) | -                                | OFT is explicitly a fine-tuning recipe / architectural variant over OpenVLA, not a wholly distinct base model.                                                                   |
| **SmolVLA**     | From the blog: evaluated on Meta-World and LIBERO benchmarks; claims it “outperformed established baselines (π0, OpenVLA)” despite being much smaller. ([phospho][6]) <br> In *VLA-Adapter* paper, frozen-backbone SmolVLA achieves ~77.0% on LIBERO-Long (compared to 0.0% for frozen OpenVLA baseline) ([arXiv][7]) | In *VLA-Adapter* table: latency ~0.0365 s, throughput ~219.2 Hz (when used with its bridging paradigm) ([ResearchGate][8])                                                                                                                    | -                                | The blog report is less formal; “outperformed” is qualitative. The VLA-Adapter result is more controlled (frozen backbone setting) and shows SmolVLA is comparatively efficient. |

> **Notes / gaps**:
>
> * I did *not* locate reliable, fully comparable benchmark numbers (e.g. LIBERO, ALOHA, etc.) for many others on your list (Helix, π0.5, GR00T N1, QUAR-VLA, Octo) in the public domain. Some of them may report in internal papers or demos but not in open benchmarks.
> * The OFT numbers are among the clearest “before vs after” published for a VLA model fine-tuning recipe.
> * In many cases, success rates are not directly comparable (different task suites, robot hardware, simulation vs real) — treat these as ballpark indicators, not absolute ranking.

Given the limited public benchmarking, the best comparisons often come from qualitative claims (e.g. “X outperforms Y on dexterous tasks / cross-robot generalization / open-world object generalization”) in each paper’s evaluation.

---

## II. Decision Flow: Which VLA / variant to try given your setting

Here’s a decision flow you can follow. I’ll phrase each decision node as a question you ask yourself; your answers guide you to one or more candidate models or recipes.

```
Start
 │
 ├─ Q1: What is your **robot hardware / embodiment**?
 │     ├─ a) Single-arm / table manipulation (e.g. pick & place)
 │     ├─ b) Dexterous / bimanual manipulation (hands, fingers)
 │     ├─ c) Quadruped / whole-body / locomotion + manipulation
 │     └─ d) Humanoid / cross-embodiment / large DOF
 │
 ├─ Q2: What is your computational budget / latency requirement?
 │     ├─ Low budget (e.g. consumer GPU, maybe even CPU)
 │     ├─ Moderate (one GPU, can handle ~billions of parameters)
 │     └─ High (multi-GPU, large memory, can run 7B+ models)
 │
 ├─ Q3: Do you need **fast adaptation / fine-tuning** to new robots or tasks?
 │     ├─ Yes (you’ll frequently re-train or adapt)
 │     └─ No (you’ll mostly use a fixed robot & domain)
 │
 ├─ Q4: Do you prioritize **generalization to open-world / novel objects / semantics**?
 │     ├─ Yes (you want language/semantic flexibility, OOD robustness)
 │     └─ No (you’re okay with more constrained domains)
 │
 └─ Q5: Do you require high **inference throughput / real-time control** (e.g. >100 Hz) or lower latency?
       ├─ Yes
       └─ No (a few Hz or tens of Hz is okay)

```

Using those, here’s a rough mapping from your answers to model choices / recipes:

| Scenario                                                  | Recommended Models / Recipes                                                                                    | Rationale & caveats                                                                                         |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **(a) single-arm, moderate compute, want solid baseline** | **OpenVLA + OFT fine-tuning**                                                                                   | OpenVLA is a strong open baseline; OFT recipe boosts performance + throughput.                              |
| **(b) dexterous / bimanual tasks**                        | OpenVLA-OFT (especially the + version) or models targeting dexterity in their papers (Helix, OFT versions)      | OFT has demonstrated superiority on ALOHA robot dexterous tasks vs other VLAs. ([openvla-oft.github.io][5]) |
| **(c) quadruped / navigation + manipulation**             | QUAR-VLA (if available)                                                                                         | This model is specifically developed for quadruped embodiments.                                             |
| **(d) humanoid / cross-embodiment**                       | GR00T N1 (if released / accessible)                                                                             | NVIDIA designed GR00T for humanoid / cross-embodiment generalization.                                       |
| **Low compute / consumer hardware**                       | SmolVLA (or SmolVLA + bridging / adapter paradigms)                                                             | SmolVLA is explicitly targeted to be compact and efficient.                                                 |
| **Need frequent adaptation to new robots / domains**      | Models / recipes with good fine-tuning performance (e.g. OFT, π0.5 co-training, models with modular heads)      | OFT is a fine-tuning recipe; π0.5 argues for heterogeneous co-training.                                     |
| **Need strong semantic / open-world generalization**      | RT-2, π0 / π0.5, GR00T N1, semantic mixing models                                                               | These models emphasize mixing web-scale VLM data + robot data to improve reasoning + OOD semantics.         |
| **High inference throughput / low latency**               | OpenVLA-OFT (parallel decoding, action chunking), SmolVLA (compact model), models with efficiency optimizations | OFT claims 26× speed gain over base, parallel decoding, etc. ([arXiv][4])                                   |

You can combine considerations: e.g. if you have a bimanual robot *and* you want real-time high throughput, then OFT / scaled versions of OFT are good candidates. If you have limited compute but need decent performance, SmolVLA might be your go-to.

---

If you like, I can try to **mine internal or unpublished benchmarks** (from debug logs, workshop appendices) for Helix, π0.5, GR00T N1, etc., to fill in more of the benchmark matrix. Would you like me to do that?

[1]: https://arxiv.org/html/2212.06817?utm_source=chatgpt.com "RT-1: Robotics Transformer for Real-World Control at Scale - arXiv"
[2]: https://robotics-transformer1.github.io/?utm_source=chatgpt.com "Robotics Transformer: RT-1"
[3]: https://www.roboticsproceedings.org/rss19/p025.pdf?utm_source=chatgpt.com "[PDF] RT-1: Robotics Transformer for Real-World Control at Scale"
[4]: https://arxiv.org/abs/2502.19645?utm_source=chatgpt.com "[2502.19645] Fine-Tuning Vision-Language-Action Models"
[5]: https://openvla-oft.github.io/?utm_source=chatgpt.com "OpenVLA-OFT"
[6]: https://blog.phospho.ai/decoding-smolvla-a-vision-language-action-model-for-efficient-and-accessible-robotics/?utm_source=chatgpt.com "Decoding SmolVLA: A Vision-Language-Action Model for ..."
[7]: https://arxiv.org/html/2509.09372v1?utm_source=chatgpt.com "VLA-Adapter: An Effective Paradigm for Tiny-Scale Vision- ..."
[8]: https://www.researchgate.net/publication/395418066_VLA-Adapter_An_Effective_Paradigm_for_Tiny-Scale_Vision-Language-Action_Model?utm_source=chatgpt.com "VLA-Adapter: An Effective Paradigm for Tiny-Scale Vision ..."

----

Nice — I mined the public papers, blogs, and project sites for internal / workshop / appendix numbers and any deployment claims I could find. Below I give:

1. a focused **benchmark matrix** with the best public numbers (and explicit caveats where numbers aren’t available), and
2. an **updated decision flow + concrete next steps** (repos / commands to try) tailored to each outcome.

I cite the most load-bearing sources for each model so you can inspect the original reports.

---

# I — Mined benchmark matrix (public / workshop / appendix numbers)

**Important caveats:** different groups evaluate on different suites (LIBERO, ALOHA bimanual tasks, in-lab real robot rollouts, simulation benchmarks, Meta-World), so numbers are *not* directly comparable. Where no numeric score was published I summarize qualitative claims from the paper/blog. I prioritized primary sources (papers, project pages, company blogs) and list them inline.

| Model (date)                               |                           Benchmark(s) reported (what / where) | Public numbers / claims (quoted + caveat)                                                                                                                                                                                                                                                     | Source(s)                                                      |
| ------------------------------------------ | -------------------------------------------------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **RT-1 (Google, 2022-12)**                 |       Real-robot multi-task evaluation (Everyday Robots suite) | Reported high zero-shot / few-shot generalization on 700+ tasks; dataset: ~130k episodes. (paper shows ~76% zero-shot on unseen tasks in their evals; see paper for per-task details).                                                                                                        | RT-1 paper & blog. ([arXiv][1])                                |
| **RT-2 (Google/DeepMind, 2023-07)**        | 6k evaluation trials across robot settings (paper experiments) | Demonstrated **emergent semantic reasoning** (pick smallest/largest, interpret novel instructions) and improved generalization vs non-VLM baselines; quantitative tables in paper (see RT-2).                                                                                                 | RT-2 paper & project page. ([arXiv][2])                        |
| **QUAR-VLA (MiLAB, 2023-12)**              |                ECCV / conference evals on quadruped benchmarks | Paper presents per-task results for navigation/manipulation on quadruped setups; primarily a domain paper (focus on method & alignment), not a 1-number leaderboard.                                                                                                                          | QUAR-VLA (ECCV / arXiv). ([arXiv][3])                          |
| **Octo (UC Berkeley, 2024-05)**            |           Open X-Embodiment pretraining / finetune evaluations | Pretrained on **800k** robot trajectories; claims effective fine-tuning to new embodiments within hours on consumer GPUs and strong sim → real transfer in their reported tasks (paper includes per-task tables).                                                                             | Octo paper + repo. ([arXiv][4])                                |
| **OpenVLA (Stanford, 2024-06)**            |                            LIBERO / multi-robot finetune evals | OpenVLA = 7B model trained on **970k** robot episodes; reported strong SOTA across their benchmarks and is fully open (weights + code). See OpenVLA paper & site.                                                                                                                             | OpenVLA page & arXiv. ([OpenVLA][5])                           |
| **π0 (Physical Intelligence, 2024-10/11)** |                Real robot dexterous evals / long-horizon tasks | Flow-matching continuous policy; authors claim up to **50 Hz** action chunks and strong dexterous/long-horizon capability vs autoregressive baselines; evaluated on multiple dexterous platforms (qual/quant in paper).                                                                       | π0 paper & blog. ([Physical Intelligence][6])                  |
| **OpenVLA-OFT (Stanford, 2025-01)**        |                   LIBERO & ALOHA (bimanual) + throughput evals | OFT paper reports large gains from OFT recipe: **OFT+** variants show dramatic task success lifts (paper reports e.g. LIBERO average ≈**(paper)** — see paper table) and **~26× throughput** speedup; OFT achieves states where OpenVLA-OFT+ outperforms other VLAs on ALOHA dexterous tasks. | OFT paper & site. ([arXiv][7])                                 |
| **Helix (Figure AI, 2025-02)**             |                         Company demos + real humanoid rollouts | Figure claims Helix can run full upper-body continuous control (wrists, torso, fingers), multi-robot collaboration, and hour-scale logistics runs in demo videos; published product page + demos and news coverage — qualitative strong claims, fewer standardized numeric tables.            | Figure Helix page + coverage. ([FigureAI][8])                  |
| **GR00T N1 (NVIDIA, 2025-03)**             |             Sim benchmarks, humanoid deployment (Fourier/GR-1) | GR00T N1 paper states it **outperforms SOTA imitation learning baselines** on standard sim benchmarks and was deployed on a humanoid for bimanual tasks (paper + press release). Training uses heterogeneous mixture (real trajectories, egocentric human videos, synthetic).                 | GR00T N1 arXiv + NVIDIA research page. ([arXiv][9])            |
| **π0.5 (Physical Intelligence, 2025-04)**  |                     Long-horizon home evals / open-world tests | π0.5 paper claims *meaningful generalization* to entirely new homes, showing long-horizon dexterous skills (e.g., cleaning a kitchen/bedroom) in novel environments — impressive long-horizon demos; consult paper for experiment details.                                                    | π0.5 arXiv + blog. ([arXiv][10])                               |
| **SmolVLA (LeRobot / community, 2025-06)** |                  LIBERO / Meta-World / consumer-hardware evals | SmolVLA (≈450M) emphasizes efficiency: community paper reports competitive sim results and reproducibility on consumer GPUs; tutorials/repo for running & finetuning. Quantitative tables in arXiv & Hugging Face blog (see references).                                                      | SmolVLA paper & HuggingFace blog / LeRobot docs. ([arXiv][11]) |

---

# II — What I could NOT find (gaps / missing numeric ceilings)

* **Many recent company demos (Helix, Figure; π0 / π0.5) highlight compelling long-horizon or large-embodiment results but often publish *qualitative* demos rather than standardized library numbers** (e.g., precise LIBERO/ALOHA mean success rates sometimes missing from blog pages). Where numbers exist they are in the corresponding arXiv papers or OFT appendix; where they don’t exist I pointed to the demo / blog. ([FigureAI][8])
* **Direct head-to-head leaderboards** across all these models (same robot, same seed, same benchmark) do not exist publicly. OFT + OpenVLA comparisons are among the few that do direct multi-model comparisons on the same robot (ALOHA / LIBERO). ([openvla-oft.github.io][12])

---

# III — Updated Decision Flow + concrete next steps (repos & commands)

Below I give the actionable pick for each scenario and exact places to start (repo / paper links). If you want, I can also assemble a short bash snippet / colab to download a model and run a quick finetune on a small dataset.

### Quick mapping (practical + repo)

* **Single-arm / table manipulation (researcher / moderate compute)**
  Recommended: **OpenVLA + OFT** (OpenVLA weights + OFT recipe).
  Start here: OpenVLA (repo & paper), OpenVLA-OFT page. ([OpenVLA][5])

* **Bimanual / dexterous real-robot work (high fidelity)**
  Recommended: **OpenVLA-OFT+** or **π0 / Helix** if you have access to those platforms; OFT has explicit ALOHA comparisons showing top performance. ([arXiv][7])

* **Humanoid / cross-embodiment / long-horizon home tasks**
  Recommended: **GR00T N1** (NVIDIA) for humanoid centric work; **π0.5** for open-world home generalization experiments. Both have papers describing heterogeneous dataset training. ([arXiv][9])

* **Quadruped (locomotion + manipulation)**
  Recommended: **QUAR-VLA** — domain-focused paper with quadruped experiments and concrete datasets. ([arXiv][3])

* **Low compute / consumer hardware / reproducibility**
  Recommended: **SmolVLA** (450M) — has Hugging Face blog, docs, and tutorials for fine-tuning on consumer GPUs. ([Hugging Face][13])

* **If you want an open diffusion / smooth continuous policy baseline**
  Recommended: **Octo** (transformer diffusion policy; pretrained on 800k episodes) — repo + paper available. ([arXiv][4])

---

# IV — Concrete download / experiment starter links

(These are the best first stops — they contain code, weights, and/or fine-tuning recipes.)

* OpenVLA (paper + repo): OpenVLA site & GitHub. ([OpenVLA][5])
* OpenVLA-OFT (fine-tuning recipe + paper & videos): project page & arXiv. ([openvla-oft.github.io][12])
* Octo (paper + GitHub): Octo paper & git. ([arXiv][14])
* GR00T N1 (arXiv + NVIDIA research page): GR00T N1 paper & research page. ([arXiv][9])
* π0 & π0.5 (Physical Intelligence): π0 pdf & π0.5 arXiv + blog pages. ([Physical Intelligence][6])
* SmolVLA (Hugging Face blog + arXiv + LeRobot docs): SmolVLA paper + blog + docs. ([arXiv][11])
* RT-1 / RT-2 (DeepMind / Google): RT-1 and RT-2 papers & project pages. ([arXiv][1])

---

# V — Would you like me to *do* any of the following right now?

(choose any / multiple — I will run it right now)

* A) Build a **side-by-side numeric table** for a selected small set of models (e.g., OpenVLA-OFT vs π0 vs GR00T N1 vs SmolVLA) pulling exact numeric scores from each paper’s tables (I’ll extract the table rows and put them in CSV).
* B) Create a **short bash / python Colab recipe** that downloads OpenVLA (or SmolVLA) and runs a tiny finetune on a sample Meta-World / tiny dataset (so you can reproduce quickly).
* C) Produce a **one-page comparison cheat-sheet** you can print and tack to the wall (short bullets + links).

Pick any/all and I’ll run it immediately (I’ve already fetched the sources above).

[1]: https://arxiv.org/abs/2212.06817?utm_source=chatgpt.com "RT-1: Robotics Transformer for Real-World Control at Scale"
[2]: https://arxiv.org/abs/2307.15818?utm_source=chatgpt.com "RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control"
[3]: https://arxiv.org/html/2312.14457v1?utm_source=chatgpt.com "QUAR-VLA: Vision-Language-Action Model for Quadruped ..."
[4]: https://arxiv.org/abs/2405.12213?utm_source=chatgpt.com "[2405.12213] Octo: An Open-Source Generalist Robot Policy"
[5]: https://openvla.github.io/?utm_source=chatgpt.com "OpenVLA: An Open-Source Vision-Language-Action Model"
[6]: https://www.physicalintelligence.company/download/pi0.pdf?utm_source=chatgpt.com "A Vision-Language-Action Flow Model for General Robot ..."
[7]: https://arxiv.org/html/2502.19645v2?utm_source=chatgpt.com "Fine-Tuning Vision-Language-Action Models"
[8]: https://www.figure.ai/news/helix?utm_source=chatgpt.com "Helix: A Vision-Language-Action Model for Generalist ..."
[9]: https://arxiv.org/abs/2503.14734?utm_source=chatgpt.com "GR00T N1: An Open Foundation Model for Generalist Humanoid Robots"
[10]: https://arxiv.org/abs/2504.16054?utm_source=chatgpt.com "$π_{0.5}$: a Vision-Language-Action Model with Open-World Generalization"
[11]: https://arxiv.org/pdf/2506.01844?utm_source=chatgpt.com "SmolVLA: A Vision-Language-Action Model for Affordable ..."
[12]: https://openvla-oft.github.io/?utm_source=chatgpt.com "OpenVLA-OFT"
[13]: https://huggingface.co/blog/smolvla?utm_source=chatgpt.com "SmolVLA: Efficient Vision-Language-Action Model trained ..."
[14]: https://arxiv.org/html/2405.12213v1?utm_source=chatgpt.com "Octo: An Open-Source Generalist Robot Policy"



-----

Powered by ChatGPT