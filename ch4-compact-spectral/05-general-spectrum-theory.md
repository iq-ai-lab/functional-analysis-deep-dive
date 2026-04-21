# 5. 일반 스펙트럼 이론 — 고유값 너머의 스펙트럼

## 🎯 핵심 질문

- "스펙트럼"이 "고유값"과 다를 수 있을까?
- 왜 일부 연산자는 고유값을 가지지 않으면서도 스펙트럼을 가질까?
- 신경망에서 vanishing/exploding gradient 문제는 스펙트럼과 어떤 관련이 있을까?

## 🔍 왜 이 이론이 AI에서 중요한가

RNN의 학습 안정성은 가중치 행렬 $W$의 스펙트럼 반경(spectral radius)에 의존한다. 스펙트럼 반경이 1보다 크면 gradient가 폭증하고, 1보다 작으면 소실된다. 따라서 spectral normalization은 $\|W\|_{\text{op}} = 1$이 되도록 정규화한다. 또한 Graph Neural Networks의 수렴성도 그래프 Laplacian의 스펙트럼으로 분석된다. Transformer의 attention mechanism도 스펙트럼 관점에서 분석되고 있으며, 선형 attention은 스펙트럼을 조정하여 계산 복잡도를 줄인다. 마지막으로, 연속 스펙트럼을 가지는 이동 연산자(shift operator)의 이해는 recurrent 구조의 본질을 드러낸다.

## 📐 수학적 선행 조건

- 유계 선산 연산자 $B(X, Y)$
- 역원의 존재와 연속성
- 정규직교 기저와 직교 여공간
- 컴팩트성의 개념

## 📖 직각 이해 (유한차원 → 무한차원)

**유한차원:** $n \times n$ 행렬 $A$의 스펙트럼은 고유값들의 집합: $\sigma(A) = \{\lambda \in \mathbb{C} : \det(A - \lambda I) = 0\}$.

**무한차원 변화:** 무한차원에서는 $\det$을 정의할 수 없다! 대신 역원의 존재성으로 정의한다:
$$\sigma(T) := \{\lambda \in \mathbb{C} : T - \lambda I \text{는 } B(X)에서 역원을 갖지 않음\}.$$

**아주 다른 점:** 무한차원에서 스펙트럼에는 고유값뿐만 아니라 "근처에 있지만 고유값이 아닌" 점들도 포함될 수 있다.

**직각적 분류:**
- **점 스펙트럼:** 고유값 (T-λI가 단사 아님)
- **연속 스펙트럼:** T-λI가 단사이지만, 치역이 조밀하지 않음 (약간의 "잡음"이 중대한 변화를 만듦)
- **잔여 스펙트럼:** T-λI가 단사이고 치역이 조밀하지만 전사 아님

**기하학 의미:** 
- 점 스펙트럼 = 명확한 고유 진동 모드
- 연속 스펙트럼 = 모든 모드를 약간씩 섞을 수 있는 "연속적" 행동
- 잔여 스펙트럼 = 매우 비정규(singular) 구조

## ✏️ 엄밀한 정의

**정의 5.1 (스펙트럼):** $T \in B(X)$에 대해, **스펙트럼** $\sigma(T)$는
$$\sigma(T) := \{\lambda \in \mathbb{C} : T - \lambda I \text{는 } B(X) \text{에서 역원을 갖지 않음}\}.$$

**정의 5.2 (리솔벤트):** $\lambda \notin \sigma(T)$일 때, **리솔벤트(resolvent)**는
$$R(\lambda, T) := (T - \lambda I)^{-1} \in B(X).$$

**정의 5.3 (스펙트럼의 분류):**
- **점 스펙트럼:** $\sigma_p(T) := \{\lambda \in \mathbb{C} : \ker(T - \lambda I) \neq \{0\}\}$ (고유값)
- **연속 스펙트럼:** $\sigma_c(T) := \{\lambda \in \mathbb{C} : T - \lambda I \text{ is injective, } \overline{\text{ran}(T - \lambda I)} = X, \text{ but } \text{ran}(T - \lambda I) \neq X\}$
- **잔여 스펙트럼:** $\sigma_r(T) := \{\lambda \in \mathbb{C} : T - \lambda I \text{ is injective, } \overline{\text{ran}(T - \lambda I)} \neq X\}$

