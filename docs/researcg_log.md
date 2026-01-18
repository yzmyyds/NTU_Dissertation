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
- **Status:** 🔴 In Progress
- **Today's Goal:** Successfully install DSRL dependencies and run `dsrl_can.yaml` inference.

### 📝 Implementation Notes
- **DSRL Repo Analysis:**
    - Core logic is in `train_dsrl.py`.
    - It uses a specialized fork of `Stable Baselines3` for the latent optimization.
- **Environment Progress:**
    - [x] Conda env created (Python 3.9).
    - [ ] `pip install -e .` for DPPO fork (Investigating dependency conflict with `numpy`).

### 💡 Thoughts & Reflections
- **Residual RL logic:** After reading the README, I realize the "steering" happens at the noise sampling stage, not the action chunking stage. I need to consider how to align this with the ACT-style action trunks.

---