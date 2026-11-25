# CPU Scheduler Challenge

A menu-driven **CPU scheduling simulator and performance analyzer** developed in 32-bit x86 Assembly using the Irvine32 library. The program lets users create process workloads, simulate scheduling algorithms, inspect Gantt charts and process metrics, compare algorithms, and analyze stored CPU performance results.

## Features

- **Four CPU scheduling algorithms**
  - First Come First Serve (FCFS) — non-preemptive
  - Round Robin (RR) — preemptive, configurable time quantum
  - Priority Scheduling — preemptive; priority `1` is the highest
  - Shortest Remaining Time First (SRTF) — preemptive
- **Process management**
  - Add, update, delete, display, and clear processes
  - Validate process fields and enforce unique process IDs
- **Workload scenario generator**
  - Interactive workload (short bursts)
  - Batch workload (long bursts)
  - Mixed workload (short and long bursts with varied priorities)
- **Simulation output**
  - Gantt chart, including CPU `IDLE` intervals
  - Completion Time (CT), Turnaround Time (TAT), Waiting Time (WT), and Response Time (RT)
  - Average WT, TAT, and RT
- **Algorithm comparison**
  - Run two different algorithms against the same process dataset
  - Compare average WT, TAT, and RT; lower values are better
- **Session history**
  - Store and revisit recent individual runs
  - Keep the latest algorithm comparison
  - The individual-run history is a bounded circular buffer
- **CPU performance analyzer**
  - CPU busy and idle time
  - Total elapsed time and utilization
  - Throughput and context-switch count
  - Waiting-time-based starvation indicator

## Requirements

- Windows
- MASM-compatible 32-bit x86 assembler and linker
- Visual Studio configured for an **x86 / Win32** assembly project
- [Irvine32 library](http://www.kipirvine.com/asm/) (`Irvine32.inc` and the required library files)

> This project uses Irvine32 procedures such as `ReadInt`, `WriteString`, `WriteDec`, `SetTextColor`, `Clrscr`, and `WaitMsg`. It is not a standalone NASM program.

## Build and Run

1. Install Visual Studio with the **Desktop development with C++** workload and configure an x86 MASM project.
2. Install/configure the Irvine32 library.
3. Add the project assembly source file to the MASM project.
4. Configure the Irvine32 include and library directories in the project settings.
5. Build the project using the **Win32 / x86** target.
6. Run the executable in a Windows console.

The source includes `Irvine32.inc`; ensure the include path and Irvine32 linker configuration are correct before building.

## How to Use

1. Launch the program to open the main menu.
2. Select **Process Management** to add or edit processes.
3. For each process, enter:
   - **PID:** 1–999, unique
   - **Arrival Time:** 0–100
   - **Burst Time:** 1–25
   - **Priority:** 1–10 (`1` means highest priority)
4. Choose **Individual Algorithm Simulation** to run one scheduler.
5. Choose **Compare Two Algorithms** to compare two distinct schedulers on the current dataset.
6. Use **Workload Scenario Simulator** to load a predefined workload. Loading a scenario replaces the current process data after confirmation.
7. Open **View Previous Results** to inspect saved runs.
8. Open **CPU Performance Analyzer** to analyze a stored result without rerunning the scheduler.

For Round Robin, specify a time quantum between **1 and 50**.

## Scheduling and Metrics

The simulator records the first CPU start time and completion time for each process.

- **Completion Time (CT):** time at which the process finishes.
- **Turnaround Time (TAT):** `CT - Arrival Time`.
- **Waiting Time (WT):** `TAT - Burst Time`.
- **Response Time (RT):** `First Start Time - Arrival Time`.

Average metrics are displayed to two decimal places.

## Implementation Overview

The program is organized around:

- **Parallel process arrays:** PID, arrival, burst, and priority values are stored by process index.
- **Working-state arrays:** remaining burst, first start, completion, and scheduling flags are initialized for each simulation.
- **Scheduling procedures:** each algorithm dispatches through an algorithm jump table.
- **Gantt segment arrays:** consecutive segments for the same process are merged; idle intervals are represented using a dedicated `IDLE` process marker.
- **Result slots:** individual simulations and comparison results are stored separately from the active process dataset.
- **Console helper procedures and macros:** reusable input validation, formatted tables, centered cells, borders, and colored messages.

## Limits

The source defines these capacity and input limits:

| Item | Limit |
|---|---:|
| Processes per dataset | 20 |
| Gantt segments per run | 600 |
| Individual runs retained | 10 |
| Process ID | 1–999 |
| Arrival time | 0–100 |
| Burst time | 1–25 |
| Priority | 1–10 |
| Round Robin quantum | 1–50 |

If the Gantt segment capacity is exceeded, the chart is marked as truncated. History retains a limited number of individual runs; older entries are replaced as new runs are stored.

## Course Project

**Course:** Computer Organization and Assembly Language (COAL)  
**Project:** CPU Scheduler Challenge  
**Implementation:** 32-bit x86 Assembly with Irvine32

---

*This README describes the functionality and constraints defined in the supplied assembly source.*