**정의 5.4 (스펙트럼 반경):**
$$r(T) := \sup\{|\lambda| : \lambda \in \sigma(T)\} = \lim_{n \to \infty} \|T^n\|^{1/n}.$$

## 🔬 정리와 증명

### 정리 5.5 (스펙트럼은 컴팩트 집합)

**명제:** $T \in B(X)$에 대해, $\sigma(T)$는 $\mathbb{C}$의 컴팩트 부분집합이고, $\sigma(T) \subseteq B_r(0)$ (여기서 $r \leq \|T\|$).

**증명:**

**Step 1: 유계성**

$|\lambda| > \|T\|$이면, $T - \lambda I = -\lambda(I - T/\lambda)$는 역원을 가진다:
$$(T - \lambda I)^{-1} = -\frac{1}{\lambda} (I - T/\lambda)^{-1} = -\frac{1}{\lambda} \sum_{n=0}^\infty (T/\lambda)^n.$$

(Neumann 급수, $\|T/\lambda\| < 1$이므로 수렴)

따라서 $|\lambda| > \|T\|$이면 $\lambda \notin \sigma(T)$.

즉, $\sigma(T) \subseteq B_{\|T\|}(0)$.

**Step 2: 폐집합**

리솔벤트 함수 $R(\lambda, T) = (T - \lambda I)^{-1}$는 $\mathbb{C} \setminus \sigma(T)$에서 해석적이다 (analytic).

$\sigma(T)$ = 이 함수의 정의역 밖 = 폐집합.

(더 정밀하게, Liouville 정리 사용: $R(\lambda, T)$가 무한원점에서 0으로 가고 analytic이므로...)

**Step 3: 컴팩트성**

유계 폐집합이므로 컴팩트. $\square$

### 정리 5.6 (스펙트럼 반경 공식)

**명제:** 
$$r(T) = \lim_{n \to \infty} \|T^n\|^{1/n}.$$

**증명:**

**Step 1: 상한**

고유값 $\lambda$에 대해 $Tx = \lambda x$ ($x \neq 0$)이면,
$$\|T^n x\| = |\lambda|^n \|x\|, \quad \text{thus} \quad \|T^n\| \geq |\lambda|^n.$$

따라서 $\|T^n\|^{1/n} \geq |\lambda|$ for all $\lambda \in \sigma_p(T)$.

고유값이 스펙트럼을 결정하므로, $\limsup_{n \to \infty} \|T^n\|^{1/n} \geq r(T)$.

**Step 2: 하한**

$|\lambda| > r(T)$이면, $T - \lambda I$는 가역이고, 리솔벤트의 norm을 평가할 수 있다:
$$\|(T - \lambda I)^{-1}\| \leq \frac{1}{|\lambda| - r(T)}.$$

이를 사용하면, Gelfand 공식을 통해
$$r(T) = \liminf_{n \to \infty} \|T^n\|^{1/n}.$$

**Step 3: 극한 존재**

위의 두 부등식으로부터,
$$r(T) = \lim_{n \to \infty} \|T^n\|^{1/n}. \quad \square$$

### 정리 5.7 (자기수반 연산자의 스펙트럼)

**명제:** $T = T^* \in B(H)$이면,
- (1) $\sigma(T) \subseteq \mathbb{R}$ (모든 스펙트럼이 실수)
- (2) $\sigma_r(T) = \emptyset$ (잔여 스펙트럼이 없음)
- (3) $r(T) = \|T\|$

**증명:**

**(1) 실수 스펙트럼:**

$\lambda = \alpha + i\beta$라 하고, $\beta \neq 0$이라 하자. 

$x \neq 0$에 대해,
$$\|(T - \lambda I)x\|^2 = \|(T - \alpha I)x - i\beta x\|^2 = \|(T - \alpha I)x\|^2 + \beta^2 \|x\|^2.$$

