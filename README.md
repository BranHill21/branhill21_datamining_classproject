# Fairness-Aware Citation Ranking: Mitigating Structural Age Bias in Academic Networks

This repository contains a comprehensive data mining and graph analysis project designed to expose and mitigate structural "age bias" in large-scale academic citation networks. Utilizing the **ACM-Citation-network-V12** dataset (6.6 million nodes, 13.1 million edges), this project explores how standard graph algorithms inherently favor historical papers and proposes a Fairness-Aware PageRank solution to restore visibility to modern innovation.

## Repository Structure

The project was developed in phases and is organized into the following Jupyter Notebooks:

* **`Project1_datamining.ipynb`**: The initial exploratory data analysis (EDA) pipeline. Features memory-safe parsing of the multi-gigabyte JSON dataset, Bow-tie decomposition of the graph (IN, OUT, GSCC, OTHER), and baseline structural metric calculations.
* **`236004960_project_checkpoint2.ipynb`**: The formal project proposal outlining the core research questions, algorithmic methodology, and specific evaluation metrics (Spearman Rank, Demographic Parity, Kendall-Tau, Modularity).
* **`236004960_project_final_deliverable.ipynb`**: The final curated narrative. This notebook contains the complete execution of the research questions, the implementation of the external Fairness-Aware PageRank algorithm, sensitivity testing, and the deep-dive analysis into the "Scale-Up Problem" of graph recommendations.

## Research Overview & Key Findings

The final deliverable tackles three interconnected research questions:

### RQ1: The Matthew Effect and Structural Prestige
* **Focus:** Global vs. Local importance (PageRank vs. In-Degree) and temporal bias.
* **Finding:** The graph is highly fragmented (only 26.45% of nodes are in the Giant Strongly Connected Component). A severe "Matthew Effect" exists where papers from the 1950s hold **25 times higher** average PageRank than papers from the 2020s. Standard centrality metrics mistake "age" for "importance."

### RQ2: Community Detection & Semantic Purity
* **Focus:** Mapping structural silos using Louvain Modularity.
* **Finding:** The network yields a highly modular structure (Modularity > 0.76). Manual validation of Venue/Field of Study (FOS) metadata confirms these clusters represent isolated semantic research fields (e.g., Computer Vision vs. Hardware Architecture). Traditional random walks get trapped within these historical semantic boundaries.

### RQ3: Fairness-Aware Citation Ranking (External Technique)
* **Focus:** Implementing a Demographic-Aware teleportation vector to penalize historical entrenchment.
* **The "Goldilocks" Success:** On a 100,000-node subgraph, applying a slight bias weight of 1.2 to modern papers (>= 2018) successfully shifted their top-100 representation from 6.00% to **42.00%** (perfect Demographic Parity), while maintaining high semantic utility (Kendall-Tau = **0.7834**).
* **The Scale-Up Limit (Hub Gravity):** When testing this exact successful intervention on the massive 6.6-million node graph, it failed mathematically. Extreme testing proved that **Probability Dilution** and the **Hub Gravity** of 1980s "super-hubs" completely overpower global teleportation biases at a macro scale.

## Technical Implementation

The pipeline is built for high-performance graph mining using the `igraph` library's C-core to handle millions of edges within standard memory constraints.

* **Memory-Safe Parsing:** Custom streaming logic to load massive JSON logs line-by-line while handling inconsistent schema types and enforcing metadata cleanliness.
* **Algorithm Execution:** PageRank, HITS, In-degree, Louvain Community Detection, and Personalized PageRank (PPR).
* **Evaluation Metrics:** Spearman Rank Correlation, Kendall-Tau Distance (Ranking Utility), and Demographic Parity (Algorithmic Fairness).

## Getting Started

### Prerequisites

The analysis is optimized for Python 3.9+ and requires the following libraries:
* `python-igraph`
* `pandas`
* `numpy`
* `scipy`
* `matplotlib`

### Usage Instructions

1.  **Environment Setup:**
    It is highly recommended to run these notebooks in **Google Colab** (T4 GPU or High-RAM instance) to prevent Out-Of-Memory (OOM) errors when calculating eigenvectors on 13.1 million edges. Install the primary dependency at the start of your session:
    ```bash
    pip install python-igraph pandas numpy scipy matplotlib
    ```

2.  **Data Acquisition:**
    The notebooks include logic to automatically download and extract the `ACM-Citation-network-V12.zip` dataset. Ensure your environment has approximately 5GB of free disk space for the compressed and extracted files.

3.  **Execution Flow:**
    For the complete narrative and final results, open and run **`236004960_project_final_deliverable.ipynb`**. 
    * *Note:* The final execution cell running Personalized PageRank on the entire 6.6M node network typically computes in 15–30 seconds via the PRPACK backend.

## Conclusion
By bridging data mining techniques with algorithmic fairness literature, this project demonstrates that while modifying the global teleportation vector is an elegant way to inject equity into local subgraphs, true systemic fairness in billion-scale recommendation engines requires overcoming the deep historical gravity of citation DAGs.
