# Robot-learning data engineering pathway

From one ALOHA episode to reliable training data that another person can inspect, reproduce and trust.

## Target

Become capable of turning robot sensor streams into synchronized, validated, versioned and efficiently served datasets for policy and world-model training.

This is a practical pathway, not a timetable. Move to the next module when the completion evidence exists. Someone with relevant experience may move quickly; someone encountering a topic for the first time may need several iterations.

## How to use this pathway

- Start at the first module whose completion test you cannot already prove.
- Produce a working artifact and a short [module log](MODULE_LOG_TEMPLATE.md) for every module.
- Use simulation, public datasets or physical hardware according to what you can access. State which one you used and what the choice prevents you from claiming.
- Keep experiments small until a measurement shows that scale is the problem.
- Treat a failed experiment as useful evidence when the setup and result are reproducible.
- Do not claim production readiness from a notebook or a successful local run.

## Working principles

- Preserve the original recording and its context.
- Make transformations explicit and reproducible.
- Validate data before publication or training.
- Version datasets independently from code and models.
- Trace every training sample to its source episode, robot configuration and transformation history.
- Quarantine data that cannot be interpreted safely instead of silently repairing it.
- Measure accepted, usable data rather than only collected volume.
- Record cost, rights and operational limitations alongside technical results.

## Evidence expected from every module

Each completed module should leave behind:

- a runnable artifact, such as a notebook, converter, validator or service;
- a pinned environment and the exact data snapshot or source revision;
- automated checks or a repeatable manual verification procedure;
- measurements, including failures and uncertainty;
- a short account of data rights, access and retention constraints;
- the important costs, including compute, storage, network and human effort;
- a decision about what the evidence supports doing next.

Use [`MODULE_LOG_TEMPLATE.md`](MODULE_LOG_TEMPLATE.md) to keep this evidence consistent without turning the repository into paperwork.

## Pathway

| Module | Build | Completion test |
| --- | --- | --- |
| 0. First learning loop | Inspect ALOHA data, evaluate ACT, train one baseline and run one controlled data experiment | Explain a sample completely, reproduce an evaluation and show one measured connection between data and behaviour |
| 1. Robot-data semantics | Compatibility and rights matrix across three robot datasets | Identify what can be pooled, transformed or cannot be reconciled, and explain why |
| 2. Raw capture | Record synchronized sensor and control streams with an episode manifest | Replay the recording and report timing, drops, calibration, versions, rights and capture cost |
| 3. Formats and conversion | Deterministic converter from the raw record to one training format | A clean rerun produces the same logical episodes without duplicates; invalid episodes are quarantined |
| 4. Data quality | Structural, temporal, physical, visual and semantic validators | Deliberate corruption is detected before publication or training |
| 5. Reliable processing | Resumable backfill over a bounded dataset subset | Workers can fail and restart without corrupting or partially publishing a snapshot |
| 6. Multi-source collection | Simulated or real collection from several producers with resumable upload and manifests | Report accepted data, rejection reasons, capture-to-availability latency and cost per accepted hour |
| 7. Training-data serving | Deterministic episode and temporal-window loader over versioned snapshots | Reproduce a sample and dataset mixture without copying the underlying media |
| 8. Data-to-learning experiments | ACT experiments and, if justified, a small action-conditioned predictor | Connect a specific synchronization, coverage or sampling choice to measured model behaviour |
| 9. Operational capstone | A complete capture-to-training data system with recovery and audit evidence | Trace a failed model sample to raw data, reproduce its processing and recover from an injected failure |

## Professional practice across the pathway

These are not separate modules to postpone until the end. Apply the relevant parts from the first dataset onward.

### Data rights and governance

Track the rights for datasets, code, model checkpoints and media separately. A repository being public does not establish permission to redistribute its data or use it for every training purpose.

For each source, record:

- who created and owns it;
- the stated licence and where that statement was found;
- permitted use, redistribution and derivative-work conditions;
- consent or privacy constraints when people, homes, workplaces or personal information may appear;
- retention, deletion and access requirements;
- any uncertainty that requires review before use or publication.