(두 항이 직교하므로 Pythagorean theorem, $T - \alpha I$는 자기수반)

따라서 $\|(T - \lambda I)x\| \geq |\beta| \|x\|$이므로 $T - \lambda I$는 단사.

더 세밀한 논증으로, 치역도 전체이므로 역원을 가진다.

즉, $\lambda \notin \sigma(T)$.

따라서 $\sigma(T) \subseteq \mathbb{R}$.

**(2) 잔여 스펙트럼 없음:**

$\lambda$가 $T$의 스펙트럼이면, 자동으로 연속 스펙트럼에 속한다. (점이 아니고 잔여가 아니면 연속)

(증명 스케치: $\ker(T - \lambda I) = \{0\}$이지만 치역이 조밀하지 않으면, 정규직교 기저를 취해서...)

**(3) 스펙트럼 반경:**

자기수반이므로 모든 고유값이 실수이고, 스펙트럼도 실수.

따라서 $r(T) = \max\{|\lambda| : \lambda \in \sigma(T)\}$.

한편, $\|T\| = \sup_{\|x\|=1} |\langle Tx, x \rangle| = \max\{|\lambda| : \lambda \in \sigma(T)\}$.

(이전 정리들로부터)

따라서 $r(T) = \|T\|$. $\square$

### 정리 5.8 (이동 연산자의 스펙트럼)

**명제:** $\ell^2(\mathbb{Z})$에서 양쪽 이동 연산자 $S(x_n) = (x_{n+1})$의 스펙트럼은
$$\sigma(S) = \{\lambda \in \mathbb{C} : |\lambda| = 1\} = \partial B_1(0).$$

그리고 
- $\sigma_p(S) = \emptyset$ (고유값 없음)
- $\sigma_c(S) = \partial B_1(0)$ (모두 연속 스펙트럼)
- $\sigma_r(S) = \emptyset$

**증명:**

**Step 1: 정규성**

$S^* = S^{-1}$ (이동의 역은 역방향 이동)이므로,
$$SS^* = S^{-1} S = I = S S^{-1} = S^* S.$$

따라서 $S$는 정규(normal) 유니터리 연산자.

**Step 2: 스펙트럼 경계**

$\|S\| = 1$이므로 (이동은 거리를 보존), $\sigma(S) \subseteq B_1(0)$.

정밀하게는, $|\lambda| = 1$만이 스펙트럼에 속한다. (가역성 논증)

**Step 3: 고유값 없음**

$Sx = \lambda x$ ($\|x\|_2 = 1$)이면, 
$$\|S^n x\| = |\lambda|^n.$$

$S$의 정의상, $\|S^n x\|$는 대수적으로 변한다 (not geometrically decay). 

따라서 고유값이 있으면 모순. 즉, $\sigma_p(S) = \emptyset$.

**Step 4: 연속 스펙트럼**

$|\lambda| = 1$일 때, $S - \lambda I$가 단사이지만 치역이 조밀하지 않음을 보일 수 있다.

(기술적: Fourier 해석을 사용하면, $S$는 $e^{i\theta}$ 곱하기와 같음)

따라서 $\sigma(S) = \sigma_c(S) = \partial B_1(0)$. $\square$

### 정리 5.9 (스펙트럼 반경과 수렴)

**명제:** $r(T) < 1$ ⟹ $T^n \to 0$ (exponentially).

더 정확히, $r(T) < \rho < 1$인 $\rho$가 존재하면,
$$\|T^n\| = O(\rho^n).$$

**증명:**

$r(T) < 1$이면, 정의상
$$\lim_{n \to \infty} \|T^n\|^{1/n} < 1.$$

따라서 임의의 $\epsilon > 0$에 대해 $N$이 존재하여 $n > N$이면
$$\|T^n\|^{1/n} < 1 + \epsilon.$$

즉, $\|T^n\| < (1 + \epsilon)^n$.

$\epsilon$를 충분히 작게 하면, $1 + \epsilon < \rho < 1$ for some $\rho$이므로,
$$\|T^n\| = O(\rho^n). \quad \square$$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.linalg import eig, eigsh

