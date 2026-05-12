# skills/channel-model.md — Wireless Channel Modeling Reference

> Generic reference for wireless communications papers involving MIMO channel models.
> Covers: channel matrix structure, array response, path loss, near-field, multi-user stacking.
> Fill in Section 7 (Paper-Specific Model) with your actual channel assumptions.

---

## 1. Channel Matrix Taxonomy

### MIMO Channel H ∈ ℂ^(N_r × N_t)

- Rows index receive antennas (N_r of them)
- Columns index transmit antennas (N_t of them)
- Entry H_{ij}: complex gain from transmit antenna j to receive antenna i
- H^H: Hermitian transpose (conjugate transpose), dimension N_t × N_r

### Regime Classification

| Regime | Condition | Model type |
|---|---|---|
| Far-field | distance >> D²/λ (Fraunhofer) | Planar wavefront (angle-only steering vector) |
| Near-field | distance < D²/λ | Spherical wavefront (per-element distance + angle) |
| LoS dominant | Rician K >> 1 | Structured deterministic component dominates |
| Rich scattering | Rayleigh (K = 0) | Random i.i.d. complex Gaussian entries |

**Fraunhofer distance:** d_F = 2D²/λ, where D = array aperture, λ = wavelength.

---

## 2. Array Response Vectors

### ULA (Uniform Linear Array) — Far-Field

N-element ULA with half-wavelength spacing (d = λ/2), angle of departure θ:

```
a(θ) = (1/√N) [1, e^{j π sin(θ)}, e^{j 2π sin(θ)}, ..., e^{j (N-1)π sin(θ)}]^T  ∈ ℂ^N
```

In NumPy:
```python
def ula_response(N: int, theta: float) -> np.ndarray:
    n = np.arange(N)
    return (1 / np.sqrt(N)) * np.exp(1j * np.pi * n * np.sin(theta))
```

### UPA (Uniform Planar Array)

N_y × N_z elements, angles (θ, φ):
```python
def upa_response(Ny: int, Nz: int, theta: float, phi: float) -> np.ndarray:
    ay = np.exp(1j * np.pi * np.arange(Ny) * np.sin(theta) * np.cos(phi))
    az = np.exp(1j * np.pi * np.arange(Nz) * np.sin(phi))
    return np.kron(ay, az) / np.sqrt(Ny * Nz)
```

### Near-Field Spherical Wavefront

Per-element distance r_n from source to element n:
```
r_n = √(r₀² + n²d² - 2r₀ n d sin(θ))  ≈  r₀ - n d sin(θ) + n²d²cos²(θ)/(2r₀)
```

Channel coefficient for element n:
```
h_n = (κ₀ / r_n^{n_ref/2}) * exp(-j 2π r_n / λ)
```

---

## 3. Path Loss and Distance-Dependent Models

### Free-Space Path Loss (Friis Formula)

```
PL(r) = (λ / 4πr)²   [linear scale]
PL_dB(r) = 20 log10(λ/4π) - 20 log10(r)
```

### General Distance-Dependent Complex Channel

Amplitude + phase form (commonly used in near-field / position-dependent MIMO):

```
h(r, φ) = (κ₀ / r^{n_ref/2}) * exp(-j * φ(r))
```

- κ₀: reference path gain at unit distance
- n_ref: path loss exponent (2 = free space; 2–4 for indoor)
- φ(r) = 2πr/λ + constant: phase accumulation

### Rician Channel Model

```
H = √(K/(K+1)) H_LoS + √(1/(K+1)) H_NLoS
```

- H_LoS: deterministic LoS component (use array response vectors)
- H_NLoS ~ CN(0, I) per entry (random scattering)
- K = 0: pure Rayleigh; K → ∞: pure LoS

---

## 4. Multi-User Channel Stacking

### Downlink (Base Station → K users)

Individual user channel: h_k ∈ ℂ^{N_t}, k ∈ {1, ..., K}

Aggregate channel matrix:
```
H = [h_1, h_2, ..., h_K]^H  ∈ ℂ^{K × N_t}
```

Received signal at user k (with beamformer W = [w_1, ..., w_K]):
```
y_k = h_k^H W s + n_k = h_k^H w_k s_k + Σ_{j≠k} h_k^H w_j s_j + n_k
```

SINR at user k:
```
SINR_k = |h_k^H w_k|² / (Σ_{j≠k} |h_k^H w_j|² + σ²)
```

### Uplink (K users → Base Station)

Individual user channel: g_k ∈ ℂ^{N_r}

Aggregate channel:
```
G = [g_1, g_2, ..., g_K]  ∈ ℂ^{N_r × K}
```

Received aggregate signal:
```
y = G P^{1/2} s + n  ∈ ℂ^{N_r}
```

where P = diag(p_1, ..., p_K) is the uplink power allocation matrix.

---

## 5. Interference Covariance Matrix

For sensing receivers or uplink combiners, the interference-plus-noise covariance:

```
R_I = Σ_k (interference source k contribution) + σ²I
    = G_interference * Q * G_interference^H + σ²I
```

Used in MVDR / Capon beamformer:
```
u_mvdr = R_I^{-1} g_target / ‖R_I^{-1} g_target‖
```

In NumPy (use solve, not inversion):
```python
R_I_inv_g = np.linalg.solve(R_I, g_target)
u_mvdr = R_I_inv_g / np.linalg.norm(R_I_inv_g)
```

---

## 6. Notation Conventions for Channel Models

| Object | Symbol | Type | LaTeX |
|---|---|---|---|
| Single-user downlink channel | h_k | vector ∈ ℂ^{N_t} | `\mathbf{h}_k` |
| Multi-user channel matrix | H | matrix ∈ ℂ^{K × N_t} | `\mathbf{H}` |
| Uplink channel vector | g_k | vector ∈ ℂ^{N_r} | `\mathbf{g}_k` |
| Downlink beamformer | w_k | vector ∈ ℂ^{N_t} | `\mathbf{w}_k` |
| Beamforming matrix | W | matrix ∈ ℂ^{N_t × K} | `\mathbf{W}` |
| Noise variance | σ² | scalar | `\sigma^2` |
| User set (downlink) | K_c | set | `\mathcal{K}_c` |
| User set (uplink) | K_u | set | `\mathcal{K}_u` |

---

## 7. Paper-Specific Channel Model (Fill In)

> Replace this section with the actual channel model for your paper.

### System Scenario
| Parameter | Symbol | Value |
|---|---|---|
| Carrier frequency | f_c | [e.g., 28 GHz] |
| Wavelength | λ | [e.g., c/f_c] |
| Path loss exponent | n_ref | [e.g., 2.0] |
| Reference path gain | κ₀ | [value / formula] |
| Channel model | — | [e.g., LoS near-field / Rician / Rayleigh] |
| Scenario | — | [e.g., indoor 20×20 m²] |

### Channel Equations Used in This Paper
[Write the exact channel model equations, matching main.tex notation]

### Simulation Notes
[Any Monte Carlo averaging specifics, seed, realization count, etc.]
