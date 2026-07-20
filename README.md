# Data engineering for robot learning

Robot-learning material is often written for robotics researchers. This repository documents a practical route into the field from a data-engineering perspective, organized as small modules with working artifacts and completion evidence.

The focus is the data beneath robot policies and world models: collection, synchronization, validation, conversion, storage, lineage, cost and training access.

The repository began as a personal learning record. The explanations, templates and completion tests are written so that other learners and practitioners can reproduce the work, adapt it to their environment and see where a result has limitations.

This is not a comprehensive robotics course. It teaches robotics and machine-learning concepts when they become necessary to build or judge a data system.

## Start here

1. Read the [learning pathway](ROADMAP.md).
2. Open [Module 0: understanding one robot episode](intro-to-robot-learning/README.md).
3. Work through the [ALOHA dataset notebook](intro-to-robot-learning/aloha_transfer_cube_dataset.ipynb).
4. Use the [module log template](MODULE_LOG_TEMPLATE.md) to record reproducibility, evidence, rights, cost and failures.

## How progress works

There are no calendar targets. Move forward when the module's completion test is supported by an artifact and evidence that another person can inspect.

Someone may substitute a different robot, simulator, dataset or infrastructure tool. The module log should explain the substitution, why it was reasonable and which claims no longer transfer to the original setup.

Scale is conditional. Distributed processing, fleet collection and service operations begin only after a smaller pipeline works and measured volume, latency or reliability requirements justify the extra machinery.

## Learning path

| Module | Focus | Status |
| --- | --- | --- |
| 0 | Understand ALOHA data, evaluate ACT, train one baseline and run a controlled data experiment | In progress |
| 1 | Robot-data semantics and compatibility | Planned |
| 2 | Raw capture with ROS 2 and MCAP | Planned |
| 3 | Dataset formats and deterministic conversion | Planned |
| 4 | Robotics data quality | Planned |
| 5 | Reliable and resumable processing | Planned |
| 6 | Multi-source collection | Planned |
| 7 | Versioned training-data serving | Planned |
| 8 | Data-to-learning experiments | Planned |
| 9 | Operational capstone | Planned |

## Repository structure

```text
.
├── README.md                              # Purpose and navigation
├── ROADMAP.md                             # Evidence-gated engineering pathway
├── MODULE_LOG_TEMPLATE.md                 # Reproducible record for each milestone
└── intro-to-robot-learning/
    ├── README.md                          # Module 0A objectives and completion test
    ├── Resources.md                       # Curated references with reasons
    └── aloha_transfer_cube_dataset.ipynb  # Existing dataset inspection notebook
```

The repository will grow one proven module at a time. A completed module should contain a short explanation, a working artifact, verification evidence and a clear account of what remains uncertain.
