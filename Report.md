## Self-Pruning Neural Network Report

### Explanation of L1 Penalty on Sigmoid Gates

In this model, each weight is associated with a learnable gate parameter. These gate scores are passed through a sigmoid function, which constrains their values between 0 and 1. The output of the sigmoid determines how much each weight contributes to the final computation.

To encourage sparsity, an L1 penalty is applied to these gate values. Since the sigmoid outputs are always non-negative, the L1 norm reduces to the sum of all gate values. Minimizing this sum encourages many of the gates to move toward zero.

When a gate value approaches zero, the corresponding weight is effectively removed from the network because it gets multiplied by a very small number. This results in a sparse network where only the most important connections remain active. Compared to L2 regularization, L1 is more effective at driving values exactly to zero, which makes it suitable for pruning.

---

### Results Table

| Lambda | Test Accuracy (%) | Sparsity Level (%) |
|--------|------------------|--------------------|
| 1e-05  | 56.33            | 48.44              |
| 8e-05  | 56.39            | 86.44              |
| 1e-04  | 56.06            | 88.45              |
| 1e-03  | 55.65            | 99.76              |
| 8e-03  | 46.58            | 99.98              |

---

### Analysis

The results show a clear relationship between the value of λ and the behavior of the network.

For very small values of λ such as 1e-05, the sparsity is relatively low, meaning most connections remain active. This leads to slightly better accuracy, but the model is not effectively pruning unnecessary weights.

As λ increases to values like 8e-05 and 1e-04, sparsity increases significantly while accuracy remains nearly unchanged. This indicates that many redundant connections can be removed without hurting performance. These values provide the best balance between sparsity and accuracy.

For larger values such as 1e-03 and 8e-03, the sparsity becomes extremely high, but the accuracy starts to drop. This happens because the model is pruning too aggressively and removing important connections.

Based on these observations, λ = 8e-05 provides the best trade-off in this experiment. It achieves high sparsity while maintaining comparable accuracy.

---

### Gate Value Distribution

The distribution of gate values for the best model (λ = 8e-05) shows a large concentration of values close to zero and another group of values away from zero.

The spike near zero indicates that many weights have been effectively pruned. The remaining values correspond to important connections that the model has retained. This bimodal distribution confirms that the network is successfully learning to prune itself during training.
