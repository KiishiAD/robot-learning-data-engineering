# Module 0 resources

## Data inspection

- [Hugging Face Robotics Course: introduction to robot learning](https://huggingface.co/learn/robotics-course/unit1/1): introduces the core concepts and LeRobot.
- [ALOHA human-demonstration dataset](https://huggingface.co/datasets/lerobot/aloha_sim_transfer_cube_human): the dataset card, schema and metadata used by the notebook. Record the revision you use.
- [LeRobot dataset visualizer](https://huggingface.co/spaces/lerobot/visualize_dataset): inspect complete episodes alongside state and action values.
- [LeRobotDataset documentation](https://huggingface.co/docs/lerobot/en/lerobot-dataset-v3): explains the dataset layout and loading interface.
- [Hugging Face dataset cards](https://huggingface.co/docs/hub/datasets-cards): explains where dataset descriptions, licence metadata and usage context should be documented.

## Evaluation and training

- [Official ALOHA ACT checkpoint](https://huggingface.co/lerobot/act_aloha_sim_transfer_cube_human): model card, training provenance and published evaluation evidence for the baseline used in Module 0B.
- [ACT documentation](https://huggingface.co/docs/lerobot/act): policy overview and current LeRobot training and evaluation entry points.
- [gym-aloha](https://github.com/huggingface/gym-aloha): the matching ALOHA simulation environment. Pin a compatible revision with the LeRobot version used for evaluation.

Compatibility warning: the checkpoint card records LeRobot commit `3c0a209` and an older evaluation interface, while the current ACT documentation uses a different entry point. This repository has not yet tested and pinned a complete Module 0B environment. Treat setup as unresolved until the version matrix in the module guide is replaced with executed commands and verified revisions.

## Optional context

- [Learning fine-grained bimanual manipulation with low-cost hardware](https://arxiv.org/abs/2304.13705): the ACT and ALOHA paper.
- [The paradigm shift towards multimodal foundation models](https://www.youtube.com/watch?v=VEs1QYEgOQo): broader context for modern robot learning. It is not required to complete this module.
