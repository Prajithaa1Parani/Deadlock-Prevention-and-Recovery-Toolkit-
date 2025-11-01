# Deadlock Toolkit - Interactive Simulator

A web-based tool for simulating and visualizing deadlock detection, prevention and recovery using Banker's Algorithm and Resource Allocation Graphs.

## Overview
The Deadlock Toolkit provides an interactive interface to:
- Simulate resource allocation scenarios with multiple processes and resources
- Detect potential deadlocks using Banker's Safety Algorithm 
- Visualize resource allocation and requests through dynamic graphs
- Test resource requests and prevent unsafe allocations
- Recover from deadlock situations through process termination

## Flowchart
```mermaid
graph TD
    A[Start] --> B[Set Process & Resource Count]
    B --> C[Initialize Matrices]
    C --> D[Fill Max Claims & Current Allocation]
    D --> E[Compute Need Matrix]
    E --> F{Safety Check}
    F --> |Safe| G[Allow Resource Requests] 
    F --> |Unsafe| H[Prevent New Allocations]
    G --> I[Update Matrices]
    I --> F
    H --> J[Recovery Options]
    J --> K[Abort Process]
    K --> I