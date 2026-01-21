# Estimating Software

Notice on Usage: This repository is for demonstration and educational purposes only. All rights are reserved. No permission is granted for commercial use, redistribution, or modification of the source code. Recruiters and hiring managers are explicitly permitted to review the code for the purpose of candidate evaluation.

## Project Vision
This is a "Field-to-Firmware" solution to bridge the gap between onsite operational experience and high-performance software engineering.

By shifting from deterministic "single-point" estimates to probabilistic risk modeling, this tool identifies the confidence intervals of a project's cost, protecting margins against material and labor volatility.

## System Architecture

The application is designed as an Offline-First Monolith to ensure full functionality on remote job sites with zero internet connectivity.

  - Frontend: Angular 17 (Standalone) – Interactive dashboard with D3.js visualizations.

  - Backend: Java 21 (Spring Boot 3) – Orchestrates business logic and the simulation pipeline.

  - Database: H2 (File-based) – Persistent local storage located in the ./data/ directory.

  - Acceleration: NVIDIA CUDA – High-speed parallel kernel for 10,000+ Monte Carlo iterations.

  - Bridge: JNI (Java Native Interface) – Efficiently connects the JVM to unmanaged C++ memory.

## Project Organization 

```
EstimatingProject/
├── src/main/java/          # Spring Boot Backend (REST API, Services)
├── src/main/resources/     # Application config & H2 DB setup
├── src/main/native/        # C++/CUDA Kernels & JNI Bridge logic
├── frontend/               # Angular Source Code
├── docker-compose.yml      # GPU-enabled container orchestration
└── README.md               # Project documentation
```

## Statistical Engine: Beta-PERT & Monte Carlo
The engine combines two statistics concepts to provide a better "guess" than just one solution alone. 

The input: Beta-PERT Distribution: 
For every line item (e.g., Concrete, Steel, Labor), the user provides three values: Optimistic (O), Most Likely (M), and Pessimistic (P). The engine calculates:

Expected Mean (μ):
μ=6O+4M+P​

Standard Deviation (σ):
σ=6P−O​

The Engine: Monte Carlo Simulation
While Beta-PERT defines the risk of a single item, Monte Carlo determines the risk of the whole project.

    Random Sampling: The CUDA kernel generates 10,000 unique "scenarios." In each scenario, it picks a random value for every line item based on its Beta-PERT curve.

    Aggregation: It sums these random values to get a "Total Project Cost" for that specific iteration.

    Statistical Analysis: After 10,000 runs, the engine sorts the results to find the P10 (Aggressive), P50 (Median), and P90 (Safe) bid amounts.


## Technical Challenges
I am currently documenting and solving the following engineering hurdles:

  - JNI Memory Latency: Reducing the overhead of copying massive data sets between Java and C++.

 - Warp Divergence: Optimizing the CUDA kernel to ensure all 32 threads in a warp follow similar execution paths during random sampling to maximize GPU throughput.

-  Offline ACID Compliance: Tuning the H2 database to prevent corruption during unexpected power loss on remote job-site laptops.

  ## Project Roadmap
  
  This project is currently in Initial Development.
  
[ ] Phase 1: Foundation

    [ ] Initialize Spring Boot / Angular structure.

    [ ] Configure H2 local file persistence.

[ ] Phase 2: Simulation Engine

    [ ] Implement Beta-PERT math logic (Java CPU Fallback).

    [ ] Generate JNI header files for native communication.

[ ] Phase 3: GPU Acceleration

    [ ] Develop .cu kernel for parallel Monte Carlo iterations.

    [ ] Implement memory management for Host-to-Device transfers.

[ ] Phase 4: Data Visualization

    [ ] Build Angular "S-Curve" and Histogram risk charts using D3.js.

    [ ] Secure local vault for future QuickBooks API integration.
    
    ​
