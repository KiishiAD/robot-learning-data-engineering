# Robot-Learning Data Engineering Pathway

From understanding one ALOHA episode to building a reliable path from robot activity to training data.

## Goal

Become capable of turning robot sensor streams into synchronized, validated, versioned and efficiently served datasets for robot policies and world models.

This roadmap is a practical guide, not a timetable. Progress comes from building each module and passing its completion test, rather than following fixed dates.

## How to use this roadmap

- Work through the modules in order because each one builds on the previous module.
- Keep each build small enough to run, inspect and explain locally.
- Use a simulator, public dataset or physical robot depending on what you can access.
- Record the dataset and software versions used so the result can be reproduced.
- Move on only when the module's completion test is supported by a working artifact.
- Introduce distributed or fleet-scale tools only when the smaller pipeline works and a measured bottleneck justifies them.

## Pathway at a glance

| Module | Capability | Practical build | Complete when |
| --- | --- | --- | --- |
| 0. First learning loop | Understand how demonstrations become policy behaviour | Inspect ALOHA data, evaluate ACT, train a baseline and run one controlled data experiment | You can explain a sample, reproduce an evaluation and show how one data choice affects the result |
| 1. Robot-data semantics | Compare data produced by different robots and controllers | Compatibility matrix across three robot datasets | You can identify what can be combined, what needs transformation and what is not safely comparable |
| 2. Raw capture | Record synchronized robot streams with enough context to replay them | ROS 2 and MCAP recording with an episode manifest | You can replay an episode and explain its timing, stream rates, drops and configuration |
| 3. Formats and conversion | Turn raw recordings into a training-ready dataset deterministically | Converter from MCAP to LeRobot | Re-running the conversion produces the same logical episodes without duplicates |
| 4. Data quality | Detect unusable or misleading data before training | Structural, temporal, physical, visual and semantic validators | Known defects are detected and reported before the dataset is published or loaded for training |
| 5. Reliable and resumable processing | Process larger datasets without losing or corrupting work | Resumable backfill over a bounded dataset | Workers can fail and restart while producing the same complete dataset snapshot |
| 6. Multi-source collection | Collect episodes consistently from several producers | Simulated or real multi-source upload pipeline | Interrupted, duplicate and invalid uploads are handled correctly and visibly |
| 7. Versioned training-data serving | Reproduce exactly what a training run consumed | Dataset snapshot and deterministic episode/window loader | A sample, split and dataset mixture can be reproduced from their identifiers and configuration |
| 8. Data-to-learning experiments | Connect data engineering choices to model behaviour | Controlled ACT experiments and an optional small predictor | One synchronization, coverage or sampling choice has a measured and explained effect |
| 9. End-to-end capstone | Operate the complete path from capture to learning | Small capture-to-training system with recovery and lineage | A model result can be traced to raw data and the pipeline recovers from an injected failure |

## Module 0: first learning loop

**Goal:** understand the complete path from one demonstration sample to evaluated policy behaviour.

Use the ALOHA transfer-cube dataset, ACT policy and matching simulator. Complete the module as four small milestones.

### 0A. Understand one episode

- Inspect camera observations, robot state, actions, timestamps and episode boundaries.
- Plot selected state and action dimensions through a complete episode.
- Explain what the model receives and what it must predict.
- Note important information that is missing or unclear, such as units or coordinate conventions.

**Done when:** you can choose one sample and explain every major field, where it occurs in the episode and how it relates to the surrounding samples.

Start with the existing [Module 0 guide](intro-to-robot-learning/README.md) and [ALOHA notebook](intro-to-robot-learning/aloha_transfer_cube_dataset.ipynb).

### 0B. Evaluate before training

- Run the official pretrained ACT checkpoint in the matching ALOHA simulator.
- Begin with a small smoke test to prove that the environment and checkpoint work together.
- Save the evaluation configuration, random seeds, rollout results and example videos.
- Group failures into understandable types, such as grasp, transfer, timing or release failures.

**Done when:** another person can rerun the evaluation with the recorded setup and obtain a comparable result.

### 0C. Train one baseline

- Train one ACT checkpoint on the human demonstration dataset.
- Save the training configuration, software versions, logs and checkpoint.
- Load the saved checkpoint in a fresh process.
- Evaluate it with the same task settings used for the pretrained checkpoint.
- Compare rollout success and failure types, not only training loss.