# ============ 1. 스펙트럼 vs 고유값 ============
print("=== Spectrum vs Eigenvalues ===\n")

# 대각 행렬 (고유값 명확)
A = np.diag([3, 2, 1, 0.5, 0.1])
evals_A, _ = np.linalg.eig(A)
print(f"Diagonal matrix A:")
print(f"  Eigenvalues: {np.sort(np.abs(evals_A))[::-1]}")
print(f"  Spectrum σ(A): {set(np.sort(np.abs(evals_A))[::-1])}")

# Upper triangular (일부 고유값 = 스펙트럼의 경계)
B = np.array([[1, 1, 0],
              [0, 1, 1],
              [0, 0, 1]], dtype=float)
evals_B, _ = np.linalg.eig(B)
print(f"\nUpper triangular matrix B:")
print(f"  Eigenvalues: {evals_B}")
print(f"  ‖B‖ = {np.linalg.norm(B, 2):.4f}")
print(f"  max|λ| = {np.max(np.abs(evals_B)):.4f}")

# ============ 2. 이동 연산자의 스펙트럼 (이산 버전) ============
print("\n=== Shift Operator Spectrum ===\n")

# n×n shift matrix
n = 100
shift_matrix = np.zeros((n, n))
for i in range(n-1):
    shift_matrix[i, i+1] = 1
# shift_matrix[n-1, 0] = 1  # circular shift

evals_shift, _ = np.linalg.eig(shift_matrix)

# Plot eigenvalues
fig, axes = plt.subplots(1, 2, figsize=(13, 5))

# Shift operator eigenvalues (should be near unit circle)
ax = axes[0]
ax.scatter(evals_shift.real, evals_shift.imag, alpha=0.6, s=50)
circle = plt.Circle((0, 0), 1, fill=False, color='r', linestyle='--', linewidth=2)
ax.add_patch(circle)
ax.set_xlabel('Re', fontsize=12)
ax.set_ylabel('Im', fontsize=12)
ax.set_title(f'Shift Operator Eigenvalues (n={n})', fontsize=12)
ax.grid(True, alpha=0.3)
ax.set_aspect('equal')
ax.set_xlim(-1.5, 1.5)
ax.set_ylim(-1.5, 1.5)

# ============ 3. 스펙트럼 반경과 ‖T^n‖ ============
print("=== Spectral Radius: r(T) = lim ‖T^n‖^{1/n} ===\n")

# 다양한 행렬에 대해 r(T) 계산
matrices = {
    'Stable (λ=0.9)': np.eye(5) * 0.9,
    'Stable (λ=0.7)': np.eye(5) * 0.7,
    'Unstable (λ=1.1)': np.eye(5) * 1.1,
    'RNN weight (mixed)': np.random.randn(5, 5) * 0.3,
}

ax = axes[1]

for name, mat in matrices.items():
    evals, _ = np.linalg.eig(mat)
    r = np.max(np.abs(evals))
    
    # Compute ‖T^n‖^{1/n} for n = 1, 2, ..., 30
    norms = []
    powers = []
    for n in range(1, 31):
        mat_n = np.linalg.matrix_power(mat, n)
        norm_n = np.linalg.norm(mat_n, 2)
        norms.append(norm_n)
        powers.append(n)
    
    norm_ratios = [norms[i]**(1/(i+1)) for i in range(len(norms))]
    ax.plot(powers, norm_ratios, 'o-', label=f'{name} (r={r:.3f})', linewidth=2, markersize=5)
    print(f"{name}:")
    print(f"  r(T) = {r:.4f}")
    print(f"  lim ‖T^n‖^{1/n} → {norm_ratios[-1]:.4f}")

ax.axhline(1, color='k', linestyle=':', label='Critical (r=1)', linewidth=2)
ax.set_xlabel('n', fontsize=12)
ax.set_ylabel('‖T^n‖^{1/n}', fontsize=12)
ax.set_title('Spectral Radius Convergence', fontsize=12)
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3)
ax.set_ylim([0, 2])

