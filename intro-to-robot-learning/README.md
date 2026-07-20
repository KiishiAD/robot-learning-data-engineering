# Module 0A: understand one robot episode

This milestone begins with the ALOHA transfer-cube dataset: two simulated robot arms passing a cube from one gripper to the other.

Module 0 is split into four milestones in the [roadmap](../ROADMAP.md#module-0-first-complete-learning-loop):

- 0A: understand the dataset;
- 0B: evaluate the official pretrained ACT policy;
- 0C: train and evaluate one baseline;
- 0D: run one controlled data experiment.

This directory currently covers 0A. Evaluation, training and controlled data changes are later artifacts, not requirements hidden inside this notebook.

## Current artifact

[`aloha_transfer_cube_dataset.ipynb`](aloha_transfer_cube_dataset.ipynb) currently:

- loads a sample from `lerobot/aloha_sim_transfer_cube_human`;
- requests a small temporal window;
- displays one current top-camera observation;
- prints the available fields and 14-dimensional robot state.

It does not yet plot a complete episode or inspect the action, timestamp, episode index and frame index together. Those are remaining learning tasks for 0A. The notebook itself is intentionally unchanged by this documentation update.

## Learning objectives

- Distinguish an episode from an individual sample.
- Understand camera observations, robot state and actions.
- Interpret the 14 state values as the ordered motor positions of both arms.
- Understand how `delta_timestamps` selects temporal context and future targets.
- Locate a sample within its episode using timestamp and frame metadata.
- Identify units, coordinate conventions, calibration and collection details that would be required to reproduce the data on another robot.
- Record the dataset revision, software environment and stated data licence rather than relying on a moving default branch or public visibility.

## Published environment status

The repository does not yet contain a verified environment for Module 0B. The existing notebook has outputs, but it does not pin enough of its environment to reproduce them independently.

The [checkpoint model card](https://huggingface.co/lerobot/act_aloha_sim_transfer_cube_human) says the model was trained with LeRobot commit [`3c0a209`](https://github.com/huggingface/lerobot/tree/3c0a209f9fac4d2a57617e686a7f2a2309144ba2) and refers to an older `eval.py` interface. Current ACT documentation uses a different evaluation entry point. These sources are useful history, not a tested combination for this repository.

| Component | Verified version or revision | Status |
| --- | --- | --- |
| Python | Not recorded | Unresolved |
| LeRobot | Not recorded | Unresolved |
| `gym-aloha` | Not recorded | Unresolved |
| ALOHA dataset snapshot | Not pinned | Unresolved |
| ACT checkpoint snapshot | Not pinned | Unresolved |
| PyTorch, CUDA and GPU requirements | Not recorded | Unresolved |

The first setup task for Module 0B is to run one compatible combination, record the exact commands and replace this table with tested pins. Until then, the repository should not present an evaluation command as known-good.

## Run the milestone

1. Read the dataset's [`meta/info.json`](https://huggingface.co/datasets/lerobot/aloha_sim_transfer_cube_human/blob/main/meta/info.json) and dataset card.
2. Record the dataset revision and the LeRobot version or commit used.
3. Run the notebook from top to bottom.
4. Use the [LeRobot visualizer](https://huggingface.co/spaces/lerobot/visualize_dataset) to watch complete episodes.
5. Inspect one sample's observation, state, action, timestamp, episode index and frame index together.
6. Plot selected state and action dimensions through one episode.
7. Complete a copy of the [module log](../MODULE_LOG_TEMPLATE.md), including a brief rights and cost note. For this public simulation exercise, a rough account of download size, storage and compute is enough.

## Completion test

Choose one sample and explain:

- what the robot can see;
- what the 14 state values represent;
- what action values the model must predict;
- when the sample occurs within its episode;
- how the temporal window relates to that point;
- which units, coordinate conventions and collection metadata remain unclear;
- which exact dataset and software revisions produced the result.

Include the plot, sample identifiers and environment details in the module log so someone else can check the explanation.

## Next milestone

After 0A is evidenced in the repository, continue to [0B: evaluate before training](../ROADMAP.md#0b-evaluate-before-training). Run the official pretrained ACT checkpoint in the matching ALOHA simulator, save rollout evidence and record an actual success result before training a new policy.
