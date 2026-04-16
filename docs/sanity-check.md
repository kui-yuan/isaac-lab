1. Follow Quick Installation Guide section in Isaac-Lab-Quickstart-Guide.md to setup dependencies.

2. UR10e Reach Task with proprioceptive observations
    ```bash
    nohup python scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Deploy-Reach-UR10e-v0 --headless --num_envs 256 --experiment_name sanity-check --run_name proprioceptive --logger wandb --log_project_name isaac > logs/nohup/sanity-check/proprioceptive.out &
    ```