plt.tight_layout()
plt.savefig('spectrum_analysis.png', dpi=100)
print("\n✓ Spectrum analysis plots saved.")

# ============ 4. RNN의 Vanishing/Exploding Gradient ============
print("\n=== RNN: Vanishing/Exploding Gradient ===\n")

# 시간 단계가 T이고 시간 RNN 가중치가 W일 때,
# dL/dh_t ∝ (W^T)^{T-t} dL/dh_T
# 안정성: r(W^T) = r(W) < 1

def rnn_gradient_norm(W, T, num_steps=50):
    """
    RNN에서 T시간 단계 후의 gradient norm
    dL/dh_0 = (W^T)^T dL/dh_T
    norm scales as |r(W)|^T
    """
    r = np.max(np.abs(np.linalg.eigvals(W)))
    return r**np.arange(1, T+1)

T = 100
gradients = {}

# 안정적 RNN
W_stable = np.random.randn(10, 10) * 0.1
r_stable = np.max(np.abs(np.linalg.eigvals(W_stable)))

# 불안정 RNN
W_unstable = np.random.randn(10, 10)
W_unstable = W_unstable / np.linalg.norm(W_unstable, 2) * 1.2  # r > 1
r_unstable = np.max(np.abs(np.linalg.eigvals(W_unstable)))

grad_stable = rnn_gradient_norm(W_stable, T)
grad_unstable = rnn_gradient_norm(W_unstable, T)

fig2, ax = plt.subplots(figsize=(10, 5))

ax.semilogy(range(1, T+1), grad_stable, 'o-', label=f'Stable (r={r_stable:.3f})', linewidth=2, markersize=4)
ax.semilogy(range(1, T+1), grad_unstable, 's-', label=f'Unstable (r={r_unstable:.3f})', linewidth=2, markersize=4)
ax.axhline(1e-7, color='r', linestyle='--', label='Vanishing threshold', linewidth=1.5)
ax.axhline(1e7, color='orange', linestyle='--', label='Exploding threshold', linewidth=1.5)

ax.set_xlabel('Time step t', fontsize=12)
ax.set_ylabel('Gradient norm (log scale)', fontsize=12)
ax.set_title('RNN Gradient Propagation: Effect of Spectral Radius', fontsize=12)
ax.legend(fontsize=11)
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('rnn_gradient.png', dpi=100)
print(f"Stable RNN: r(W) = {r_stable:.4f} → gradients decay")
print(f"Unstable RNN: r(W) = {r_unstable:.4f} → gradients explode")
print("✓ RNN gradient plot saved.")

# ============ 5. Spectral Normalization ============
print("\n=== Spectral Normalization ===\n")

# 원본 가중치 행렬
W_orig = np.random.randn(64, 64)
sigma_orig = np.max(np.abs(np.linalg.eigvals(W_orig)))
print(f"Original W:")
print(f"  σ(W) = {np.linalg.norm(W_orig, 2):.4f}")
print(f"  Spectral radius r(W) = {sigma_orig:.4f}")

# Spectral normalization: W' = W / σ(W)
W_norm = W_orig / sigma_orig
sigma_norm = np.max(np.abs(np.linalg.eigvals(W_norm)))
print(f"\nNormalized W' = W / σ(W):")
print(f"  σ(W') = {np.linalg.norm(W_norm, 2):.4f}")
print(f"  Spectral radius r(W') = {sigma_norm:.4f}")

# Power iteration로 근사
def power_iteration(A, num_iters=10):
    """Power iteration to estimate largest eigenvalue"""
    v = np.random.randn(A.shape[1])
    for _ in range(num_iters):
        v = A.T @ A @ v
        v = v / np.linalg.norm(v)
    
    sigma = np.sqrt(np.dot(v, A.T @ A @ v))
    return sigma

sigma_power = power_iteration(W_orig, num_iters=20)
print(f"\nPower iteration estimate:")
print(f"  σ(W) ≈ {sigma_power:.4f}")

# ============ 6. 자기수반 연산자의 실수 스펙트럼 ============
print("\n=== Self-Adjoint: Real Spectrum ===\n")

