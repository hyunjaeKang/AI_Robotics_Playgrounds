# ACT: Action Chunking with Transformers

----

Action Chunking with Transformers (ACT) is an imitation learning algorithm designed for robotic manipulation that predicts a sequence of actions, or an "action chunk," instead of a single action. By leveraging a Conditional Variational Autoencoder (CVAE) and a transformer architecture, ACT mitigates the problem of compounding errors that is common in traditional behavior cloning.

<img src="https://tonyzhaozh.github.io/aloha/resources/algo.png" alt="Your image title" width=100% height=100%/>

Fig. 4: Architecture of Action Chunking with Transformers (ACT). We train ACT as a Conditional VAE (CVAE), which has an encoder and a decoder. Left: The encoder of the CVAE compresses action sequence and joint observation into z, the style variable. The encoder is discardedat test time. Right: The decoder or policy of ACT synthesizes images from multiple viewpoints, joint positions, and z with a transformer encoder, and predicts a sequence of actions with a transformer decoder. z is simply set to the mean of the prior (i.e. zero) at test time.

----

### Setup a conda environment


- conda env : [le_robot](../README.md#setup-a-conda-environment)


----

- **ACtion Chunking Transformer (ACT) Policy / AlohaTransferCube**
<table border="1">
  <tbody>
    <tr>
      <td>
        <figure>
          <video width="320" height="240" controls>
            <source src="https://github.com/hyunjaeKang/AI_Robotics_Playgrounds/raw/main/LeRobot/ACT/output/train_videos_act_aloha_transfer/videos_step_020000/eval_episode_0.mp4" type="video/mp4">
          </video>
          <figcaption align="center">
            Episode #0
          </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <video width="320" height="240" controls>
            <source src="https://github.com/hyunjaeKang/AI_Robotics_Playgrounds/raw/main/LeRobot/ACT/output/train_videos_act_aloha_transfer/videos_step_020000/eval_episode_1.mp4" type="video/mp4">
          </video>
          <figcaption align="center">
            Episode #1
          </figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <video width="320" height="240" controls>
            <source src="https://github.com/hyunjaeKang/AI_Robotics_Playgrounds/raw/main/LeRobot/ACT/output/train_videos_act_aloha_transfer/videos_step_020000/eval_episode_2.mp4" type="video/mp4">
          </video>
          <figcaption align="center">
            Episode #2
          </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <video width="320" height="240" controls>
            <source src="https://github.com/hyunjaeKang/AI_Robotics_Playgrounds/raw/main/LeRobot/ACT/output/train_videos_act_aloha_transfer/videos_step_020000/eval_episode_3.mp4" type="video/mp4">
          </video>
          <figcaption align="center">
            Episode #3
          </figcaption>
        </figure>
      </td>
    </tr>
  </tbody>
</table>


---

### References:

- ***Paper***:
    - https://arxiv.org/abs/2304.13705
    - https://huggingface.co/papers/2304.13705
- ***Blog***:
    - https://huggingface.co/lerobot/act_aloha_sim_transfer_cube_human
    - https://tonyzhaozh.github.io/aloha/

- ***Github***:
    - https://github.com/tonyzhaozh/act


