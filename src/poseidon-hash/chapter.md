# Poseidon hash for ZK applications

Many use cases of practical computational integrity proof systems such as SNARKs, STARKs, Bulletproofs, involve *proving the knowledge of a preimage under a certain cryptographic hash function*, which is expressed as a circuit over a large prime field. However, due to the incompability of those hash functions (SHA-256 in Zcash cryptocurrency), the ZKP for such a problem caused a huge computational penalty. Traditional hash functions are simply not well-suited in the context of ZKP (they mainly optimize performance on certain architectures). Therefore, new hash functions that performs natively with ZKP systems are needed.

In 2021, Grassi et al. {{#cite GKRRS21}} introduced \\(\mathsf{Poseidon}\\), a cryptographic hash function that works natively with finite field objects - the primitives of arithmetic circuits, therefore friendly with ZK applications. \\(\mathsf{Poseidon}\\) uses \\(8\\) times fewer constraints per message bit than Pedersen Hash. This whole tutorial about Poseidon hash are simpler explainations about the referenced paper, with a number of examples and clarifications about the specific implementation.