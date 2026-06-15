# Protein Secondary Structure Prediction Project 
-> Protein secondary structure prediction is a fundamental challenge in computational biology because secondary structures form the bridge between a protein’s linear amino acid sequence and its functional three-dimensional structure. 

-> The three main structural elements—α-helices (H), β-strands (E), and random coils (C)—are stabilized by non-covalent interactions between residues. While α-helices are primarily governed by local hydrogen bonding, β-sheets often arise from interactions between residues that may be far apart in the primary sequence. 

-> Protein sequences also evolve over time, and residues important for structural stability or function tend to remain conserved or co-evolve with interacting partners. 

-> To capture this evolutionary signal, position-specific scoring matrices (PSSMs) derived from multiple sequence alignments are commonly used as input features for structure prediction models. 

-> Deep learning approaches such as Recurrent Neural Networks (RNNs) are well suited for modelling biological sequences because they maintain a hidden state that propagates information across the sequence, enabling the capture of dependencies between distant residues. 

-> However, traditional RNNs often suffer from the vanishing gradient problem. This limitation can be addressed using gated architectures such as Gated Recurrent Units (GRUs), which regulate information flow and enable learning long-range relationships. 

### In this project, a Bidirectional GRU (Bi-GRU) network is implemented, trained on PSSM features, and by processing sequences in both forward and backward directions, it is able to capture local motifs and long-range interactions for accurate Q3 secondary structure prediction.

# Materials/methods

1.	Dataset and Preprocessing:
 
The model was trained and evaluated using protein sequences represented by Position-Specific Scoring Matrices (PSSM), where each amino acid residue is encoded as a 20-dimensional vector, capturing the evolutionary conservation and substitution probabilities derived from Multiple Sequence Alignment (MSA). 
To ensure data alignment for batch processing, padding was used such that features (PSSM) were padded with 0.0, while structural labels (DSSP) were padded with -1. During the training phase, a binary masking approach was applied via the ignore_index parameter in the Cross-Entropy loss function, so only biological residues contributed to the gradient updates. 

2.	Architecture

In this model, there are two parts, one is the “encoder” , featuring two stacked layers of Bidirectional Gated Recurrent Units (Bi-GRUs). Unlike sliding-window models, the Bi-GRU maintains a "hidden state" that captures long-range dependencies across the sequence. The bidirectional nature also allows the model to process information from both the N-terminal and C-terminal directions simultaneously, mimicking the global interactions that stabilize secondary structures. The second part is the “decoder” which consists of a linear classifier that projects these high-dimensional states into three-state logits (H, E, C). 

3. Training and Optimization

To optimise the model, firstly the 2 Bi-GRU layers were used followed by implementing a multi-layer perceptron (instead of the single linear classifier from before) which also comprises a ReLU activation and Dropout (0.3) to prevent vanishing gradients and overfitting, respectively. The model also uses a Cross-Entropy Loss with class weights to address the natural imbalance between Helices, Strands, and Coils. Optimization was performed using the Adam optimized, paired with a learning rate scheduler to ensure stable convergence as the model reached the global loss minimum.

