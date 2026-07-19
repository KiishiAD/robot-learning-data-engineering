# Robot-Learning Data Engineering Pathway

From one ALOHA episode to reliable, fleet-scale training data.

## Target

Become capable of turning heterogeneous robot sensor streams into synchronized, validated, versioned and efficiently served datasets for policies and world models.

The objective is not to reproduce billions of robot-hours in a portfolio. It is to build components whose correctness survives growth: immutable raw logs, explicit schemas, idempotent processing, lineage, quality gates and reproducible sampling.

## Principles

- Preserve raw evidence.
- Transform explicitly.
- Validate before publication.
- Version data independently from code and models.
- Trace every training sample back to its raw episode and robot configuration.
- Measure accepted data quality, not only collected volume.

## Pathway

| Module | Time | Build | Completion test |
| --- | --- | --- | --- |
| 0. First learning loop | Weeks 1-2 | Inspect ALOHA data, evaluate ACT, train a baseline and inject data defects | Explain one sample completely and show how a data defect changes rollout success |
| 1. Robot-data semantics | Weeks 3-6 | Compatibility matrix across three robot datasets | Identify which fields can be pooled, transformed or cannot be reconciled |
| 2. Raw capture | Weeks 7-10 | Record RGB, depth, joints, transforms and actions to MCAP | Replay the recording and report drops, rates and latency |
| 3. Formats and conversion | Weeks 11-16 | MCAP-to-LeRobot v3 converter with RLDS compatibility | Re-running conversion is deterministic and does not duplicate episodes |
| 4. Data quality | Weeks 17-22 | Structural, temporal, physical, visual and semantic validators | Deliberate corruption is detected before training |
| 5. Distributed processing | Weeks 23-30 | Fault-tolerant DROID conversion backfill | Workers can fail and restart without corrupting the dataset snapshot |
| 6. Fleet collection | Weeks 31-38 | Simulated fleet with resumable upload and versioned manifests | Report accepted hours, rejection rate and capture-to-training latency |
| 7. Training-data serving | Weeks 39-44 | Deterministic episode and temporal-window loader | Change dataset mixtures without copying the underlying videos |
| 8. Learning and world models | Weeks 45-50 | ACT experiments and a small action-conditioned predictor | Connect synchronization, coverage and sampling choices to model behaviour |
| 9. Industrial capstone | Weeks 51-52+ | Complete capture-to-training data platform | Trace a failed model sample back to its raw recording and configuration |

## Module 0: first complete learning loop

Use the ALOHA transfer-cube dataset and simulator.

1. Inspect the camera observation, 14-dimensional robot state, action, timestamp and episode boundaries.
2. Evaluate the official ACT checkpoint before training.
3. Train checkpoints at 12,500, 25,000, 50,000 and, if useful, 100,000 steps.
4. Compare human and scripted demonstrations using the same evaluation states.
5. Compare 50 demonstrations with 10 under both equal-step and equal-epoch budgets.
6. Shift actions by 1, 3 and 5 frames, drop camera frames and swap two joint columns.

Use 10 evaluation episodes only as a smoke test. Use 50-100 paired episodes and multiple training seeds before making serious comparisons.

## Module 1: robot-data semantics

Learn joints, coordinate frames, rigid transforms, forward kinematics, action representations, control frequency, sensors and calibration.

Build a compatibility matrix covering:

- Robot embodiment and degrees of freedom.
- State and action meaning, shape, units and reference frame.
- Absolute versus relative actions.
- Control and sensor frequencies.
- Cameras and calibration.
- Task and success labels.

## Module 2: raw capture

Treat ROS 2 as the transport layer and MCAP as the immutable raw record.

Record sensor time, publication time, receipt time, command time and evidence of physical execution. Include robot, sensor, firmware, controller, policy and calibration versions in the episode manifest.

## Module 3: formats and conversion

Use formats by responsibility:

| Format | Responsibility |
| --- | --- |
| MCAP / rosbag2 | Raw capture and replay |
| LeRobot v3 | Primary PyTorch-oriented training format |
| RLDS / TFDS | Open X-Embodiment and TensorFlow compatibility |
| HDF5 / robomimic | Compatibility with existing imitation-learning pipelines |

The converter must expose field mappings, units, frame transformations, resampling, episode segmentation, video encoding, hashes and lineage. Invalid episodes go to quarantine; they are not silently repaired.

## Module 4: robotics data quality

Validate four layers:

- **Structural:** checksums, decoding, fields, shapes and dtypes.
- **Temporal:** monotonic timestamps, rates, jitter, drops and action-state lag.
- **Physical:** joint limits, impossible velocities, saturation and transform consistency.
- **Semantic:** task labels, success, action convention, calibration and controller compatibility.

Preserve valid failure episodes. Failed behaviour can be useful; corrupted or semantically unknowable data is different.

## Modules 5-7: scale and serving

Separate immutable raw data, canonical datasets, queryable episode metadata, disposable derived artifacts and logical training views.

Required properties:

- Idempotent workers and deterministic episode IDs.
- Resumable processing and upload.
- Atomic dataset publication.
- Incremental recomputation.
- Episode-level lineage.
- Reproducible filters, splits, weights and shuffling.
- Parallel video decoding, caching and throughput measurement.

## Module 8: CS285 and world models

Prioritize these [Berkeley CS285](https://rail.eecs.berkeley.edu/deeprlcourse/) topics:

1. Behavioural cloning.
2. Distribution shift.
3. Reinforcement-learning basics.
4. Model-based reinforcement learning.
5. Offline reinforcement learning.
6. Exploration.
7. Multi-task reinforcement learning.

Start world-model work with aligned temporal samples:

```text
context: images[t-15:t], states[t-15:t], actions[t-15:t]
targets: images[t+1:t+30], states[t+1:t+30]
```

## Capstone

Demonstrate one complete path:

```text
record MCAP
→ checksum and upload
→ validate
→ convert to LeRobot
→ publish a dataset snapshot
→ serve temporal samples
→ train and evaluate
→ mine failures
→ define the next collection priorities
```

The strongest final claim is not "I trained a robot policy." It is: "I built the infrastructure that turns unreliable robot activity into reproducible training data, and I proved it remains correct under failure."

## Core references

- [LeRobotDataset v3](https://huggingface.co/docs/lerobot/en/lerobot-dataset-v3)
- [ROS 2 tutorials](https://docs.ros.org/en/rolling/Tutorials.html)
- [MCAP with ROS 2](https://mcap.dev/guides/getting-started/ros-2)
- [RLDS](https://github.com/google-research/rlds)
- [DROID](https://droid-dataset.github.io/)
- [Open X-Embodiment](https://robotics-transformer-x.github.io/)
- [Modern Robotics](https://modernrobotics.northwestern.edu/)
- [Berkeley CS285](https://rail.eecs.berkeley.edu/deeprlcourse/)