Do not replace unknown rights with an assumption. Use data with clear terms for exercises and record unresolved cases as blockers.

### Security and privacy

- Keep credentials and private URLs out of notebooks, manifests and Git history.
- Restrict raw recordings to the people and systems that need them.
- Encrypt sensitive data in transit and at rest where the environment requires it.
- Minimize capture of faces, voices, screens, addresses and unrelated surroundings.
- Record who accessed or changed restricted datasets when the system moves beyond personal experimentation.
- Define how compromised credentials, accidental disclosure and deletion requests would be handled.

The exact controls depend on the data and organisation. State what applies instead of copying an enterprise checklist into a small experiment.

### Cost and capacity

Track the costs that could change a technical decision:

- operator and robot time used for collection;
- review, annotation and relabelling time;
- raw and derived storage;
- video encoding and conversion compute;
- network transfer and cloud egress;
- training and evaluation compute;
- repeated work caused by rejected or corrupted episodes.

Useful measures include cost per recorded hour, cost per accepted hour, storage per episode, conversion time per hour of data and time from capture to training availability. Early modules can use rough local estimates. Later modules should use measured resource consumption and stated prices. Record the currency, pricing date, provider, service or hardware tier, region and price source. For human effort, explain the rate basis instead of presenting an unexplained labour cost.

### Reproducibility and change management

- Pin the software environment and record hardware assumptions.
- Version schemas, calibration, robot configuration, controller, policy and collection instructions.
- Use small golden episodes as test fixtures.
- Review schema changes and provide migration or rollback instructions.
- Publish datasets atomically so readers never see a half-built snapshot.
- Keep runbooks for recovery steps that are too important to live only in one person's memory.

### Reliability and observability

Measure enough to tell whether the system is healthy:

- capture and processing throughput;
- missing, rejected, quarantined and accepted episodes;
- timestamp drift, drops and processing lag;
- retry counts and failure reasons;
- storage growth and queue depth;
- data age from capture to training availability.

Add service objectives only when other people or automated jobs depend on the system. Before then, simple measurements and explicit failure reports are enough.

## Module 0: first complete learning loop

Use the ALOHA transfer-cube dataset, ACT policy and matching simulator. Complete this module in four milestones rather than treating it as one large assignment.

### 0A. Understand the data

1. Inspect camera observations, the 14-dimensional robot state, actions, timestamps and episode boundaries.
2. Plot selected state and action dimensions through one episode.
3. Explain what is measured, what the model predicts and what metadata would be needed to reproduce the data on another robot.
4. Record the dataset revision, software version and any undocumented units or conventions.

Completion evidence: choose one sample and explain every major field, its time within the episode and the important missing context.

### 0B. Evaluate before training

1. Run the official pretrained ACT checkpoint in the matching simulator.
2. Use a small number of episodes to prove the environment, checkpoint and evaluation pipeline work.
3. Save rollout videos, success results, environment settings and random seeds.
4. Classify visible failures, such as grasping, transfer, timing or release.

Completion evidence: another person can rerun the evaluation and obtain comparable results. A training-loss graph does not satisfy this milestone.

A small evaluation is a smoke test, not a performance claim. For comparisons, choose the episode count and number of seeds based on the size of the expected effect, available compute and acceptable uncertainty. Report the sample size and limitations.

### 0C. Train one baseline

1. Train one ACT checkpoint on the human demonstrations using a recorded configuration.
2. Start with a bounded update budget. Extend it only when the learning curve and compute budget justify another run.
3. Load the saved checkpoint in a fresh process and evaluate it under the same settings used for the pretrained policy.
4. Compare rollout success and failure types, not only loss.

Completion evidence: the checkpoint, configuration, logs and evaluation results reproduce one baseline from data to behaviour.

### 0D. Change one property of the data

Choose one experiment:

- compare human and scripted demonstrations;
- reduce the number of unique demonstrations while controlling the training budget;
- introduce a known action delay;
- drop camera frames;
- alter a joint mapping in a copy of the data.

Change one variable at a time and keep the evaluation conditions fixed. Where the change represents corruption, first test whether the quality checks catch it. Train only when the experiment asks a question that validation alone cannot answer.

