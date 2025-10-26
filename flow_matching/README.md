# Flow-Matching

### setup-a-conda-environment

```
git clone git@github.com:hyunjaeKang/flow_matching.git temp_flow_matching


conda create -y -n flow_matching python=3.10
conda activate flow_matching

pip install ipykernel ipywidgets
pip install moviepy==1.0.2 opencv-python mediapy
pip install torch renderlab matplotlib scipy
pip install "gymnasium[classic-control]" mujoco robomimic==0.2.0
pip install stable_baselines3 nlopt
pip install hydra-core dill wandb zarr diffusers einops imagecodecs numba
pip install git+https://github.com/facebookresearch/pytorch3d.git@stable
pip install matplotlib termcolor torchdiffeq torchsde torchdyn torchcfm 
pip install pymunk shapely gdown scikit-image scikit-learn scikit-video
```

