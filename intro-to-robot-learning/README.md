# Module 0: Understanding One Robot Episode

This module begins with the ALOHA transfer-cube dataset: two simulated robot arms passing a cube from one gripper to the other.

## Learning objectives

- Distinguish an episode from an individual sample.
- Understand camera observations, robot state and actions.
- Interpret the 14 state values as the ordered motor positions of both arms.
- Understand how `delta_timestamps` returns temporal context and future action targets.
- Identify metadata that would be required to reproduce the data on another robot.

## Files

- [`aloha_transfer_cube_dataset.ipynb`](aloha_transfer_cube_dataset.ipynb): loads one sample, displays the top-camera image and inspects robot state.
- [`Resources.md`](Resources.md): the references used in this module.

## Run the module

1. Read the dataset's [`meta/info.json`](https://huggingface.co/datasets/lerobot/aloha_sim_transfer_cube_human/blob/main/meta/info.json).
2. Run the notebook from top to bottom.
3. Use the [LeRobot visualizer](https://huggingface.co/spaces/lerobot/visualize_dataset) to watch complete episodes.
4. Explain the observation and action fields in plain English.
5. Plot selected state and action dimensions through one episode.

## Completion test

Choose one sample and explain:

- What the robot can see.
- What the 14 state values represent.
- What action values the model must predict.
- When the sample occurs within its episode.
- Which units, coordinate conventions and collection metadata are still needed.
