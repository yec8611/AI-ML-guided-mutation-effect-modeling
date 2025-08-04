# AI-ML-guided-mutation-effect-modeling
Repository of modeling enzyme mutation effect using advanced ML including pLM, GNN and Alphafold

This repository is for single mutation effect prediction using advanced ML models. Traditional deep mutation scanning is experimentally labrious and hardly sufficient to cover all mutation space. Recent advancement in machine learning has endorsed usage of transformer-based protein language models for generating context-aware rich representation of amino acid sequences. We used a test dataset from a [CYP2C9 study](https://www.cell.com/ajhg/fulltext/S0002-9297(21)00269-X?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS000292972100269X%3Fshowall%3Dtrue), which contains 6142 missense mutations on CYP2C9, a critial metabolic enzyme heavilly involved in drug metabolism and pharmacogenomics. We trained several classical regression models using embeddings from ESM2 for each mutant sequence, as well as fine-tuned using Low Rank Adaptation, and utilized structural information from Alphafold2 to build an equivariant graph neural network (EGNN).

## Classical ML with ESM2-650M embeddings yield moderate prediction power
<img width="1489" height="1190" alt="image" src="https://github.com/user-attachments/assets/b2dbc1d3-04f8-4d06-895d-fd854a11f463" />
The best model is with XGBoost with spearman correleation of 0.682.

## Regression head only (MLP) vs LoRA fine tuning
<img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/7bda45f4-7f2a-4eac-b70c-435c0014f77a" />

Rich embeddings from ESM2-650M already enable pattern learning for this dataset on enzymatic activity; LoRA further boosted performance (ρ = 0.65).

## Combine structrual information with ESM2 embeddings
If considering the role of 3D structure in single mutation's impact on protein function, a straightforward approach is to use graph neural network where each residue is represented by a node with connected edges to other nearby nodes encoding 3D relationship. We used graph attention mechanism (GAT) to weigh the importance of different neighbors for each node. The node feature is comprised of one-hot encoded residue identity and positional encoding of the mutation. Using robust group cross validation, we got an impressive performance of prediction (spearman's ρ = 0.46), only with information from Alphafold2-predicted structure and mutation identity. Next, we injected ESM2-650M embeddings into node features to make use of the context-rich information for each mutant, and achieved prominently improved spearman correlation of 0.71. Further, we optimized the feature set by computing the embedding difference between wild type and mutant (delta_emb = mut_emb - wt_emb), and utilized a more suitable loss (Huber + Soft Spearman) to accommodate the training goal (modeling mutation effect ranking). This optimized workflow increased ρ to 0.76 (the best performance for this dataset). On the other hand, we tested a recently developed equivariant graph network architecture (EGNN) on the CYP2C9 dataset. The model's output doesn't change if the input structure is rotated or moved in space, allowing it to learn physical patterns. The core concept here is using E_GCL layers (equivariant graph convolutional layers) to model protein coornidates as the training process goes, given a fixed Alphafold2 strucutrue in the beginning. Also combining ESM2 embeddings, we achieved a spearman's rho of 0.75, slightly lower than the GAT approach. The EGNN might need more data to excel at the muation effect prediction task.

<img width="450" height="450" alt="image" src="https://github.com/user-attachments/assets/3023e6c1-3fce-4cdb-90c5-b332d8d1591a" />
<img width="501" height="440" alt="image" src="https://github.com/user-attachments/assets/9e343b40-c97f-4eef-beac-b4cd17965d2a" />


