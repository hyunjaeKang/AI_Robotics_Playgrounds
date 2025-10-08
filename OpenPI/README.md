# openpi


## Install OpenPI
```
# pwd
# >> ~/AI_Robotics_Playgrounds/OpenPI/

git clone --recurse-submodules git@github.com:Physical-Intelligence/openpi.git temp_openpi
cd temp_openpi
GIT_LFS_SKIP_SMUDGE=1 uv sync
GIT_LFS_SKIP_SMUDGE=1 uv pip install -e .
```

----
## Examples

### 1. Run Aloha Sim

- [Terminal #1]
```
# pwd
# >> ~/AI_Robotics_Playgrounds/OpenPI/

source ./temp_openpi/.venv/bin/activate
uv run ./temp_openpi/scripts/serve_policy.py --env ALOHA_SIM
```


- [Terminal #2]

```
# pwd
# >> ~/AI_Robotics_Playgrounds/OpenPI/

uv venv --python 3.10 ./temp_openpi/examples/aloha_sim/.venv
source ./temp_openpi/examples/aloha_sim/.venv/bin/activate
uv pip sync ./temp_openpi/examples/aloha_sim/requirements.txt
uv pip install -e ./temp_openpi/packages/openpi-client


MUJOCO_GL=egl python ./temp_openpi/examples/aloha_sim/main.py
```

------

### 2. Run Simple Client

- [Terminal #1]
```
# pwd
# >> ~/AI_Robotics_Playgrounds/OpenPI/

source ./temp_openpi/.venv/bin/activate
uv run ./temp_openpi/scripts/serve_policy.py --env DROID
```


- [Terminal #2]
```
# pwd
# >> ~/AI_Robotics_Playgrounds/OpenPI/

uv venv --python 3.10 ./temp_openpi/examples/simple_client/.venv
source ./temp_openpi/examples/simple_client/.venv/bin/activate
uv pip sync ./temp_openpi/examples/simple_client/requirements.txt
uv pip install -e ./temp_openpi/packages/openpi-client


python ./temp_openpi/examples/simple_client/main.py --env DROID
```


---
### Reference:


- ***Papers***:
    - [π0: Our First Generalist Policy](https://www.physicalintelligence.company/download/pi0.pdf)
    - [FAST: Efficient Action Tokenization for Vision-Language-Action Models](https://arxiv.org/abs/2501.09747)
    - [π0.5: a VLA with Open-World Generalization](https://www.physicalintelligence.company/download/pi05.pdf)
    
- ***Blog***:
    - [π0: Our First Generalist Policy](https://www.physicalintelligence.company/blog/pi0)
    - [FAST: Efficient Robot Action Tokenization](https://www.physicalintelligence.company/research/fast)
    - [π0.5: a VLA with Open-World Generalization](https://www.physicalintelligence.company/blog/pi05)
    - [LAs that Train Fast, Run Fast, and Generalize Better](https://www.physicalintelligence.company/research/knowledge_insulation)

- ***Github***:
    - [openpi](https://github.com/Physical-Intelligence/openpi/tree/main)