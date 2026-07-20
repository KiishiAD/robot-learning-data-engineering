# Data Engineering for Robot Learning

Robot-learning resources are largely written for robotics researchers. This repository documents my route into the field from a data-engineering background, organized as small practical modules.

The focus is the data beneath robot policies and world models: collection, synchronization, validation, conversion, storage, lineage and training access.

This is a structured record of my learning, not a comprehensive robotics course.

## Start here

1. Read the [learning pathway](ROADMAP.md).
2. Open [Module 0: Understanding one robot episode](intro-to-robot-learning/README.md).
3. Work through the [ALOHA dataset notebook](intro-to-robot-learning/aloha_transfer_cube_dataset.ipynb).

## Learning path

| Module | Focus | Status |
| --- | --- | --- |
| 0 | Inspect, train and evaluate on ALOHA | In progress |
| 1 | Robot-data semantics and compatibility| Planned |
| 2 | Raw capture with ROS 2 and MCAP| Planned |
| 3 | Dataset formats and conversion | Planned |
| 4 | Robotics data quality | Planned |
| 5 | Reliable and resumable processing | Planned |
| 6 | Multi source collection | Planned |
| 7 | Versioned training-data serving | Planned |
| 8 | Data-to-learning experiments | Planned |
| 9 | End-to-end capstone | Planned |

## Repository structure

```text
.
├── README.md                              # Purpose and navigation
├── ROADMAP.md                             # Full engineering learning pathway
└── intro-to-robot-learning/
    ├── README.md                          # Module 0 objectives and completion test
    ├── Resources.md                       # Curated references with reasons
    └── aloha_transfer_cube_dataset.ipynb  # Dataset inspection notebook
```

The structure will grow one completed module at a time. Each module should contain a short explanation, a practical artifact and a clear completion test.
