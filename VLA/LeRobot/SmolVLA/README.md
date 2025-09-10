# SmolVLA

- SmolVLA (Small Vision-Language-Action) is an open-source, compact artificial intelligence model for robotics developed by Hugging Face in collaboration with the community. It is designed to run efficiently on consumer-grade hardware and uses publicly available datasets, which democratizes access to advanced robotics. 

- Key features of SmolVLA include:
    - Compact size The model has only about 450 million parameters, allowing it to run on a single consumer GPU or even a CPU. This is in contrast to many larger, resource-intensive VLA models with billions of parameters.
    - Efficient architecture SmolVLA uses several optimizations for high performance and low latency:
        - Layer skipping: It uses features from intermediate layers of its vision-language model (VLM) backbone rather than the final, more computationally expensive layers.
        - Reduced visual tokens: The model processes images with a reduced number of visual tokens to minimize computational overhead.
        - Interleaved attention: The "action expert" component uses an interleaved attention mechanism that combines both self-attention for smoother action sequences and cross-attention for better grounding in perception and instructions.
    - Community-driven training The model was trained entirely on diverse, publicly available datasets from the Hugging Face LeRobot community. This reliance on open data makes the model transparent, reproducible, and robust to real-world variations and noise.
    - Asynchronous inference To improve responsiveness and reduce idle time, SmolVLA uses an asynchronous inference stack. This allows the robot to begin executing its next set of actions while the model is still processing the current observation.
    - High performance Despite its smaller size, SmolVLA is highly capable. In both simulated environments (like LIBERO and Meta-World) and with real robots (like the SO100), it has been shown to match or exceed the performance of much larger VLA models. 

- Block Diagram of SmolVLA
    - <img src="https://cdn-uploads.huggingface.co/production/uploads/640e21ef3c82bd463ee5a76d/aooU0a3DMtYmy_1IWMaIM.png" alt="Your image title" width=30% height=30%/>
    
    

- VAL Flow Matching
    - <img src="https://learnopencv.com/wp-content/uploads/2025/06/smolvla-architecture-vlm-flowmatching-lerobot.png" alt="Your image title" width=30% height=30%/>

---

### Reference: 

- HuggingFace Blog: [https://huggingface.co/blog/smolvla](https://huggingface.co/blog/smolvla)
- Paper: https://huggingface.co/papers/2506.01844
- LearnOpenCV : https://learnopencv.com/smolvla-lerobot-vision-language-action-model/