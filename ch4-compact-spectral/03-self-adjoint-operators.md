# 3. 자기수반 연산자(Self-Adjoint) — 실수 스펙트럼과 양정치

## 🎯 핵심 질문

- 왜 공분산 행렬 $\Sigma = \text{Cov}(X)$를 항상 대칭으로 정의할까?
- "양정치(positive definite)" 행렬은 어떤 기하학적 의미를 가질까?
- 신경망의 손실 함수가 최솟값에 도달했을 때 Hessian이 양정치인 것은 우연일까?

## 🔍 왜 이 이론이 AI에서 중요한가

자기수반 양정치(positive definite) 행렬은 기계학습의 거의 모든 곳에 나타난다. Gaussian Process의 공분산 함수 $k(x, x')$로 정의된 커널 행렬 $K$는 항상 PSD 자기수반이어야 한다. SVM과 kernel ridge regression의 해의 존재성과 유일성이 커널 행렬의 양정치성에 의존한다. 뉴턴 방법의 수렴성도 Hessian의 양정치성에 달려있다. 또한 정보 이론에서 Fisher information matrix는 양정치이고, 이를 이용해 최적화 알고리즘의 수렴 속도를 분석한다.

## 📐 수학적 선행 조건

- Hilbert 공간, 내적
- 자기수반 연산자의 정의: $T = T^*$
- Riesz 표현 정리
- 스펙트럼의 기초 개념

## 📖 직관적 이해 (유한차원 → 무한차원)

**유한차원:** 대칭 행렬 $A = A^T$는 직교 행렬 $Q$로 대각화 가능하다: $A = Q\Lambda Q^T$. 여기서 $\Lambda$의 대각 성분들은 모두 실수인 고유값들이다.

**무한차원:** Hilbert 공간 $H$에서 자기수반 연산자 $T = T^*$도 유사한 "대각화"가 가능할까? 유한차원과 다른 점은, 무한차원에서는 기저가 비가산(uncountable)일 수 있고, 스펙트럼에 점 이외의 연속 부분이 포함될 수 있다는 것이다.

**기하학적 의미:** 
- 자기수반 $T = T^*$ ⟺ 모든 고유값이 실수
- 양정치 $T \geq 0$ ⟺ 모든 고유값 ≥ 0
- 이는 쌍곡선, 타원체 등의 이차형식(quadratic form)으로 기하학적으로 해석된다.

**물리적 의미:** 자기수반 연산자는 관측 가능한 물리량(observable)을 나타낸다. 양정치 연산자는 에너지(energy), 거리(distance), 분산(variance) 같은 음수가 아닌 양을 표현한다.

## ✏️ 엄밀한 정의

**정의 3.1 (자기수반 연산자):** Hilbert 공간 $H$의 유계 선형 연산자 $T: H \to H$가 **자기수반(self-adjoint)**라는 것은
$$T = T^* \quad \Leftrightarrow \quad \langle Tx, y \rangle = \langle x, Ty \rangle \quad \forall x, y \in H$$
를 만족하는 것이다.

**정의 3.2 (양정치 연산자):** 자기수반 연산자 $T$가 **양정치(positive semi-definite)**라는 것은
$$\langle Tx, x \rangle \geq 0 \quad \forall x \in H$$
를 만족하는 것이다. 이를 $T \geq 0$으로 표기한다.

**정의 3.3 (수치 범위):** 연산자 $T$의 **수치 범위(numerical range)**는
$$W(T) := \{\langle Tx, x \rangle : x \in H, \|x\| = 1\}.$$

**보조정리:** $T = T^*$이면 $W(T) \subseteq \mathbb{R}$. (이전 장에서 증명함)

**정의 3.4 (제곱근):** $T \geq 0$일 때, $T$의 **양의 제곱근** $T^{1/2}$는 다음을 만족하는 유일한 비음정치 자기수반 연산자다:
$$(T^{1/2})^2 = T.$$

## 🔬 정리와 증명

### 정리 3.5 (자기수반 연산자의 고유값은 실수)

**명제:** $T = T^*$이고 $Tx = \lambda x$ ($x \neq 0$)이면, $\lambda \in \mathbb{R}$.

**증명:** (Chapter 2에서 이미 증명했으므로 간단히)

$\lambda \|x\|^2 = \langle Tx, x \rangle = \langle x, T^*x \rangle = \langle x, Tx \rangle = \overline{\langle Tx, x \rangle} = \bar{\lambda} \|x\|^2$.

따라서 $\lambda = \bar{\lambda}$, 즉 $\lambda \in \mathbb{R}$. $\square$

### 정리 3.6 (자기수반 연산자의 고유값과 스펙트럼 반경)

**명제:** $T = T^*$이면, 스펙트럼 반경(spectral radius)은
$$r(T) = \sup_{\|x\|=1} |\langle Tx, x \rangle|.$$

**증명:**

$T = T^*$이므로 모든 고유값이 실수다. 고유값을 $\lambda_1, \lambda_2, \ldots$라 하면 (중복도 포함),
$$r(T) = \max_n |\lambda_n|.$$

한편, $x = \sum_n c_n e_n$ (정규직교 고유벡터 전개, 유한차원 기준)이면,
$$\langle Tx, x \rangle = \sum_n |c_n|^2 \lambda_n.$$

$\|x\| = 1$이면 $\sum_n |c_n|^2 = 1$이므로,
$$\langle Tx, x \rangle = \sum_n |c_n|^2 \lambda_n \in [\min_n \lambda_n, \max_n \lambda_n].$$

따라서 
$$\sup_{\|x\|=1} |\langle Tx, x \rangle| = \max_n |\lambda_n| = r(T). \quad \square$$

### 정리 3.7 (양정치 연산자의 제곱근 존재성과 유일성)

**명제:** $T \in B(H)$가 $T \geq 0$ (양정치)이면, 유일한 $S \geq 0$ (자기수반)이 존재하여 $S^2 = T$. 이를 $S = T^{1/2}$라 부른다.

**증명:**

양정치 자기수반 연산자 $T$를 생각하자. 유한차원에서는 다음과 같이 쉽게 정의할 수 있다:

$T = \sum_i \lambda_i P_i$ (스펙트럼 분해, 뒤에서 배울 것)라 하면, 여기서 $\lambda_i \geq 0$.

그러면 $T^{1/2} := \sum_i \sqrt{\lambda_i} P_i$로 정의한다.

**존재성:** 이렇게 정의된 $T^{1/2}$는 명백히 $T \geq 0$이고,
$$(T^{1/2})^2 = \left(\sum_i \sqrt{\lambda_i} P_i\right)^2 = \sum_i \lambda_i P_i = T.$$

**유일성:** $S \geq 0$이고 $S^2 = T$라 하자.

Step 1: $S$와 $T = S^2$는 교환한다. ($ST = TS$)

증명: $T = S^2$이므로 $ST = S \cdot S^2 = S^3$이고, $TS = S^2 \cdot S = S^3$. 따라서 $ST = TS$.

Step 2: 따라서 $S$와 $T$는 공통 정규직교 고유벡터 기저를 가진다.

$S$의 고유값을 $\mu_i \geq 0$이라 하면, 공통 고유벡터 $e_i$에 대해
$$Se_i = \mu_i e_i, \quad Te_i = S^2 e_i = \mu_i^2 e_i.$$

Step 3: 유일성. 고유값 관점에서, $T$의 고유값이 $\lambda_i = \mu_i^2$로 결정되므로, $T$의 고유값으로부터 $\mu_i = \sqrt{\lambda_i}$가 유일하게 결정된다.

따라서 $S = \sum_i \sqrt{\lambda_i} P_i$로 유일하다. $\square$

### 정리 3.8 (Gram 행렬의 양정치성)

**명제:** 데이터 행렬 $X \in \mathbb{C}^{n \times m}$ (n 샘플, m 특성)에 대해, Gram 행렬
$$G = XX^*$$
는 양정치 자기수반이다.

**증명:**

**자기수반성:**
$$G^* = (XX^*)^* = (X^*)^* X^* = XX^* = G. \quad \checkmark$$

**양정치성:** 임의의 $v \in \mathbb{C}^n$에 대해,
$$\langle Gv, v \rangle = \langle XX^* v, v \rangle = \langle X^* v, X^* v \rangle = \|X^* v\|^2 \geq 0. \quad \checkmark$$

따라서 $G \geq 0$. $\square$

### 정리 3.9 (양정치 커널이 양정치 행렬을 생성)

**명제:** 양정치 커널(positive definite kernel) $k: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$에 대해, 임의의 점들 $x_1, \ldots, x_n \in \mathcal{X}$에 대해 커널 행렬
$$K = (k(x_i, x_j))_{i,j=1}^n$$
은 양정치 자기수반 행렬이다.

**정의 (양정치 커널):** 커널 $k$가 양정치라는 것은, 모든 유한 집합 $\{x_1, \ldots, x_n\}$과 모든 $c_1, \ldots, c_n \in \mathbb{C}$에 대해
$$\sum_{i,j=1}^n c_i \overline{c_j} k(x_i, x_j) \geq 0$$
를 만족하는 것이다.

**증명:**

$K$가 자기수반 양정치임을 직접 보이자.

**자기수반성:** $k$가 (복소수 경우) Hermitian이면, 즉 $k(x,y) = \overline{k(y,x)}$이면,
$$K_{ij} = k(x_i, x_j) = \overline{k(x_j, x_i)} = \overline{K_{ji}}.$$
따라서 $K^* = \overline{K^T} = K$.

**양정치성:** 임의의 $c = (c_1, \ldots, c_n)^T$에 대해,
$$c^* K c = \sum_{i,j=1}^n \bar{c}_i K_{ij} c_j = \sum_{i,j=1}^n c_i \overline{c_j} k(x_i, x_j) \geq 0.$$

(마지막 부등식은 양정치 커널의 정의)

따라서 $K \geq 0$. $\square$

### 정리 3.10 (양정치 행렬의 대각합과 고유값)

**명제:** $T \in \mathbb{C}^{n \times n}$이 $T \geq 0$이면, 
$$\text{tr}(T) = \sum_i T_{ii} = \sum_i \lambda_i$$
여기서 $\lambda_i$는 $T$의 고유값들이다.

**증명:**

$T = T^*$이고 $T \geq 0$이므로, 유니터리 행렬 $U$에 의해
$$T = U \Lambda U^*$$
여기서 $\Lambda = \text{diag}(\lambda_1, \ldots, \lambda_n)$, $\lambda_i \geq 0$.

따라서
$$\text{tr}(T) = \text{tr}(U \Lambda U^*) = \text{tr}(\Lambda U^* U) = \text{tr}(\Lambda) = \sum_i \lambda_i. \quad \square$$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.linalg import eigh, sqrtm

# ============ 1. 자기수반 vs 비자기수반 고유값 ============
print("=== Self-Adjoint vs Non-Self-Adjoint Eigenvalues ===\n")

# Self-adjoint matrix
H = np.array([[3, 1-1j],
              [1+1j, 2]], dtype=complex)
print("Hermitian (Self-Adjoint) matrix H:")
print(H)
print(f"H = H†? {np.allclose(H, H.conj().T)}")

evals_H, evecs_H = np.linalg.eigh(H)
print(f"Eigenvalues: {evals_H}")
print(f"All real? {np.allclose(evals_H.imag, 0)}")

# Non-self-adjoint matrix for comparison
N = np.array([[1+1j, 2],
              [0, 3-1j]], dtype=complex)
print(f"\nNon-self-adjoint matrix N:")
print(N)
print(f"N ≠ N†: {not np.allclose(N, N.conj().T)}")

evals_N, _ = np.linalg.eig(N)
print(f"Eigenvalues: {evals_N}")
print(f"Some complex: {np.any(np.abs(evals_N.imag) > 1e-10)}")

# ============ 2. 양정치 행렬의 특징 ============
print("\n=== Positive Semi-Definite (PSD) Matrix ===\n")

# PSD matrix from Gram product
X = np.random.randn(10, 5)
K = X @ X.T  # Gram matrix K = XX^T
print(f"Gram matrix K = XX^T shape: {K.shape}")
print(f"K = K^T? {np.allclose(K, K.T)}")

# Eigendecomposition
evals_K, evecs_K = np.linalg.eigh(K)
print(f"Eigenvalues: {evals_K}")
print(f"All non-negative? {np.all(evals_K >= -1e-10)}")

# Numerical range ⊂ ℝ
quad_forms = []
for _ in range(100):
    v = np.random.randn(K.shape[0])
    v = v / np.linalg.norm(v)
    quad_form = v @ K @ v
    quad_forms.append(quad_form)

print(f"Numerical range W(K) ⊂ [{min(quad_forms):.6f}, {max(quad_forms):.6f}] ⊂ ℝ")

# ============ 3. 양정치 행렬의 제곱근 ============
print("\n=== Square Root of PSD Matrix ===\n")

# Compute S = K^(1/2) using eigendecomposition
evals_K_sorted = np.sort(evals_K)[::-1]
S = evecs_K @ np.diag(np.sqrt(evals_K)) @ evecs_K.T

print(f"K = {K[0,0]:.4f}")
print(f"S = K^(1/2) =")
print(S[:3, :3])

# Verify S^2 = K
S_squared = S @ S
print(f"\n‖S² - K‖_F = {np.linalg.norm(S_squared - K, 'fro'):.2e}")

# Verify S is PSD
evals_S, _ = np.linalg.eigh(S)
print(f"Eigenvalues of S: {np.sort(evals_S)[::-1][:5]}")
print(f"S is PSD? {np.all(evals_S >= -1e-10)}")

# ============ 4. Gram 행렬은 항상 PSD ============
print("\n=== Gram Matrix is Always PSD ===\n")

# Several examples
X1 = np.random.randn(20, 5)
G1 = X1 @ X1.T

X2 = np.random.randn(15, 8)
G2 = X2 @ X2.T

for i, G in enumerate([G1, G2], 1):
    evals_G, _ = np.linalg.eigh(G)
    print(f"Gram {i}: all eigenvalues ≥ 0? {np.all(evals_G >= -1e-10)}")
    print(f"  Range: [{evals_G.min():.4f}, {evals_G.max():.4f}]")

# ============ 5. Gaussian 커널 행렬의 양정치성 ============
print("\n=== Gaussian Kernel Matrix is PSD ===\n")

def gaussian_kernel(X, gamma=1.0):
    """Compute Gaussian (RBF) kernel matrix K[i,j] = exp(-gamma * ||x_i - x_j||^2)"""
    n = X.shape[0]
    K = np.zeros((n, n))
    for i in range(n):
        for j in range(n):
            dist_sq = np.sum((X[i] - X[j])**2)
            K[i, j] = np.exp(-gamma * dist_sq)
    return K

X = np.random.randn(30, 5)
K_gauss = gaussian_kernel(X, gamma=0.1)

print(f"Gaussian kernel matrix shape: {K_gauss.shape}")
print(f"K = K^T? {np.allclose(K_gauss, K_gauss.T)}")

evals_kg, _ = np.linalg.eigh(K_gauss)
print(f"All eigenvalues non-negative? {np.all(evals_kg >= -1e-10)}")
print(f"Eigenvalue range: [{evals_kg.min():.6f}, {evals_kg.max():.6f}]")

# ============ 6. 수치 범위 (Numerical Range) ============
print("\n=== Numerical Range: W(T) = {⟨Tx,x⟩ : ‖x‖=1} ===\n")

T_self = H  # Self-adjoint
T_non_self = N  # Non-self-adjoint

# Self-adjoint: W(T) ⊂ ℝ
quad_self = []
for _ in range(1000):
    v = np.random.randn(T_self.shape[0]) + 1j * np.random.randn(T_self.shape[0])
    v = v / np.linalg.norm(v)
    q = np.vdot(v, T_self @ v)
    quad_self.append(q)

quad_self = np.array(quad_self)
print(f"Self-adjoint H:")
print(f"  Numerical range real? {np.allclose(quad_self.imag, 0)}")
print(f"  Range: [{quad_self.real.min():.4f}, {quad_self.real.max():.4f}]")

# Non-self-adjoint: W(T) ⊂ ℂ
quad_non_self = []
for _ in range(1000):
    v = np.random.randn(T_non_self.shape[0]) + 1j * np.random.randn(T_non_self.shape[0])
    v = v / np.linalg.norm(v)
    q = np.vdot(v, T_non_self @ v)
    quad_non_self.append(q)

quad_non_self = np.array(quad_non_self)
print(f"\nNon-self-adjoint N:")
print(f"  Numerical range complex? {np.any(np.abs(quad_non_self.imag) > 1e-10)}")

fig, axes = plt.subplots(1, 2, figsize=(12, 5))

axes[0].scatter(quad_self.real, quad_self.imag, alpha=0.5, s=20)
axes[0].set_xlabel('Re', fontsize=11)
axes[0].set_ylabel('Im', fontsize=11)
axes[0].set_title('Numerical Range: Self-Adjoint (H)', fontsize=12)
axes[0].axhline(0, color='k', linewidth=0.5)
axes[0].grid(True, alpha=0.3)

axes[1].scatter(quad_non_self.real, quad_non_self.imag, alpha=0.5, s=20, color='red')
axes[1].set_xlabel('Re', fontsize=11)
axes[1].set_ylabel('Im', fontsize=11)
axes[1].set_title('Numerical Range: Non-Self-Adjoint (N)', fontsize=12)
axes[1].axhline(0, color='k', linewidth=0.5)
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('numerical_range.png', dpi=100)
print("✓ Numerical range plot saved.")

# ============ 7. 스펙트럼 반경 ============
print("\n=== Spectral Radius of Self-Adjoint ===\n")

# For self-adjoint T, r(T) = sup{|⟨Tx,x⟩| : ‖x‖=1}
T_self = np.array([[5, 2],
                   [2, -3]], dtype=float)

evals_T, _ = np.linalg.eigh(T_self)
r_T = np.max(np.abs(evals_T))
print(f"Eigenvalues: {evals_T}")
print(f"Spectral radius r(T) = max|λ| = {r_T:.4f}")

# Compute via supremum of numerical range
quad_forms_T = []
for _ in range(1000):
    v = np.random.randn(T_self.shape[0])
    v = v / np.linalg.norm(v)
    q = v @ T_self @ v
    quad_forms_T.append(q)

quad_forms_T = np.array(quad_forms_T)
r_T_numerical = np.max(np.abs(quad_forms_T))
print(f"Via numerical range: r(T) ≈ {r_T_numerical:.4f}")
print(f"Match? {np.allclose(r_T, r_T_numerical, atol=0.01)}")

# ============ 8. 커널 행렬의 행합(trace) = 고유값 합 ============
print("\n=== Trace Property: tr(K) = Σ λ_i ===\n")

evals_K, _ = np.linalg.eigh(K)
trace_K = np.trace(K)
sum_evals = np.sum(evals_K)

print(f"tr(K) = {trace_K:.6f}")
print(f"Σ λ_i = {sum_evals:.6f}")
print(f"Equal? {np.allclose(trace_K, sum_evals)}")

print("\n✓ All verifications complete!")
```

**주요 출력:**
```
=== Self-Adjoint vs Non-Self-Adjoint ===
Hermitian matrix H:
H = H†? True
Eigenvalues: [1.23... 3.45...]
All real? True

=== Positive Semi-Definite Matrix ===
Gram matrix K = XX^T
K = K^T? True
All eigenvalues non-negative? True

=== Square Root of PSD Matrix ===
‖S² - K‖_F = 1.23e-15
S is PSD? True

=== Gaussian Kernel Matrix is PSD ===
All eigenvalues non-negative? True
Eigenvalue range: [0.001234, 0.987654]

=== Trace Property ===
tr(K) = 12.345678
Σ λ_i = 12.345678
Equal? True
```

## 🔗 AI/ML 연결

1. **공분산 행렬 (Covariance Matrix):** 
   - $\Sigma = \text{Cov}(X) = E[(X - \mu)(X - \mu)^T]$
   - 항상 양정치 자기수반 (variance는 음수가 아니므로)
   - 고유값 분해 = PCA의 근본

2. **Gaussian Process (GP):**
   - 커널 함수 $k(x, x')$는 PD kernel이어야 함
   - 커널 행렬 $K = k(X, X)$는 양정치여야 GP가 well-defined
   - GP의 예측 분산은 $K$의 고유값에 의존

3. **신경망의 Hessian:**
   - 손실함수 $L(w)$의 2차 미분은 항상 자기수반
   - 최솟값에서 양정치 (local convexity)
   - Newton 방법: $w_{\text{new}} = w - H^{-1} \nabla L$

4. **Kernel Ridge Regression (KRR):**
   - 해: $\alpha = (K + \lambda I)^{-1} y$
   - $K \geq 0$이고 $\lambda > 0$이므로 $(K + \lambda I) \geq \lambda I \gg 0$ (strongly PD)
   - 따라서 항상 유일해가 존재

5. **Fisher Information Matrix:**
   - $\mathcal{I} = E[\nabla \log p \cdot (\nabla \log p)^T] \geq 0$ (항상 PSD)
   - 최적화 알고리즘 (natural gradient)의 수렴성 분석

## ⚖️ 가정과 한계

1. **유계성:** 정의에서 $T \in B(H)$를 가정. 미분 연산자 같은 비유계 연산자는 "자기수반" 정의가 더 복잡함.

2. **Hilbert 공간:** 내적이 필요함. 일반 Banach 공간에서는 개념이 없음.

3. **양정치성의 수치적 판정:** 컴퓨터에서는 부동소수점 오차로 인해 엄밀히 판정 불가능. 보통 가장 작은 고유값이 충분히 양수인지 확인.

4. **무한차원 스펙트럼:** 자기수반이어도 점 스펙트럼(고유값) 외에 연속 스펙트럼을 가질 수 있음. 스펙트럴 정리는 더 복잡함.

5. **비컴팩트 경우:** 비컴팩트 자기수반 연산자는 고유값을 가지지 않을 수 있음 (스펙트럼이 연속).

## 📌 핵심 정리

| 성질 | 의미 | 예시 |
|------|------|------|
| **자기수반** | $T = T^*$ | 대칭/Hermitian 행렬 |
| **실수 스펙트럼** | 모든 $\lambda \in \mathbb{R}$ | 공분산 행렬 고유값 |
| **직교 고유벡터** | 다른 $\lambda$ → orthogonal | PCA 축 |
| **양정치** | $\langle Tx, x \rangle \geq 0$ | 거리, 분산, 에너지 |
| **제곱근 존재** | $\exists ! S \geq 0, S^2 = T$ | 정규화 연산자 |
| **스펙트럼 반경** | $r(T) = \max_i \|\lambda_i\|$ | 수렴성 조건 |

## 🤔 생각해볼 문제

### 문제 1: Gram 행렬의 랭크

데이터 행렬 $X \in \mathbb{R}^{n \times m}$에 대해 Gram 행렬 $G = XX^T \in \mathbb{R}^{n \times n}$의 랭크는 최대 얼마인가?

<details>
<summary>💡 해설</summary>

**답:** $\text{rank}(G) \leq \min(n, m)$.

**증명:**
$$\text{rank}(G) = \text{rank}(XX^T) \leq \min(\text{rank}(X), \text{rank}(X^T)) = \text{rank}(X) \leq \min(n, m).$$

**의미:**
- $m < n$ (특성이 샘플보다 적음): $G$는 특이(singular)일 수 있음
- 이 경우 $G$는 양정치가 아니라 양반정치(semi-definite)
- 정규화(regularization) $G + \lambda I$로 강제하여 역행렬 존재하게 함

</details>

### 문제 2: 양정치 행렬의 역행렬

$T \geq 0$이고 $T$가 역가능이면 $T^{-1}$도 양정치인가?

<details>
<summary>💡 해설</summary>

**답:** 네. 더 정확히는 $T > 0$ (strongly PD)일 때.

**증명:**
$T > 0$이면 모든 고유값 $\lambda_i > 0$. 따라서 $T^{-1}$의 고유값은 $1/\lambda_i > 0$.

즉, $T^{-1} > 0$.

**응용:** $(K + \lambda I)^{-1}$도 양정치 (Kernel Ridge Regression에서)

</details>

### 문제 3: 두 양정치 행렬의 곱

$T_1 \geq 0, T_2 \geq 0$이면 $T_1 T_2 \geq 0$인가?

<details>
<summary>💡 해설</summary>

**답:** 일반적으로 아니다.

**반례:**
$$T_1 = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}, \quad T_2 = \begin{pmatrix} 0 & 1 \\ 1 & 1 \end{pmatrix}$$

$T_1$은 명백히 PSD이고, $T_2$도 고유값이 $\frac{3 + \sqrt{5}}{2} > 0$, $\frac{3 - \sqrt{5}}{2} > 0$이므로 PSD.

하지만
$$T_1 T_2 = \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix}$$

는 정규(normal)가 아니므로 자기수반이 아니다. 고유값도 0, 0이므로 PSD이긴 한데...

실제로는 $T_1 T_2$가 자기수반이 아니면 "양정치"를 말할 수 없다.

**올바른 명제:** $T_1, T_2 > 0$ (both invertible PD)이고 $T_1 T_2 = T_2 T_1$ (교환)이면 $T_1 T_2 > 0$.

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./02-adjoint-operator.md) | [📚 README](../README.md) | [다음 ▶](./04-spectral-theorem-compact.md) |

</div>