Completion evidence: show either that the defect was rejected before training or that the controlled data change produced a measured behavioural difference.

## Module 1: robot-data semantics

Learn the minimum kinematics and control concepts needed to interpret data: joints, coordinate frames, rigid transforms, action representations, control frequency, sensors and calibration.

Build a compatibility matrix covering:

- robot embodiment and degrees of freedom;
- state and action meaning, shape, units and reference frame;
- absolute versus relative actions;
- control and sensor frequencies;
- cameras, intrinsics, extrinsics and calibration procedure;
- task, failure and success labels;
- collection method and controller;
- licence, redistribution and privacy constraints;
- missing information and the consequence of guessing.

Use three datasets with meaningful differences. Do not write a generic standard yet.

Completion evidence: for each pair, classify fields as directly compatible, transformable with documented assumptions, or unsafe to combine.

## Module 2: raw capture

Use ROS 2 as the transport layer and MCAP or rosbag2 as the raw recording format for this pathway. These are practical choices, not universal requirements. Explain the alternatives if your environment uses something else.

Record sensor time, publication time, receipt time, command time and, where available, evidence of physical execution. Include robot, sensor, firmware, controller, policy, calibration and schema versions in an episode manifest.

A simulator or recorded-message replay is acceptable when hardware is unavailable. State which timing, calibration and failure claims cannot be tested without a physical system.

Capture work should also record:

- dropped and late messages;
- clock source and synchronization method;
- expected and measured stream rates;
- operator instructions and intervention points;
- environment and task setup;
- storage consumed and operator or robot time;
- rights and privacy conditions for the recording.

Completion evidence: replay an episode, reconstruct its stream relationships and publish a report covering drops, rates, latency, calibration, versions, cost and known limitations.

## Module 3: formats and conversion

Use formats by responsibility:

| Format | Typical responsibility |
| --- | --- |
| MCAP / rosbag2 | Raw capture and replay |
| LeRobot | Primary PyTorch-oriented training format in this repository |
| RLDS / TFDS | Legacy compatibility with Open X-Embodiment and TensorFlow pipelines when required; the upstream RLDS repository is archived and read-only |
| HDF5 / robomimic | Compatibility with existing imitation-learning pipelines when required |

Build one source-to-target converter first. Add another target only when an exercise or consumer requires it.

The converter must expose:

- field mappings, units and coordinate-frame transformations;
- resampling and synchronization decisions;
- episode segmentation rules;
- video encoding settings;
- source hashes, schema versions and lineage;
- deterministic episode identifiers;
- quarantine reasons for data it cannot interpret safely.

Use a small golden fixture in automated tests. Verify idempotency, schema migration behaviour and atomic publication.

Completion evidence: two clean conversions of the same source produce the same logical episodes and identifiers, while a malformed fixture is rejected without affecting the published snapshot.

## Module 4: robotics data quality

Validate five layers:

- **Structural:** checksums, decoding, required fields, shapes and dtypes.
- **Temporal:** monotonic timestamps, rates, jitter, drops and action-state lag.
- **Physical:** joint limits, impossible velocities, saturation and transform consistency.
- **Visual:** corrupt frames, frozen streams, exposure problems and unexpected resolution changes.
- **Semantic:** task labels, success, action convention, calibration and controller compatibility.

Preserve valid failure episodes. Failed behaviour may be useful; corrupted or semantically unknowable data is different.

Define a small data contract for publication. Each check should pass, reject, quarantine or warn with a reason. Avoid a single quality score that hides why an episode is unsafe.

Completion evidence: inject known defects into copies of a golden episode and prove the appropriate check catches each one before publication or training.

## Module 5: reliable processing

Start this module only after the local converter is repeatable and you can name the bottleneck that requires parallel processing, such as elapsed conversion time, memory pressure or an operational deadline.

Use a bounded public-dataset subset or generated workload before attempting a full large-dataset backfill.

Required properties:

- idempotent workers;
- resumable tasks and uploads;
- deterministic episode identifiers;
- atomic dataset publication;
- incremental recomputation;
- bounded retries and visible dead-letter or quarantine state;
- resource and cost measurements.

