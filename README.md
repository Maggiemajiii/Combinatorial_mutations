# Combinatorial_mutations
Protein modeling for combinatorial mutations

## Problem

Predicting the phenotype of combinatorial mutations based on single mutation data.
Features: Height, Hair Color

## Dataset
* Source: [Add link to dataset source]

## Federation -> clients

### Data Preprocessing

* reducing dimensionality

### Model Structure

* Baseline model: Linear Regression
* Ensemble model: Random Forest
* Finetuning model: GrridsearchCV


## Tools

## Implementation

## Usage

* Discrete Phenotype
* Continuous Phenotype

## Delivery Flowchart

```mermaid
flowchart TD
	A[Problem: Predict phenotype of combinatorial mutations] --> B[Dataset: Single mutation data + features]
	B --> C{Training mode}
	C -- Federated --> S1[Server: Broadcast preprocessing + training config]
	C -- Centralized --> D[Centralized pipeline]

	subgraph Clients[Federated clients]
		direction TB
		E[Ingest local data] --> F[Preprocess: QC, normalize, encode]
		F --> G[Dimensionality reduction (e.g., PCA)]
		G --> H{Model}
		H -- Baseline --> H1[Linear Regression]
		H -- Ensemble --> H2[Random Forest]
		H -- Tuning --> H3[GridSearchCV]
		H1 --> I[Local evaluation]
		H2 --> I
		H3 --> I
		I --> J[Export local model + metrics]
	end

	S1 --> E
	J --> S2[Server: Secure aggregation]
	S2 --> N[Global model selection]

	D --> K[Preprocess + Dimensionality Reduction]
	K --> L{Train models}
	L --> LR[Linear Regression]
	L --> RF[Random Forest]
	L --> GS[GridSearchCV]
	LR --> M[Evaluate]
	RF --> M
	GS --> M
	M --> N

	N --> O{Phenotype type}
	O -- Discrete --> P[Classification metrics: Accuracy, F1]
	O -- Continuous --> Q[Regression metrics: R^2, MAE]
	P --> R[Predict combinatorial mutations]
	Q --> R
	R --> T[Deliver: Save artifacts, document usage, CLI/API]

	B -. targets .-> U[Phenotypes: Height, Hair Color]
	U -. informs .-> H
```

For a standalone version, see `docs/delivery_flowchart.mmd`.


