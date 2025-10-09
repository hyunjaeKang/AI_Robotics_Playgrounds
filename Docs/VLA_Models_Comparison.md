# Vision-Language-Action (VLA) Models Comparison (2022–2025)

## Overview
A comprehensive comparison of major Vision-Language-Action (VLA) models released between 2022 and 2025. Includes model descriptions, benchmark data, and decision flows for selecting the right model based on task, compute, and embodiment.

---

## Quick Summary Table

| Model (date) | Architecture / decoding | Data scale & sources | Embodiments | Openness |
|---|---|---|---|---|
| **RT-1** (Google, 2022-12) | Transformer, autoregressive | 130k robot demos | Single-arm manipulation | Paper only |
| **RT-2** (Google/DeepMind, 2023-07) | Vision-language + robot fine-tuning | Web + robot data | Single-arm | Paper + blog |
| **QUAR-VLA** (MiLAB, 2023-12) | Quadruped transformer | QUARD dataset | Quadrupeds | Paper (ECCV) |
| **Octo** (UC Berkeley, 2024-05) | Diffusion transformer | 800k trajectories | Multi-robot | Open repo |
| **OpenVLA** (Stanford, 2024-06) | 7B transformer VLA | 970k robot demos | Multi-robot | Fully open |
| **π0** (Physical Intelligence, 2024-10/11) | Flow-based continuous actions | Multi-task & VLM mix | Dex/bimanual | Paper + blog |
| **OpenVLA-OFT** (Stanford, 2025-01) | OpenVLA + Optimized Fine-Tuning | Same as OpenVLA | Bimanual | Open recipe |
| **Helix** (Figure AI, 2025-02) | Continuous high-rate control | Proprietary datasets | Full upper body | Private demos |
| **GR00T N1** (NVIDIA, 2025-03) | Dual-system (VLM + diffusion) | Multi-source | Humanoid | Open paper |
| **π0.5** (Physical Intelligence, 2025-04) | Flow + heterogeneous co-training | Multi-task | Long-horizon home | Paper + blog |
| **SmolVLA** (LeRobot, 2025-06) | Compact transformer (~450M) | Open community data | Single-arm | Fully open |

---

## Benchmark Matrix

| Model | Benchmark(s) | Reported Performance | Throughput / Latency | Notes |
|---|---|---|---|---|
| **RT-1** | Internal 700 tasks | ~76% unseen success | ~3 Hz | Early transformer baseline |
| **OpenVLA-OFT** | LIBERO / ALOHA | ~97% LIBERO avg (+20%) | ~26× faster vs base | Fine-tuning recipe |
| **SmolVLA** | LIBERO / Meta-World | ~77% LIBERO-Long (frozen) | ~219 Hz | Small model, efficient |
| **RT-2** | Internal multi-task | Strong semantic generalization | n/a | Introduced VLM+robot mix |
| **Octo** | Open X-Embodiment | Strong cross-robot transfer | n/a | Large diffusion baseline |
| **π0 / π0.5** | Real robot, dexterous | 50 Hz continuous | n/a | Long-horizon control |
| **GR00T N1** | Humanoid sim + real | >SOTA on internal | n/a | Cross-embodiment generalization |
| **Helix** | Logistics demos | Qualitative SOTA | n/a | Real upper-body control |

---

## Decision Flow: Which Model to Use

### Step 1 — Define your robot embodiment
- **Single-arm / tabletop:** OpenVLA (+OFT) or SmolVLA.
- **Bimanual / dexterous:** OpenVLA-OFT+, π0, or Helix.
- **Quadruped:** QUAR-VLA.
- **Humanoid / cross-embodiment:** GR00T N1 or π0.5.

### Step 2 — Compute budget
- **Low (consumer GPU/CPU):** SmolVLA.
- **Moderate (1 GPU):** OpenVLA + OFT.
- **High (multi-GPU):** π0/π0.5, GR00T N1.

### Step 3 — Training & Adaptation
- **Need frequent fine-tuning:** OpenVLA + OFT (optimized for speed).
- **Need plug-and-play generalization:** π0.5 or RT-2.

### Step 4 — Semantic Generalization
- **Language reasoning important:** RT-2, π0/π0.5, GR00T N1.

### Step 5 — Latency / Real-time needs
- **High-speed (>100 Hz):** OpenVLA-OFT or SmolVLA.
- **Moderate speed acceptable:** OpenVLA or Octo.

---

## Key Takeaways

- **RT-2 (2023)** proved that mixing web-scale VLM data and robot data produces semantic generalization.
- **Octo (2024)** introduced large diffusion-based policies for smooth continuous control.
- **OpenVLA (2024)** provided the first large open generalist robot transformer.
- **OpenVLA-OFT (2025)** made fine-tuning and inference drastically faster.
- **π0/π0.5 (2024–2025)** advanced continuous action modeling and long-horizon generalization.
- **GR00T N1 (2025)** combined dual VLM + diffusion for humanoids.
- **SmolVLA (2025)** democratized VLA research with small, efficient models.

---

## Recommended Resources (Links)
- [RT-1 Paper & Blog](https://robotics-transformer1.github.io)
- [RT-2 Project](https://robotics-transformer2.github.io)
- [Octo Paper & Repo](https://octo-model.github.io)
- [OpenVLA GitHub](https://openvla.github.io)
- [OpenVLA-OFT Project Page](https://openvla-oft.github.io)
- [π0 / π0.5 Papers](https://arxiv.org/abs/2410.XXXX)
- [GR00T N1 (NVIDIA)](https://research.nvidia.com/publication/2025-gr00t-n1)
- [SmolVLA (LeRobot)](https://huggingface.co/lerobot/smolvla)

---

## Notes & Limitations
- No fully standardized benchmark exists across all VLA models yet.
- Some company models (Helix, π0.5) share qualitative results only.
- OFT and SmolVLA currently represent the best open and efficient entry points for research.

---

## Suggested Next Steps
1. **Quick reproducible start:** Try OpenVLA or SmolVLA fine-tuning on small datasets.
2. **Benchmark expansion:** Evaluate OFT vs π0.5 on LIBERO or custom dexterous tasks.
3. **For humanoid research:** Experiment with GR00T N1’s diffusion-based control.
4. **Follow-up research goal:** Unified evaluation suite for all open VLAs (LIBERO++, 2025).


----

Powered by ChatGPT