# LeRobot

### Setup a conda environment

 ```
 conda create -y -n lerobot python=3.10
 conda activate lerobot
 conda install ffmpeg -c conda-forge

 pip install ipykernel ipywidgets
 pip install 'lerobot[all]'==0.3.3

 ```


---

### Policies in LeRobot

| Policy | Sub-Playground |
| ---- | ----- |
| Action Chunking with Transformers (ACT) | [ACT](./ACT/) |
| Diffusion Policy | [Diffusion_Policy](./Diffusion_Policy/) |
| Reinforcement Learning | [RL](./RL/) |
| ***π0*** | [Pi0](./Pi0/) |
| ***SmolVLA*** | [SmolVLA](./SmolVLA/) |

---

### References:

- ***Paper***:
    - [SmolVLA: A Vision-Language-Action Model for Affordable and Efficient Robotics](https://huggingface.co/papers/2506.01844)
    - [Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware](https://arxiv.org/abs/2304.13705)
    - [π_0: A Vision-Language-Action Flow Model for General Robot Control.](https://arxiv.org/abs/2410.24164)
    - [Diffusion Policy: Visuomotor Policy Learning via Action Diffusion](https://arxiv.org/pdf/2303.04137)

- ***Blog***
    - HuggingFace Blog: [https://huggingface.co/blog/smolvla](https://huggingface.co/blog/smolvla)
    - LearnOpenCV : https://learnopencv.com/smolvla-lerobot-vision-language-action-model/
    - https://huggingface.co/docs/lerobot/main/en/hilserl_sim
    - https://huggingface.co/lerobot/pi0
    - [LeRobot](https://huggingface.co/lerobot)
    - [Diffusion Policy - Project page](https://diffusion-policy.cs.columbia.edu/)
    - https://radekosmulski.com/diving-into-diffusion-policy-with-lerobot/
    - https://huggingface.co/lerobot/diffusion_pusht
    - https://huggingface.co/lerobot/act_aloha_sim_transfer_cube_human
    - https://tonyzhaozh.github.io/aloha/

- ***Github***:
    - https://github.com/huggingface/lerobot/blob/d602e8169cbad9e93a4a3b3ee1dd8b332af7ebf8/docs/source/il_sim.mdx
    - https://github.com/real-stanford/diffusion_policy
    - https://huggingface.co/docs/lerobot/main/en/installation
    - https://github.com/huggingface/notebooks/tree/main/lerobot
    - https://github.com/tonyzhaozh/act

- ***Dataset***:
    - [Diffusion Policy: How Diffusion Models Are Transforming Robot Learning from Demonstration](https://kargarisaac.medium.com/diffusion-policy-how-diffusion-models-are-transforming-robot-learning-from-demonstration-32c27ba829cf)
    - [Diffusion Policy Explained](https://medium.com/@ligerfotis/diffusion-policy-explained-14a3075ba26c)
    - https://huggingface.co/datasets/aractingi/pusht
