# Hakan Özcan

**AI Systems Engineer** — language models from first principles, GPU performance, and secure-by-design software.

I work where models meet hardware: how a training step spends its FLOPs, bytes and bandwidth, and how to make it faster without making it wrong. I build each layer myself before relying on a library: tokenizers, autograd engines, tensor runtimes, GPU kernels. When a production system misbehaves, I know which layer to suspect.

[LinkedIn](https://www.linkedin.com/in/ozcanhakan/) · [ozcanhakn@gmail.com](mailto:ozcanhakn@gmail.com)

---

## Areas of Expertise

### Language Modeling from Scratch
The full foundation-model pipeline, built end to end following Stanford CS336:

- Tokenizer, Transformer, optimizer and training loop written without pretrained libraries.
- Resource accounting before any training run: FLOPs, plus parameter, optimizer-state and activation memory budgets.
- Scaling-law fits reported with extrapolation uncertainty, not single-point predictions.
- Pretraining data pipeline: web-crawl filtering, deduplication and mixing, with the effect on model behavior measured.
- Post-training with SFT and reinforcement learning for reasoning, keeping each method's failure modes separate.

### GPU Performance and Distributed Training
- A Triton kernel suite (fused softmax, matmul, LayerNorm/RMSNorm, fused attention). Each kernel is benchmarked against PyTorch with correctness tolerances, achieved bandwidth and a roofline analysis.
- FlashAttention-2 in Triton. I explain its speedup through HBM traffic, not FLOP count.
- Memory-hierarchy reasoning: coalescing, tiling, bank conflicts, register pressure, and why high occupancy can still mean a slow kernel.
- Multi-GPU training with DDP and FSDP2: measured memory and scaling efficiency, modeled all-reduce cost per step, and compared sharding with tensor parallelism.
- The `torch.compile` stack (Dynamo, AOTAutograd, Inductor): finding graph breaks and rewriting code so it compiles cleanly.

### Deep Learning Systems
- A deep learning framework implemented from scratch (CMU 10-414/714, *Needle*): tensor storage and strides, reverse-mode autodiff, an operator library with broadcasting and shape inference, NN modules, optimizers, and CPU and GPU backends.
- Output and gradient parity tests against PyTorch.
- Performance problems narrowed to their source: Python overhead, dispatch, kernel or memory.

### PyTorch Internals and Experiment Engineering
- Autograd internals: saved tensors, hooks, and custom `autograd.Function`s verified with `gradcheck`. View/copy semantics. Mixed precision with BF16, autocast and GradScaler.
- Out-of-memory triage by component: parameters, gradients, optimizer state, activations, KV cache.
- GPU benchmarks that account for warmup, asynchronous execution and variance.
- A training template with AMP, checkpoint/resume and deterministic configuration.

### Transformer Internals
- BPE tokenizer and a minimal GPT, implemented and trained from scratch.
- Attention traced at the tensor-shape level, with property tests for shapes and masking.
- RoPE, the memory/latency trade-off of the KV cache, prefill vs. decode, and decoding strategies.
- LoRA fine-tuning judged on held-out evaluation rather than training loss, then published with a model card.

### Mathematical and Numerical Foundations
- Linear algebra through SVD: the four fundamental subspaces, rank, conditioning and low-rank structure, which I use to analyze LoRA-style updates.
- Backpropagation as a vector–Jacobian product machine, and an autodiff engine with numerical gradient checks.
- Probability and information theory: the MLE → NLL → cross-entropy chain, calibration and proper scoring rules.
- Numerical analysis: conditioning vs. algorithmic stability, floating-point pathologies (cancellation, overflow, NaN propagation), stable log-sum-exp and compensated summation.

### Statistical Machine Learning
- Validation design and leakage audits come before model selection.
- Strong gradient-boosted tabular baselines before any neural network. Calibration and threshold selection.
- Segment-level error analysis and confidence intervals on metric differences.

### Applied Cryptography and Secure Storage
I designed and built **Seraph**, an end-to-end encrypted messaging core for Android:

- Olm double-ratchet sessions with identity keys and one-time prekeys. Account state is persisted as soon as an inbound session consumes a prekey.
- A hardware-backed key hierarchy. A Keystore master key (StrongBox, falling back to TEE) protects a random root key. From that root key, HKDF-SHA256 (RFC 5869, implemented directly) derives domain-separated keys for account state, session state and the database.
- A SQLCipher-encrypted Room database and encrypted preferences (AES-256-SIV for keys, AES-256-GCM for values).
- Key material zeroized in `finally` blocks. Native account and session handles released on lock.
- Versioned migration of legacy key material. Cryptographic erasure by destroying the root key.
- Diagnostics that reveal no secrets, using truncated SHA-256 fingerprints.
- An explicit outbound message state machine (pending → encrypted → failed) and a queue for out-of-sequence inbound messages.

---

## How I Work

- **Measure before claiming.** Latency, memory, error, calibration, cost and statistical uncertainty: nothing is "better" until it has been measured.
- **First principles, then production.** I build the algorithm once by hand, then use the production library knowing what it hides.
- **Reproducible by default.** Every result traces back to a commit, a config, a seed and a dataset.
- **A research loop.** Reproduce → stress-test → ablate → find a counterexample → extend.

---

## Toolchain

| Area | Tools |
|---|---|
| Models & Kernels | PyTorch · Triton · CUDA · torch.compile · FSDP2 / DeviceMesh · Hugging Face |
| Numerical & Data | NumPy · pandas · PostgreSQL (query plans, window functions) |
| Security | libolm · Android Keystore / StrongBox · SQLCipher · Jetpack Security · Clerk |
| Observability | Sentry · Prometheus · Grafana |
| Product | TypeScript · Kotlin · Convex |
| Engineering | uv · pytest · ruff · Quarto |
