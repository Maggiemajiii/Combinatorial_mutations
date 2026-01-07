# Protein Modeling for Combinatorial Mutations

## 📊 Project Flowchart

```mermaid
graph TD
    %% --- Phase 1: Data Acquisition & Splitting ---
    subgraph Dataset_Prep [1. Dataset Construction]
        direction TB
        RawData[("<b>Raw Data Sources</b><br>(ClinVar / AWS Open Data)<br>")]
        
        Filter[Data Cleaning & Splitting]
        RawData --> Filter
        
        Filter -->|Subset A: Single Mutations| ClientData1[Client 1 Data]
        Filter -->|Subset B: Different Single Mutations| ClientData2[Client 2 Data]
        Filter -->|Subset C: Combinatorial Mutations| TestData["<b>Test Data</b><br>(Hold-out for Validation)"]
    end

    %% --- Phase 2: Preprocessing ---
    subgraph Preprocessing [2. Feature Engineering]
        direction TB
        Seq["Protein Sequences"]
        
        Embed["<b>Foundation Model Embedding</b><br>(ESM / BioNeMo)<br>"]
        DimRed["<b>Dimensionality Reduction</b><br>(PCA / t-SNE)<br>"]
        
        ClientData1 --> Seq --> Embed --> DimRed
        ClientData2 --> Seq --> Embed --> DimRed
    end

    %% --- Phase 3: Federated Learning System ---
    subgraph FL_System [3. Federated Learning (NVFlare)]
        direction TB
        Server((<b>NVFlare Server</b><br>Global Aggregation))
        
        subgraph Client_Node_1 [Client 1]
            Training1[Train Local Model]
            Tuning1["Hyperparameter Tuning<br>(GridSearchCV)"]
        end
        
        subgraph Client_Node_2 [Client 2]
            Training2[Train Local Model]
            Tuning2["Hyperparameter Tuning<br>(GridSearchCV)"]
        end

        DimRed --> Training1 & Training2
        
        %% The Federated Cycle
        Server -->|Global Weights| Training1 & Training2
        Training1 & Training2 -->|Local Updates| Server
        Tuning1 -.-> Training1
        Tuning2 -.-> Training2
    end

    %% --- Phase 4: Model Architectures ---
    subgraph Model_Zoo [4. Models]
        direction LR
        Base["<b>Baseline</b><br>Linear Regression"]
        Ensemble["<b>Ensemble</b><br>Random Forest"]
    end
    
    Training1 -.-> Base & Ensemble
    Training2 -.-> Base & Ensemble

    %% --- Phase 5: Usage / Output ---
    subgraph Usage [5. Phenotype Prediction]
        direction TB
        FinalModel(Final Global Model)
        Server --> FinalModel
        
        InputTest(Test Input: Combinatorial Mutations) --> FinalModel
        
        FinalModel --> Discrete["<b>Discrete Output</b><br>(Pathogenicity / Functionality)<br><i>'Hair Color' equivalent</i>"]
        FinalModel --> Continuous["<b>Continuous Output</b><br>(Stability Score / Fluorescence)<br><i>'Height' equivalent</i>"]
    end

    style FL_System fill:#f9fbe7,stroke:#827717,stroke-width:2px
    style Usage fill:#e3f2fd,stroke:#1565c0,stroke-width:2px