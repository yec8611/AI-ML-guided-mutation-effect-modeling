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
