# PHE_ML

## Privacy-Preserving Linear Regression using Homomorphic Encryption

This project implements **Linear Regression inference over encrypted data** using the **CKKS homomorphic encryption scheme** (via TenSEAL).

The system enables computation of predictions without exposing raw input data to the server.

---

## Motivation

In settings such as healthcare, finance, and cloud-based ML services, sensitive data cannot be shared directly. This work demonstrates a mechanism to:

* Preserve input data confidentiality
* Delegate computation to an untrusted server
* Maintain prediction accuracy

---

## Why Linear Regression

Linear Regression is well-suited for homomorphic evaluation because its prediction function:

$$
y = w^T x + b
$$

consists only of:

* additions
* multiplications

These operations are natively supported in homomorphic encryption schemes.

More complex models introduce non-linearities and deep computation graphs, which significantly increase computational cost and noise growth under encryption.

---

## Homomorphic Encryption (CKKS)

The CKKS scheme enables approximate arithmetic over encrypted real-valued vectors.

Given an input vector:

$$
x = [x_1, x_2, \dots, x_n]
$$

encryption produces:

$$
Enc(x)
$$

Homomorphic evaluation allows:

$$
Enc(w^T x + b)
$$

to be computed without decryption.

After decryption:

$$
Dec(Enc(y)) \approx y
$$

The approximation error arises from controlled noise introduced during encryption and computation.

---

## Mathematical Formulation

### Plaintext Prediction

$$
y = \sum_{i=1}^{n} w_i x_i + b
$$

### Encrypted Computation

$$
\hat{y} = Dec\left( \sum_{i=1}^{n} w_i \cdot Enc(x_i) + b \right)
$$

By linearity:

$$
\hat{y} = \sum_{i=1}^{n} w_i x_i + b + \epsilon
$$

where $\epsilon$ denotes approximation noise.

---

## Workflow

```mermaid
flowchart LR
    A["Client: Raw Input x"] --> B["Encrypt x → Enc(x)"]
    B --> C["Send to Server"]

    C --> D["Server: Compute Enc(y) = Enc(wᵀx + b)"]

    D --> E["Return Enc(y)"]
    E --> F["Client: Decrypt → y"]

    style A fill:#1e293b,color:#ffffff
    style B fill:#2563eb,color:#ffffff
    style C fill:#0f766e,color:#ffffff
    style D fill:#7c3aed,color:#ffffff
    style E fill:#0f766e,color:#ffffff
    style F fill:#1e293b,color:#ffffff
```

---

## Training vs Inference

This implementation performs:

* Training in plaintext
* Inference on encrypted inputs

Training directly on encrypted data is not practical due to:

* iterative optimization (gradient descent)
* multiplicative depth growth
* accumulation of encryption noise

---

## Tech Stack

* Python
* TenSEAL (CKKS)
* NumPy
* Scikit-learn

---

## Key Takeaways

* Homomorphic encryption enables secure inference without revealing inputs
* Linear models are computationally efficient under encryption
* CKKS introduces small, bounded approximation error
* Practical deployments currently focus on encrypted inference rather than training

---