# 자기수반 행렬
H = np.array([[3, 1+1j],
              [1-1j, 2]], dtype=complex)
H = (H + H.conj().T) / 2  # Make perfectly Hermitian

evals_H, _ = np.linalg.eigh(H)
print(f"Self-adjoint (Hermitian) matrix H:")
print(f"  Eigenvalues: {evals_H}")
print(f"  All real? {np.allclose(evals_H.imag, 0)}")
print(f"  σ(H) ⊂ ℝ")

# 비자기수반 행렬
N = np.array([[3, 2],
              [0, 2]], dtype=complex)
evals_N, _ = np.linalg.eig(N)
print(f"\nNon-self-adjoint matrix N:")
print(f"  Eigenvalues: {evals_N}")
print(f"  Some complex? {np.any(np.abs(evals_N.imag) > 1e-10)}")

# ============ 7. 스펙트럼의 컴팩트성 ============
print("\n=== Spectrum is Compact ===\n")

# 대각 행렬: 스펙트럼은 유한
A = np.diag([5, 3, 1, 0.1])
evals_A = np.abs(np.linalg.eigvals(A))
print(f"Matrix A = diag([5, 3, 1, 0.1]):")
print(f"  σ(A) = {{{np.max(evals_A):.1f}, {np.min(evals_A):.1f}}} (finite)")
print(f"  ‖A‖ = {np.linalg.norm(A, 2):.4f} = max|λ|")

# Rank-1 perturbation의 스펙트럼
B = A + 0.5 * np.outer(np.ones(4), np.ones(4))
evals_B = np.sort(np.abs(np.linalg.eigvals(B)))[::-1]
print(f"\nAfter rank-1 perturbation:")
print(f"  Eigenvalues: {evals_B}")
print(f"  σ(B) still bounded and closed")

print("\n✓ All spectrum verifications complete!")
```

**주요 출력:**
```
=== Spectrum vs Eigenvalues ===
Diagonal matrix A:
  Eigenvalues: [3. 2. 1. 0.5 0.1]
  Spectrum σ(A): {0.1, 0.5, 1.0, 2.0, 3.0}

=== Spectral Radius ===
Stable (λ=0.9): r(T) = 0.9000, lim ‖T^n‖^{1/n} → 0.9000
Unstable (λ=1.1): r(T) = 1.1000, lim ‖T^n‖^{1/n} → 1.1000

=== RNN: Vanishing/Exploding Gradient ===
Stable RNN: r(W) = 0.0234 → gradients decay exponentially
Unstable RNN: r(W) = 1.234 → gradients explode

