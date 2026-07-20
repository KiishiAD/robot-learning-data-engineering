# Module log template

Copy this file for each completed milestone or meaningful experiment. Keep it short enough to maintain, but specific enough that another person can reproduce the work.

Suggested path:

```text
logs/module-<number>-<short-name>.md
```

Remove sections that genuinely do not apply. Write `Not applicable` with a reason when the omission could otherwise look accidental.

---

# <Module or experiment name>

- **Status:** planned | running | complete | blocked
- **Date:**
- **Author:**
- **Module:**
- **Repository commit:**
- **Question:** What did this work try to find out?
- **Completion test:** What observable result counts as complete?

## Context and claim boundary

Why was this work needed?

What can the result support claiming?

What can it not support claiming? Note limits caused by simulation, sample size, unavailable hardware, missing metadata or other constraints.

## Environment

- Operating system or container image:
- Python and important package versions:
- Compute hardware:
- Robot, simulator or replay source:
- Random seeds:
- Configuration file or command:

Link to a lockfile, environment file or container definition rather than pasting a long dependency list.

## Data identity and lineage

- Dataset or recording:
- Source URL or system:
- Snapshot, revision, manifest or checksum:
- Episode or sample identifiers:
- Robot and sensor configuration:
- Calibration version:
- Controller or policy used during collection:
- Transformations applied:
- Output snapshot or artifact:

Explain how an output sample can be traced back to its source episode.

## Rights, privacy and access

- Data owner or creator:
- Stated dataset licence:
- Code and model licences used:
- Permitted use and redistribution conditions:
- Personal, workplace or sensitive information present:
- Consent or collection restrictions:
- Access controls:
- Retention or deletion requirements:
- Unresolved questions:

Link to the source of each licence or permission. Public access alone is not evidence of permission for every use.

## Method

Describe the smallest sequence needed to reproduce the work.

```bash
# Commands or entry point
```

Record non-default settings and manual decisions. Do not hide failed attempts that changed the final method.

## Verification

List the checks that were actually run.

- [ ] Input identity verified
- [ ] Environment reproduced from pinned configuration
- [ ] Automated checks passed
- [ ] Output read back independently
- [ ] Failure or malformed-input path exercised
- [ ] Completion test met

Evidence:

- Test or validation output:
- Logs:
- Plots, videos or reports:
- Output hashes or snapshot identifiers:

## Results

Report measurements with units and sample sizes.

| Measure | Result | Notes |
| --- | --- | --- |
| Episodes or samples processed | | |
| Accepted | | |
| Rejected or quarantined | | |
| Processing throughput | | |
| Capture-to-availability latency | | |
| Evaluation success | | |

Add or remove rows according to the module. Do not turn a smoke test into a performance claim.

## Cost and capacity

Record the costs that could affect a design decision.

- Currency:
- Pricing date:
- Provider or vendor:
- Service, hardware or labour tier:
- Region:
- Price source:
- Labour-rate basis:

| Cost or resource | Quantity | Price or basis | Estimated cost |
| --- | ---: | ---: | ---: |
| Human collection or review time | | | |
| Robot or simulator time | | | |
| Compute | | | |
| Raw storage | | | |
| Derived storage | | | |
| Network transfer or egress | | | |
| Training and evaluation | | | |

Useful derived measures may include cost per recorded hour, cost per accepted hour, storage per episode and conversion time per hour of data. Local experiments can state `negligible` or use rough estimates, but should name what was excluded.

## Failures and operational observations

- What failed?
- Was any partial or invalid output visible to consumers?
- What was retried, quarantined or discarded?
- Could the run resume safely?
- Which metric or log exposed the problem?
- What recovery step was tested?

Separate expected task failures from corrupted or uninterpretable data.

## Findings

State what the evidence showed in plain language.

1.
2.
3.

## Decision and next step

- **Decision:** continue | revise | stop | investigate
- **Reason:**
- **Next smallest useful step:**
- **New risks or unanswered questions:**
