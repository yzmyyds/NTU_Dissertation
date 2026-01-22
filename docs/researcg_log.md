# VLA-R3L Research Log

## [2026-01-18] Topic Development
- After talking with mentor, determined my research topic: VLA-R3L.
- Decide to use diffusion policy (DPPO) as action trunk generator and design reflective RL to optimize the accuracy.
- Paper Reading
	- Steering Your Diffusion Policy with Latent Space Reinforcement Learning
		- Designed based on the diffusion model and to handle the behavior clone (BC) problem
            1. Use Latent-Noise Space
            2. Noise Aliasing
	- Diffusion Policy Policy Optimization
		Optimize the PPO method to fine-tune the diffusion policy
	- Reinforcement Learning with Action Chunking
		Design based on the Q-learning method, instead of accessing each action of joints, pay attention to the action trunk and adjusting TD based algorithm to make unbiased n-step backups more stable
        
## [2026-01-19] Environment Setup & Code Deconstruction
- Successfully connect cloud server
- DSRL dependencies 
  - conda env python 3.9 :white_check_mark:
  - dppo :white_check_mark:
  - robomimic :x:
  - gym :x:
The reason why these two environment fail is that they all relies on MuJoCo. I encountered development problem.
```bash
Successfully built dppo robosuite
Failed to build mujoco
ERROR: ERROR: Failed to build installable wheels for some pyproject.toml based projects (mujoco)
```
The set up instruction in git requires 
```bash
pip install -e ".[robomimic]"
pip install -e ".[gym]"
```
they will try to develop mujoco automatically, but they use mujoco_py at 2021, this will lead to mujoco, there are conflicts between mujoco_py and mujoco, causing error.

Bug status: [solved] :white_check_mark:
I tried to add the mujoco210 into environment path, and jump the mujoco dvelop in -e" ".
- run `dsrl_can.yaml` inference.
- outcome: 
```bash
wandb: Currently logged in as: wenkai001 (wenkai001-nanyang-technological-university-singapore). Use `wandb login --relogin` to force relogin
wandb: wandb version 0.24.0 is available!  To upgrade, please run:
wandb:  $ pip install wandb --upgrade
wandb: Tracking run with wandb version 0.17.7
wandb: Run data is saved locally in /home/wenkai001/ssd/ziming/dsrl/wandb/run-20260119_234516-l9c9r8jx
wandb: Run `wandb offline` to turn off syncing.
wandb: Syncing run robomimic_can_dsrl_2026-01-19_23-45-15_1
wandb: ⭐️ View project at https://wandb.ai/wenkai001-nanyang-technological-university-singapore/dsrl
wandb: 🚀 View run at https://wandb.ai/wenkai001-nanyang-technological-university-singapore/dsrl/runs/l9c9r8jx
[2026-01-19 23:45:20,372][root][INFO] - Number of network parameters: 575564
Error executing job with overrides: []
Error in call to target 'model.diffusion.diffusion_eval.DiffusionEval':
FileNotFoundError(2, 'No such file or directory')
full_key: model

Set the environment variable HYDRA_FULL_ERROR=1 for a complete stack trace.
wandb: 🚀 View run robomimic_can_dsrl_2026-01-19_23-45-15_1 at: https://wandb.ai/wenkai001-nanyang-technological-university-singapore/dsrl/runs/l9c9r8jx
wandb: ⭐️ View project at: https://wandb.ai/wenkai001-nanyang-technological-university-singapore/dsrl
wandb: Synced 6 W&B file(s), 0 media file(s), 0 artifact file(s) and 1 other file(s)
wandb: Find logs at: ./wandb/run-20260119_234516-l9c9r8jx/logs
wandb: WARNING The new W&B backend becomes opt-out in version 0.18.0; try it out with `wandb.require("core")`! See https://wandb.me/wandb-core for more information.
```
basiclly set up accessable environment, but need hfm5 demo video to train.

## [2026-01-20] Environment test & Meeting

- Meet with prof Ziwei and report my progress and schedule
- test one shallow video, tried to run, but failed due to the path error. Tries to include correct path into origin yaml file.

## [2026-01-21] RL with can task Run Successfully & Learning

- Debugging
  1. Firstly, I added the video into dataset, but there still returns error of ```FileNotFoundError```. Checking the the instruction note in DSRL github web, I found that I forgot to add the pretrained diffusion weight parameters into corresponding position (```cfg/robomimic-pretrain/can/can_pre_diffusion_mlp_ta4_td20/2024-06-28_13-29-54/checkpoint/state_5000.pt```). Only included npz and pt files correctly, the first check of program runs.
  2. Adding files, the yaml runs, but still had error. It still related to the mujoco edition problem. So I tried to make a fake mujoco which could let the program to find mujoco_py stead of mujoco. Because if program didn't find mujoco, pip would be called to re-download instead of using mujoco_py. However, I failed since there are plenty of port different between new and old edition. Finally, I reloaded the conda env and installed the mujoco-3.2.2. After doing this, the DSRL runs successfully. :fireworks:
- Learning
  After searching on the webset, I decide to use the book "EasyRL" to fulfill my basic knowledge about RL.

## [2026-01-22] Get Performance of can Task

- After about 20h running, the model had evoluted 132M epoch. The performance:
![can Task Result](can_wandb.png)
Key Evaluation Metric:
  - reward: -86.0
  - success_rate: 0.955
- 