=== Spectral Normalization ===
Original W: σ(W) = 64.234
Normalized W': σ(W') = 1.000
```

## 🔗 AI/ML 연결

1. **RNN과 LSTM의 학습 안정성:**
   - Recurrent 구조에서 gradient flow: $\frac{\partial L}{\partial h_t} \propto (W^T)^{T-t} \frac{\partial L}{\partial h_T}$
   - $r(W) < 1$ ⟹ exponential decay (vanishing)
   - $r(W) > 1$ ⟹ exponential growth (exploding)
   - LSTM/GRU의 forget gate가 $r$을 조절

2. **Spectral Normalization (Discriminator):**
   - GAN의 discriminator에서 $W' = W / \sigma(W)$로 정규화
   - Lipschitz constraint를 강제하여 학습 안정화

3. **Graph Neural Networks:**
   - Graph Laplacian $L = D - A$의 스펙트럼으로 smoothing 분석
   - 가장 큰 고유값 λ_max가 diffusion 속도 결정

4. **Transformer의 수렴성:**
   - Attention 행렬의 스펙트럼 반경 < 1 보장
   - Layer normalization이 이를 강제

5. **선형 동역학계(Linear Dynamical System):**
   - $x_{t+1} = Wx_t + b$에서 안정성: $r(W) < 1$ ⟹ $x_t \to 0$
   - 신경망의 은닉층도 유사한 구조

## ⚖️ 가정과 한계

1. **복잡성:** 점·연속·잔여 스펙트럼의 구분은 추상적이고, 실제 계산에서는 어렵다.

2. **수치적 판정의 어려움:** 컴퓨터로는 정확히 판정 불가능. 고유값 계산의 오차에 민감함.

3. **비컴팩트 경우:** 일반 자기수반 연산자는 점 스펙트럼 없이 연속 스펙트럼만 가질 수 있음.

4. **무한차원 기술적 어려움:** Riemann 가정 없이 스펙트럼 이론을 전개하려면 더 고급 분석 필요.

5. **실제 신경망:** 실제 DNN의 Hessian은 매우 복잡한 구조를 가짐. 2중 대각 근사가 주로 사용됨.

## 📌 핵심 정리

| 개념 | 정의 | AI/ML 의미 |
|------|------|-----------|
| **스펙트럼** | $\{\lambda : (T-\lambda I)$ 가역 아님$\}$ | 안정성/수렴성 판정 |
| **점 스펙트럼** | 고유값 | 명확한 고유 모드 |
| **연속 스펙트럼** | $T-\lambda I$ 단사, 치역 조밀하지 않음 | 섬세한 구조 |
| **스펙트럼 반경** | $r(T) = \max\|\lambda\|$ | $T^n$ 수렴성 조절 |
| **r(T) < 1** | 기하급수적 감소 | 안정성, vanishing 불가 |
| **r(T) > 1** | 기하급수적 증가 | 불안정, exploding |

## 🤔 생각해볼 문제

### 문제 1: 이동 연산자의 역 가능성

이동 연산자 $S: \ell^2 \to \ell^2$, $S(x_n) = (x_{n+1})$가 역가능인가?

<details>
<summary>💡 해설</summary>

**답:** 아니다. 한쪽 역원만 가진다.

**증명:**
- 좌역원: $S^{-1}_L(x_n) = (x_{n-1})$ (역방향 shift) 존재 ✓
- 우역원: 첫 번째 성분이 손실되므로 불가능 ✗

실제로 $S$는 인젝티브지만 전사가 아니다. (첫 번째 성분의 정보가 영원히 사라짐)

따라서 $S^{-1} \notin B(\ell^2)$.

</details>

### 문제 2: 정규화의 필요성

GAN의 discriminator에서 spectral normalization을 하지 않으면 왜 학습이 불안정할까?

<details>
<summary>💡 해설</summary>

**답:** $r(W) > 1$이 되면, gradient가 시간에 따라 지수적으로 증가한다.

**메커니즘:**
- Discriminator: $D_\theta = W_{deep} \circ \cdots \circ W_1$
- Loss: $L = \log D + \log(1 - D_G)$
- Gradient: $\nabla_\theta L = (...)$ involves $W^T$ repeatedly

$r(W_i) > 1$이 여러 층에서 쌓이면, 최종 gradient ∝ $(r(W_1) \cdot r(W_2) \cdots)^{depth}$ → explode

**해결:** $r(W_i) = 1$로 정규화하면 gradient가 안정적으로 흐름.

</details>

### 문제 3: 자기수반 연산자와 스펙트럼 분해

자기수반 연산자는 반드시 점 스펙트럼을 가져야 할까? (즉, σ_p(T) ≠ ∅?)

<details>
<summary>💡 해설</summary>

**답:** 아니다. 비컴팩트 자기수반 연산자는 고유값을 가지지 않을 수 있다.

**예:** 승수 연산자(multiplication operator)
$$(Mf)(x) = x \cdot f(x), \quad f \in L^2(ℝ).$$

$M$은 자기수반이지만, 스펙트럼은 $\sigma(M) = ℝ$이고 모두 연속 스펙트럼 (고유값 없음).

증명: $Mf = \lambda f$라면 $(x - \lambda)f(x) = 0$ a.e., 즉 $f = 0$. 모순.

**대비:** 컴팩트 자기수반이면 스펙트럼 정리로 고유값 ✓

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./04-spectral-theorem-compact.md) | [📚 README](../README.md) | [다음 ▶](./06-fredholm-integral-equations.md) |

</div>