**Done when:** the saved artifacts reproduce one complete path from demonstrations to evaluated behaviour.

### 0D. Change one property of the data

Choose one focused experiment:

- use fewer unique demonstrations;
- compare human and scripted demonstrations;
- introduce a known action delay;
- drop selected camera frames; or
- alter one state or action mapping in a copy of the data.

Change one variable at a time and keep the evaluation conditions fixed.

**Done when:** you can compare the baseline and changed dataset under the same evaluation setup, measure the difference and explain what the result suggests.

## Module 1: robot-data semantics and compatibility

**Goal:** understand what robot fields mean before trying to combine or convert them.

Learn the minimum concepts needed to interpret robot data:

- joints and degrees of freedom;
- coordinate frames and rigid transforms;
- absolute and relative actions;
- control and sensor frequencies;
- camera intrinsics and extrinsics;
- calibration;
- task, success and failure labels; and
- collection method and controller behaviour.

Build a compatibility matrix for three datasets with meaningful differences. For every important field, compare its meaning, shape, units, reference frame, frequency and missing context.

Classify each comparison as:

- directly compatible;
- compatible after a documented transformation; or
- not safely comparable with the available information.

**Done when:** you can explain which data can be combined, which transformation is required and where guessing would make the result unreliable.

## Module 2: raw capture with ROS 2 and MCAP

**Goal:** create a raw episode that can be replayed and understood later.

Use ROS 2 as the message transport and MCAP or rosbag2 as the raw recording format. If physical hardware is unavailable, use a simulator or recorded-message replay.

Capture the streams available in your setup, such as:

- RGB or depth images;
- joint state;
- transforms;
- actions or commands;
- task events; and
- timestamps from the relevant clocks.

Create an episode manifest that records the robot, sensors, controller, calibration, software versions and expected stream rates.

Measure:

- actual stream rates;
- missing or dropped messages;
- timestamp order and drift;
- recording size; and
- the delay between command, observation and recording where it can be observed.

**Done when:** you can replay an episode, reconstruct how its streams line up and produce a short report of timing, drops, configuration and known limitations.

## Module 3: dataset formats and conversion

**Goal:** convert a raw recording into a training-ready dataset without hiding assumptions.

Use each format for a clear responsibility:

| Format | Responsibility in this pathway |
| --- | --- |
| MCAP / rosbag2 | Raw capture and replay |
| LeRobot | Primary training format |
| RLDS / TFDS | Compatibility with existing TensorFlow and Open X-Embodiment pipelines when needed |
| HDF5 / robomimic | Compatibility with existing imitation-learning pipelines when needed |

Build one converter first: MCAP to LeRobot. Add another target only when a real consumer requires it.

The converter should make these decisions visible:

- field mappings and units;
- coordinate-frame transformations;
- timestamp alignment and resampling;
- episode boundaries;
- image or video encoding;
- source identity and transformation history; and
- reasons an episode could not be converted.

Create a small, known-good episode fixture for automated tests. Also create one malformed fixture that must be rejected.

**Done when:** two clean runs over the same input produce the same episode identifiers and logical content, while invalid input is rejected without affecting the completed dataset.

## Module 4: robotics data quality

**Goal:** detect data that is broken, misleading or impossible to interpret before it reaches training.

Validate five layers:

- **Structural:** required fields, decoding, checksums, shapes and data types.
- **Temporal:** timestamp order, stream rates, jitter, drops and action-state delay.
- **Physical:** joint limits, impossible velocities, saturation and transform consistency.
- **Visual:** corrupt frames, frozen video, exposure problems and resolution changes.
- **Semantic:** task labels, success labels, action convention, calibration and controller compatibility.

Each check should pass, warn, reject or quarantine an episode with a specific reason. Keep valid task failures; failed robot behaviour is different from corrupted data.

Inject known defects into copies of the small fixture from Module 3 and run them through the validators.

**Done when:** every injected defect is detected by the expected check before the episode can be used for training.

## Module 5: reliable and resumable processing

**Goal:** process more data without making failures expensive or silently producing partial datasets.

Start this module only after the local converter is repeatable. First identify the bottleneck that requires more than one local process, such as conversion time, memory use or an operational deadline.

Use a bounded public-dataset subset or generated workload. Build processing that supports:

