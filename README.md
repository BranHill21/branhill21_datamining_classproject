# Fairness-Aware Citation Ranking: Mitigating Structural Age Bias in Academic Networks

**Start here: `main_notebook.ipynb`**

**[2 Minute Project Video](https://www.youtube.com/watch?v=aTgQlPBKzqw)**

## Project Overview
This project exposes and mitigates structural "age bias" in large-scale academic citation networks. Billions of dollars are spent on R&D, but the search engines used to discover breakthroughs often rely on classic graph algorithms that are structurally rigged against new ideas. Using the **ACM-Citation-network-V12** dataset (6.6 million nodes, 13.1 million edges), this analysis proves that standard global centrality metrics mistake "age" for "importance." To fix this, we implement a Demographic-Aware PageRank solution to bridge disconnected research silos and restore visibility to modern innovation.

## Research Questions
This project set out to answer three core questions:
1. **The Matthew Effect:** How does the global vs. local definition of "importance" shift when evaluating isolated graph components versus the Giant Strongly Connected Component (GSCC)?
2. **Community Purity:** Do isolated structural communities map strictly to semantic research fields, or do they represent geographical/institutional silos?
3. **Fairness Intervention:** Can a Fairness-Aware PageRank algorithm mitigate structural "age bias" without significantly degrading the semantic relevance of the resulting citation recommendations?

## Data 
* **Dataset:** ACM-Citation-network-V12
* **Source:** AMiner Open Data (https://opendata.aminer.cn/dataset/ACM-Citation-network-V12.zip)
* **Preprocessing:** A memory-safe custom streaming logic was implemented to parse the multi-gigabyte JSON dataset line-by-line. This handled inconsistent schema types (e.g., `venue` as string vs. dictionary) and enforced metadata cleanliness by dropping nodes without valid publication years to prepare for the demographic-aware algorithms.

## Results Summary
The baseline exploratory analysis revealed a massive structural bias: papers from the 1950s held **25 times higher** average PageRank than papers from the 2020s. 

By implementing an external Fairness-Aware PageRank with a biased teleportation vector of 1.2 on a 100k-node subgraph, we successfully elevated modern research representation from 6.00% to **42.00%** (achieving Demographic Parity) while maintaining high ranking utility (Kendall-Tau = **0.7834**). However, extreme-scale testing on the full 6.6-million node graph revealed a "Scale-Up Problem," proving that Probability Dilution and the Hub Gravity of 1980s super-hubs overpower global teleportation biases at a macro scale.

## Reproducing this Work
This project was built and executed in Google Colab. 
1. Clone this repository.
2. Review the `requirements.txt` to ensure your environment matches. It is highly recommended to run this in a Google Colab T4 GPU or High-RAM instance to prevent OOM errors during the parsing of 13.1 million edges.
3. Open `main_notebook.ipynb` and run the cells sequentially. The data downloading and extraction logic is built directly into the first initialization cell.

### Key Dependencies
* Python 3.12.13
* `python-igraph` (1.0.0)
* `pandas` (2.2.2)
* `numpy` (2.0.2)
* `scipy` (1.16.3)
* `matplotlib` (3.10.0)

## Repository Structure
```text
.
├── main_notebook.ipynb         # The final curated notebook and primary deliverable
├── checkpoints/                # Folder containing the progression of work over the semester
│   ├── checkpoint_1.ipynb      # Initial EDA, parsing logic, and structural analysis
│   └── checkpoint_2.ipynb      # Formal project proposal and algorithmic methodology
├── requirements.txt            # Exported Colab environment dependencies
├── .gitignore                  # Git ignore file
└── README.md                   # Project documentation
