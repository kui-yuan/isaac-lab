1. Follow Quick Installation Guide section in Isaac-Lab-Quickstart-Guide.md to setup dependencies.

2. Train a PPO agent on the UR10e Reach Task with proprioceptive observations.
    ```bash
    python scripts/reinforcement_learning/rsl_rl/train.py \
    --headless \
    --task=Isaac-Deploy-Reach-UR10e-v0 \
    --num_envs 256 \
    --max_iterations 1000 \
    --experiment_name sanity-check \
    --run_name ur10e-reach-proprioceptive \
    --logger wandb \
    --log_project_name isaac
    ```
