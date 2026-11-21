# sd3-speller

How much RL fine-tuning of a text-to-image model can you get out of one rented GPU?

[Flow-GRPO](https://arxiv.org/abs/2505.05470) showed that online RL makes SD3.5-M a lot better at following prompts and at spelling text inside images: GenEval went from 63% to 95%, text rendering from 59% to 92%. The recommended config samples 48 groups of 24 images every epoch, and most of the training scripts are multi-node. I don't have a cluster, and I couldn't find numbers on what happens with far less compute. So I'm measuring it.

The second thing I want to check is whether the gain is real. The reward for text rendering comes from an OCR model, and a model trained against a judge learns to please that judge, which is not always the same as getting better. The same group later described this in [GRPO-Guard](https://arxiv.org/abs/2510.22319): the reward keeps going up while image quality goes down. So every tuned checkpoint gets re-scored by judges that were never part of training (another OCR engine, strict exact match, preference and quality models), on prompts with words the model didn't see.

The setup is SD3.5-M with a LoRA, text rendering as the task, one GPU.

## Status

Just started, nothing to run yet.

## Roadmap

1. Text metrics and a prompt set with a held-out test split
2. Baselines: a couple of open models, quality against cost per image
3. The RL loop, first on a 2D toy problem where the math is easy to check, then on SD3.5-M
4. Training runs on a fixed budget
5. The audit, and a write-up of what worked and what didn't

Reference code for the method: https://github.com/yifan123/flow_grpo
