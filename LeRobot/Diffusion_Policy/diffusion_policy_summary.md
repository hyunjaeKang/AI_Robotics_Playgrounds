# Diffusion Policy: A Summary

## What is Diffusion Policy?

Diffusion policy is a technique that applies **diffusion models**
(originally used for generative tasks like image synthesis) to **policy
learning** in robotics. Instead of directly predicting the next action,
the policy generates an entire **sequence of actions (trajectory)**
through a denoising process.

------------------------------------------------------------------------

## Core Idea

1.  **Trajectory as Data**
    -   Actions are treated as a *sequence* (like pixels in an image).\
    -   The policy learns a distribution over possible action sequences
        given observations.
2.  **Diffusion Process**
    -   **Training:** Add noise to action trajectories step by step
        until they become nearly random.\
    -   **Learning:** Train a neural network to reverse this process,
        i.e., denoise and recover the original trajectory conditioned on
        observations (images, states, goals).
3.  **Inference (Control)**
    -   At runtime, start from pure noise.\
    -   Iteratively denoise into a trajectory that is consistent with
        the current observation.\
    -   Execute the first action (or short chunk) of that trajectory.\
    -   Repeat at the next timestep.

------------------------------------------------------------------------

## Why It Works Well

-   **Expressivity:** Diffusion models can represent complex, multimodal
    distributions (e.g., multiple possible valid ways to solve a task).\
-   **Stability:** More stable training compared to GANs or
    autoregressive policies.\
-   **Flexibility:** Handles high-dimensional action spaces and
    vision-based inputs (pixel observations).

------------------------------------------------------------------------

## Applications

-   Robot manipulation (grasping, tool use, assembly).\
-   Navigation and locomotion.\
-   Any task where action sequences are complex and multimodal.

------------------------------------------------------------------------

✅ **In short:**\
**Diffusion policy reframes action prediction as a conditional
generative modeling problem, where actions are denoised step by step
into feasible trajectories given observations.**

----
Powered by chatGPT
