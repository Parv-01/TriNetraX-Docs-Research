# TrinetraX - Simple Project Overview

**Tagline:** Observe. Reason. Act.

## 1. What We Are Building

TrinetraX is a **vendor-neutral compute intelligence and agentic optimization framework**.

It helps researchers and system administrators understand what is happening across GPUs and other compute devices, identify performance problems and recommend or safely apply improvements.

We will start with ** NVIDIA GPUs**, then expand to AMD, Intel, CPUs, NPUs, TPUs and other accelerators.

The goal is not to build another benchmark tool. The goal is to **measure performance, understand the cause of problems and help improve performance**.

## 2. Architecture at a Glance

```text
Researcher / ML Workload
          |
          v
1. User Interface - Python SDK + CLI
          |
          v
2. Control & Session Manager
          |
          v
3. Hardware Abstraction Layer
          |
          v
4. Data Collection & Profiling
          |
          +---------------------------+
          |                           |
          v                           v
5. Benchmark Integrations       NVIDIA / Linux Tools
          |
          +---- AI-PERF adapter
          |
          v
6. Unified Performance Data
          |
          v
7. Diagnosis & Performance Models
          |
          v
8. Agentic Intelligence Framework
          |
          v
9. Policy, Safety & Action Engine
          |
          v
10. Results, Memory & Feedback
```

These are **10 logical layers**, not necessarily 10 separate services. Each layer has a clear responsibility.

## 3. What Each Layer Does

| Layer | Responsibility | In simple English |
|---|---|---|
| 1. User Interface | Python API and command-line tools | Lets people use TrinetraX easily. |
| 2. Control & Sessions | Configuration, jobs, profiling sessions | Organizes what should run and when. |
| 3. Hardware Abstraction | Common device model and vendor backends | Gives different hardware a consistent interface while preserving vendor-specific features. |
| 4. Data Collection & Profiling | GPU/CPU/memory/power/timing telemetry | Collects evidence about what the system is doing. |
| 5. Benchmark Integrations | Adapters for tools such as AI-PERF | Runs or imports results from specialized benchmark tools. |
| 6. Unified Performance Data | Normalized schemas and workload profiles | Puts measurements into one consistent format. |
| 7. Diagnosis & Performance Models | Rules and statistical models | Finds likely bottlenecks and explains the evidence. |
| 8. Agentic Intelligence | Planner, tool selection, reasoning, memory | Decides what to investigate and which improvement to consider. |
| 9. Policy, Safety & Action | Permissions, validation, approval, rollback | Prevents risky or unauthorized changes. |
| 10. Results, Memory & Feedback | Reports, experiment history, measured outcomes | Records what actually helped and what did not. |

## 4. Where AI-PERF Fits

**AI-PERF is a specialized AI-inference benchmarking tool. TrinetraX is the wider intelligence framework.**

AI-PERF measures inference behavior such as request/token throughput and latency, including time to first token (TTFT) and inter-token latency (ITL). TrinetraX combines those results with hardware and system telemetry to help explain *why* performance is good or poor.

```text
AI inference server
       |
       v
AI-PERF measures inference performance
       |
       v
TrinetraX AI-PERF Adapter
       |
       v
Validate + normalize benchmark results
       |
       +--------------------+
       |                    |
       v                    v
GPU/CPU telemetry       Workload profile
       |                    |
       +---------+----------+
                 |
                 v
        Diagnosis and agent
                 |
                 v
     Safe recommendation / action
                 |
                 v
       Measure performance again
```

## 5. The Agentic Framework Is Central

The agent is not just a chatbot. It is a **bounded performance engineer** that uses real telemetry, tools, rules and measured results.

```text
OBSERVE
  Collect GPU, CPU, memory, system and benchmark data
      |
      v
UNDERSTAND
  Build a picture of the workload and likely bottlenecks
      |
      v
PLAN
  Choose useful diagnostic checks or candidate optimizations
      |
      v
CHECK SAFETY
  Validate permissions, risk, expected impact and rollback
      |
      v
ACT
  Run only an approved, registered action
      |
      v
MEASURE
  Compare results with the baseline
      |
      v
LEARN
  Save what worked, what failed and under which conditions
      |
      +-----------> repeat when useful
```

### Agent components

- **State builder:** turns raw telemetry into a structured snapshot.
- **Diagnosis engine:** uses deterministic rules and statistical models first.
- **Planner:** chooses the next diagnostic step or candidate action.
- **Tool interface:** calls only registered TrinetraX tools.
- **Performance memory:** stores hardware, workload, action and measured outcome.
- **Optional local SLM:** helps with complex diagnosis and planning; basic functions do not depend on it.
- **Policy engine:** checks every proposed action before execution.
- **Evaluator:** checks whether an action improved latency, throughput, energy use, or another declared objective.

### Three operating modes

1. **Advisory:** observes and recommends only. This is the initial mode.
2. **Approval-based:** proposes an action and waits for user approval.
3. **Controlled autonomous:** executes only allowlisted, reversible actions within strict limits.

The agent must never receive unrestricted root access or arbitrary shell execution. The model proposes structured actions; normal program code validates and executes them.


## 6. Security and Reliability Rules

- Run without root by default.
- Keep privileged operations out of the main Python process.
- Use a small restricted helper only if a future feature truly requires elevated permissions.
- Never use `shell=True` with user-controlled input.
- Validate configurations, paths, URLs, subprocess arguments and imported artifacts.
- Keep prompts, datasets, model weights and application content out of telemetry by default.
- Use timeouts, cancellation, clear errors and process isolation for external tools.
- Make every recommendation explain its evidence and confidence.
- Require policy checks and rollback plans for actions that change system state.
- Test normal behavior, failure behavior, security boundaries and performance overhead.

## 7. Core Idea

**AI-PERF measures inference performance. TrinetraX connects those measurements to hardware and system evidence, diagnoses likely causes and uses a safe agentic framework to recommend or eventually apply improvements.**