Completion evidence: terminate workers during a backfill, restart them and show that the final snapshot has the same canonical episode IDs, counts, logical content hashes and published manifest as a clean run. Logs and operational timestamps may differ. Report throughput, retries and cost.

## Module 6: multi-source collection

Use several simulators, replay producers or physical robots. A simulated fleet is sufficient to learn orchestration, but it cannot validate physical calibration or hardware reliability.

Build:

- authenticated producers;
- resumable uploads with integrity checks;
- versioned manifests and source identity;
- duplicate detection;
- quarantine and review paths;
- deletion and retention procedures;
- dashboards for accepted data, rejection reasons and latency.

Completion evidence: interrupt uploads, submit duplicate and invalid episodes, rotate a producer version and show how the system accepts, rejects, resumes or separates each case. Report capture-to-availability latency and cost per accepted hour.

## Module 7: training-data serving

Separate immutable raw data, canonical datasets, queryable episode metadata, disposable derived artifacts and logical training views.

Build a loader or service with:

- reproducible filters, splits, weights and shuffling;
- deterministic temporal windows;
- episode-level lineage;
- access controls for restricted snapshots;
- parallel media decoding and bounded caching;
- throughput, cache and storage measurements;
- snapshot deprecation and rollback.

Completion evidence: reproduce a sample by identifier, change a dataset mixture without copying media, and show that an older experiment can still resolve its original snapshot.

## Module 8: data-to-learning experiments

Prioritize the parts of [Berkeley CS285](https://rail.eecs.berkeley.edu/deeprlcourse/) that explain observed problems:

1. Behavioural cloning.
2. Distribution shift.
3. Reinforcement-learning basics.
4. Model-based reinforcement learning.
5. Offline reinforcement learning.
6. Exploration and multi-task learning when an experiment requires them.

Use ACT experiments first. Add a small action-conditioned predictor only after the dataset and evaluation pipeline are reproducible.

Choose temporal context and prediction horizons experimentally. Record the frame rate and durations they represent; do not copy a fixed frame window without checking whether it matches the task.

Completion evidence: change one synchronization, coverage or sampling decision and explain the measured effect, uncertainty and plausible mechanism.

## Module 9: operational capstone

Demonstrate one complete path:

```text
capture a raw episode
-> checksum and upload
-> validate and quarantine when required
-> convert to a versioned training format
-> publish a dataset snapshot atomically
-> serve reproducible temporal samples
-> train and evaluate
-> trace failures to source data
-> define the next collection or repair priority
```

The final system should include:

- a recovery runbook and a tested restore or replay procedure;
- dataset, schema and configuration version history;
- rights, access, retention and deletion records;
- measured storage, processing and training costs;
- monitoring for the failures the project has actually encountered;
- a bounded failure injection that proves partial work is not published;
- instructions that let another person reproduce one path without private knowledge.

Completion evidence: select a failed model sample, trace it to its raw recording and transformations, reproduce the sample, then recover the pipeline from an injected processing failure without corrupting the published dataset.

A defensible final claim is: "I built and tested the path that turns raw robot activity into reproducible training data, including the controls needed to understand cost, rights and failure."

## Core references

- [LeRobotDataset documentation](https://huggingface.co/docs/lerobot/en/lerobot-dataset-v3)
- [Hugging Face dataset cards](https://huggingface.co/docs/hub/datasets-cards)
- [ROS 2 tutorials](https://docs.ros.org/en/rolling/Tutorials.html)
- [ROS 2 security](https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Security.html)
- [MCAP with ROS 2](https://mcap.dev/guides/getting-started/ros-2)
- [RLDS](https://github.com/google-research/rlds), archived and read-only since 29 November 2025
- [DROID](https://droid-dataset.github.io/)
- [Open X-Embodiment](https://robotics-transformer-x.github.io/)
- [Modern Robotics](https://modernrobotics.northwestern.edu/)
- [Berkeley CS285](https://rail.eecs.berkeley.edu/deeprlcourse/)
- [SPDX licence identifiers](https://spdx.org/licenses/)