- deterministic episode identifiers;
- idempotent workers;
- resumable tasks;
- bounded retries with visible failure reasons;
- incremental recomputation; and
- atomic publication of a completed snapshot.

Test the failure path deliberately by terminating workers during processing and then restarting them.

**Done when:** the restarted run completes without duplicate or missing episodes and produces the same canonical episode IDs, counts and published manifest as a clean run.

## Module 6: multi-source collection

**Goal:** collect data consistently from more than one producer.

Use several simulators, replay producers or physical robots. A simulated setup is enough to learn the collection workflow even though it cannot prove physical robot reliability.

Build:

- producer identity and versioning;
- episode manifests;
- resumable uploads with integrity checks;
- duplicate detection;
- rejection or quarantine paths; and
- a simple view of accepted episodes, rejected episodes and upload delay.

Exercise the difficult cases:

- interrupt an upload and resume it;
- submit the same episode twice;
- submit an invalid episode; and
- change one producer's schema or software version.

**Done when:** the system handles each case predictably and reports how much submitted data became accepted, usable data.

## Module 7: versioned training-data serving

**Goal:** make training inputs reproducible without copying the underlying media for every experiment.

Separate:

- immutable raw recordings;
- validated canonical datasets;
- searchable episode metadata;
- disposable derived files; and
- logical training views made from filters, splits and weights.

Build a loader or small service that supports:

- versioned dataset snapshots;
- deterministic train, validation and test splits;
- reproducible filtering, weighting and shuffling;
- deterministic temporal windows;
- lookup by episode or sample identifier; and
- measured loading and decoding throughput.

**Done when:** you can reproduce a sample and dataset mixture from identifiers and configuration, and an older experiment can still resolve the exact snapshot it used.

## Module 8: data-to-learning experiments

**Goal:** measure how data engineering decisions affect learning and behaviour.

Learn the machine-learning concepts when an experiment requires them. A useful order is:

1. behavioural cloning;
2. distribution shift;
3. reinforcement-learning basics;
4. model-based reinforcement learning; and
5. offline reinforcement learning.

Use ACT for the first experiments because Module 0 already provides a working training and evaluation loop. Test one data question at a time, such as:

- synchronization error;
- number and variety of demonstrations;
- task or failure coverage;
- temporal context length;
- sampling balance; or
- mixing data from different sources.

Add a small action-conditioned predictor only after the dataset, loader and evaluation process are reproducible. Treat it as another consumer of the data pipeline, not as a separate platform project.

**Done when:** you can connect one specific data decision to a measured change in model behaviour, state the uncertainty and give a plausible explanation.

## Module 9: end-to-end capstone

**Goal:** prove that the complete data path works and remains understandable when something fails.

Build one small system that demonstrates this flow:

```text
capture a raw episode
-> checksum and upload
-> validate
-> convert to a versioned training format
-> publish a complete dataset snapshot
-> serve reproducible temporal samples
-> train and evaluate
-> trace a model result back to its source
-> decide what data to collect or repair next
```

The capstone should include:

- a tested way to restart or replay failed processing;
- dataset, schema and configuration versions;
- measured storage, processing and loading performance;
- monitoring for failures actually encountered during the project; and
- instructions that let another person reproduce one complete path.

Inject one bounded processing failure and recover from it. Then select one successful or failed model sample and trace it through every transformation back to the raw episode.

**Done when:** the recovered pipeline publishes a complete snapshot, the chosen sample is reproducible and its full lineage can be explained.

The final claim should be concrete: "I built and tested the path that turns raw robot activity into reproducible training data, and I can trace and recover that path when it fails."

## Core references

- [LeRobotDataset documentation](https://huggingface.co/docs/lerobot/en/lerobot-dataset-v3)
- [ROS 2 tutorials](https://docs.ros.org/en/rolling/Tutorials.html)
- [MCAP with ROS 2](https://mcap.dev/guides/getting-started/ros-2)
- [RLDS](https://github.com/google-research/rlds)
- [DROID](https://droid-dataset.github.io/)
- [Open X-Embodiment](https://robotics-transformer-x.github.io/)
- [Modern Robotics](https://modernrobotics.northwestern.edu/)
- [Berkeley CS285](https://rail.eecs.berkeley.edu/deeprlcourse/)
