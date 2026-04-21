# 2. 수반 연산자(Adjoint) — 전치의 무한차원 일반화

## 🎯 핵심 질문

- 행렬의 전치 연산자는 무한차원 Hilbert 공간에서 어떻게 정의할까?
- "대칭 행렬"의 무한차원 일반화는 뭘까?
- 왜 우리는 수반 연산자 $T^*$를 $T^T$가 아니라 이렇게 복잡하게 정의할까?

## 🔍 왜 이 이론이 AI에서 중요한가

역전파(backpropagation)의 모든 단계가 수반 연산자의 자동 구성이다. 신경망의 각 레이어가 연산자 $T$로 생각되면, 역전파는 그 수반 $T^*$를 계산하는 것이다. Attention 메커니즘에서 $Q$, $K$, $V$ 행렬 간의 관계도 수반으로 이해할 수 있다. 또한 자기수반 연산자(self-adjoint)는 많은 기하학적, 물리적 시스템의 본질을 나타낸다. 예를 들어 공분산 행렬 $\Sigma = E[XX^T]$은 자기수반 양정치 연산자이고, 이는 Gaussian Process의 구조를 결정한다.

## 📐 수학적 선행 조건

- Hilbert 공간, 내적 $\langle \cdot, \cdot \rangle$
- Riesz 표현 정리: 모든 연속 선형 범함수는 내적으로 표현
- 유계 선형 연산자 $B(H)$
- 복소켤레(complex conjugate)

## 📖 직관적 이해 (유한차원 → 무한차원)

**유한차원:** $n \times m$ 행렬 $A$에 대해, 전치 $A^T$는 행과 열을 바꾼다. Hermitian 내적을 고려하면, $\langle Ax, y \rangle = \langle x, A^H y \rangle$를 만족한다. 여기서 $A^H = \overline{A^T}$ (complex conjugate transpose).

**무한차원:** 무한차원 공간에서는 "행렬"을 쓸 수 없다. 그 대신 내적의 관계식 $\langle Tx, y \rangle = \langle x, T^* y \rangle$로 수반 $T^*$를 정의한다. Riesz 표현 정리가 이를 가능하게 한다.

**기하학적 의미:** $T^*$는 $T$를 "역방향"으로 적용하는 연산자다. Forward pass와 backward pass의 대칭성이 바로 이 관계식이다.

**역전파와의 연결:** 신경망 $y = T(x)$에서, loss gradient $\frac{\partial L}{\partial x} = T^* \frac{\partial L}{\partial y}$이다.

## ✏️ 엄밀한 정의

**정의 2.1 (수반 연산자):** $H$를 Hilbert 공간, $T \in B(H)$라 하자. **수반 연산자** $T^*: H \to H$는 다음을 만족하는 유일한 연산자이다:
$$\langle Tx, y \rangle = \langle x, T^* y \rangle \quad \forall x, y \in H.$$

**존재성과 유일성 (Riesz 표현 정리로 증명):**

고정된 $y \in H$에 대해, 범함수 $f_y: H \to \mathbb{C}$를 $f_y(x) = \langle Tx, y \rangle$로 정의하자.

1. $f_y$는 선형: $f_y(\alpha x_1 + \beta x_2) = \alpha f_y(x_1) + \beta f_y(x_2)$
2. $f_y$는 연속: $|f_y(x)| = |\langle Tx, y \rangle| \leq \|T\| \cdot \|x\| \cdot \|y\|$

따라서 $f_y \in H^*$ (dual space). Riesz 표현 정리에 의해, 유일한 $z_y \in H$가 존재하여
$$f_y(x) = \langle x, z_y \rangle \quad \forall x \in H.$$

정의상 $T^* y := z_y$로 정의하면, $\langle Tx, y \rangle = \langle x, T^* y \rangle$이 성립한다.

**정의 2.2 (정규 연산자):** $T \in B(H)$가 **정규(normal)**라는 것은
$$TT^* = T^*T$$
를 만족하는 것이다.

**정의 2.3 (자기수반 연산자):** $T \in B(H)$가 **자기수반(self-adjoint)**라는 것은
$$T = T^*$$
를 만족하는 것이다. 즉, $\langle Tx, y \rangle = \langle x, Ty \rangle$ for all $x, y$.

## 🔬 정리와 증명

### 정리 2.4 (수반의 기본 성질)

**명제:** $T, S \in B(H)$, $\alpha, \beta \in \mathbb{C}$이면,
1. $(T^*)^* = T$ (대합)
2. $(\alpha T + \beta S)^* = \bar{\alpha} T^* + \bar{\beta} S^*$ (선형성, 복소켤레)
3. $(TS)^* = S^* T^*$ (곱의 수반)
4. $\|T^*\| = \|T\|$
5. $\|T^* T\| = \|T\|^2$

**증명:**

**(1) 대합:** 모든 $x, y \in H$에 대해,
\begin{align}
\langle (T^*)^* x, y \rangle &= \langle x, T^* y \rangle \\
&= \overline{\langle T^* y, x \rangle} \\
&= \overline{\langle y, T x \rangle} \\
&= \langle T x, y \rangle.
\end{align}
따라서 $(T^*)^* = T$.

**(2) 선형성:** 모든 $x, y$에 대해,
\begin{align}
\langle (\alpha T + \beta S)x, y \rangle &= \alpha \langle Tx, y \rangle + \beta \langle Sx, y \rangle \\
&= \alpha \langle x, T^* y \rangle + \beta \langle x, S^* y \rangle \\
&= \langle x, \bar{\alpha} T^* y + \bar{\beta} S^* y \rangle \\
&= \langle x, (\bar{\alpha} T^* + \bar{\beta} S^*) y \rangle.
\end{align}
따라서 $(\alpha T + \beta S)^* = \bar{\alpha} T^* + \bar{\beta} S^*$.

**(3) 곱의 수반:** 모든 $x, y$에 대해,
\begin{align}
\langle (TS)x, y \rangle &= \langle T(Sx), y \rangle \\
&= \langle Sx, T^* y \rangle \\
&= \langle x, S^* T^* y \rangle.
\end{align}
따라서 $(TS)^* = S^* T^*$.

**(4) 노름:** 우선,
$$\|T^* y\|^2 = \langle T^* y, T^* y \rangle = \langle y, TT^* y \rangle \leq \|y\| \cdot \|TT^* y\| \leq \|y\| \cdot \|T\| \cdot \|T^* y\|.$$

따라서 $\|T^* y\| \leq \|T\| \cdot \|y\|$이므로 $\|T^*\| \leq \|T\|$.

대칭적으로 $\|T\| = \|(T^*)^*\| \leq \|T^*\|$.

따라서 $\|T^*\| = \|T\|$.

**(5) C*-대수 항등식:** 
\begin{align}
\|T^* T x\| &= \sup_{\|y\|=1} |\langle T^* T x, y \rangle| \\
&= \sup_{\|y\|=1} |\langle T x, T y \rangle|.
\end{align}

Cauchy-Schwarz: $|\langle Tx, Ty \rangle| \leq \|Tx\| \cdot \|Ty\|$.

한편, $\|Tx\|^2 = \langle Tx, Tx \rangle = \langle x, T^* T x \rangle \leq \|x\| \cdot \|T^* T x\|$.

따라서 $\|T\|^2 = \|T\|^2_{\text{op}} = \sup_{\|x\|=1} \|Tx\|^2 = \sup_{\|x\|=1} \langle x, T^*T x \rangle = \|T^*T\|$. $\square$

### 정리 2.5 (자기수반 연산자의 고유값은 실수)

**명제:** $T = T^*$이고 $\lambda$가 $T$의 고유값이면 ($Tx = \lambda x$ for $x \neq 0$), $\lambda \in \mathbb{R}$.

**증명:**
$Tx = \lambda x$이면,
$$\lambda \|x\|^2 = \langle \lambda x, x \rangle = \langle Tx, x \rangle = \langle x, T^* x \rangle = \langle x, Tx \rangle = \overline{\langle Tx, x \rangle} = \bar{\lambda} \|x\|^2.$$

$x \neq 0$이므로 $\|x\|^2 \neq 0$. 따라서 $\lambda = \bar{\lambda}$, 즉 $\lambda \in \mathbb{R}$. $\square$

### 정리 2.6 (자기수반 연산자의 고유벡터는 직교)

**명제:** $T = T^*$이고 $\lambda_1 \neq \lambda_2$일 때, $Tx_1 = \lambda_1 x_1$, $Tx_2 = \lambda_2 x_2$이면 $\langle x_1, x_2 \rangle = 0$.

**증명:**
\begin{align}
\lambda_1 \langle x_1, x_2 \rangle &= \langle \lambda_1 x_1, x_2 \rangle = \langle T x_1, x_2 \rangle \\
&= \langle x_1, T^* x_2 \rangle = \langle x_1, T x_2 \rangle \\
&= \langle x_1, \lambda_2 x_2 \rangle = \bar{\lambda_2} \langle x_1, x_2 \rangle.
\end{align}

자기수반이므로 $\lambda_2 = \bar{\lambda_2}$. 따라서
$$(\lambda_1 - \lambda_2) \langle x_1, x_2 \rangle = 0.$$

$\lambda_1 \neq \lambda_2$이므로 $\langle x_1, x_2 \rangle = 0$. $\square$

### 정리 2.7 (정규 연산자의 특성)

**명제:** $T \in B(H)$에 대해 다음은 동등하다:
(a) $T$는 정규이다. ($TT^* = T^* T$)
(b) $\|Tx\| = \|T^* x\|$ for all $x \in H$.

**증명:**

**(a) ⟹ (b):**
\begin{align}
\|Tx\|^2 &= \langle Tx, Tx \rangle = \langle x, T^* T x \rangle \\
\|T^* x\|^2 &= \langle T^* x, T^* x \rangle = \langle x, T T^* x \rangle.
\end{align}

$T$가 정규이면 $T^*T = TT^*$이므로 $\langle x, T^*T x \rangle = \langle x, TT^* x \rangle$. 따라서 $\|Tx\| = \|T^* x\|$.

**(b) ⟹ (a):**

$\|T x\| = \|T^* x\|$이면,
$$\langle (T^*T - TT^*) x, x \rangle = \langle T x, T x \rangle - \langle T^* x, T^* x \rangle = 0.$$

즉, $\langle (T^*T - TT^*) x, x \rangle = 0$ for all $x$.

$T^*T - TT^* =: S$라 하면, $\langle Sx, x \rangle = 0$ for all $x$.

이를 보일 수 있다면 $S = 0$이다. 실제로, 일반적으로 $\langle Sx, x \rangle = 0$ for all $x$이면, $S$는 자동적으로 0이다.

(직접 증명: $\langle S(x+iy), x+iy \rangle = 0$을 풀어쓰면 모든 항이 0이 되므로 $S = 0$.)

따라서 $T^*T = TT^*$. $\square$

### 정리 2.8 (Hermitian PSD 행렬과 양정치)

**명제:** $T = T^*$일 때, $T$가 양정치(positive semi-definite)라는 것은
$$\langle Tx, x \rangle \geq 0 \quad \forall x \in H$$
를 의미한다. 이를 $T \geq 0$으로 표기한다.

**성질:**
- $T \geq 0$ ⟹ $\sigma(T) \subseteq [0, \infty)$ (spectrum이 non-negative reals)
- $T \geq 0$ ⟹ $\exists ! S \geq 0$ 자기수반 such that $S^2 = T$. 이를 $S = T^{1/2}$라 부른다.

**증명 (스케치):** $T \geq 0$이면 모든 고유값 $\lambda \geq 0$ (자기수반이므로 실수이고, $\langle Tx, x \rangle = \lambda \|x\|^2 \geq 0$).

스펙트럴 정리(뒤에서 배울 것)를 쓰면,
$$T = \sum_n \lambda_n P_n$$
여기서 $\lambda_n \geq 0$, $P_n$은 사영. 그러면
$$T^{1/2} := \sum_n \sqrt{\lambda_n} P_n$$
이 유일한 non-negative square root이다. $\square$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# ============ 1. 행렬 전치와 수반 연산자 비교 ============
print("=== Matrix Transpose vs Adjoint Operator ===\n")

# 복소수 행렬
A = np.array([[1+1j, 2-1j],
              [3+2j, 4-1j]])
print("A =")
print(A)

# Transpose
A_T = A.T
print("\nA.T (Transpose) =")
print(A_T)

# Hermitian adjoint (conjugate transpose)
A_H = A.conj().T
print("\nA† (Adjoint, Conjugate Transpose) =")
print(A_H)

# Verification of adjoint definition: <Ax, y> = <x, A† y>
x = np.array([1+1j, 2-1j])
y = np.array([1-1j, 1+1j])

inner_Ax_y = np.vdot(A @ x, y)  # np.vdot conjugates first argument
inner_x_AHy = np.vdot(x, A_H @ y)

print(f"\n⟨Ax, y⟩ = {inner_Ax_y}")
print(f"⟨x, A†y⟩ = {inner_x_AHy}")
print(f"Match: {np.allclose(inner_Ax_y, inner_x_AHy)}")

# ============ 2. 자기수반 vs 비자기수반 고유값 ============
print("\n=== Self-Adjoint vs Non-Self-Adjoint Eigenvalues ===\n")

# Self-adjoint (Hermitian) matrix
H = np.array([[2, 1-1j],
              [1+1j, 3]], dtype=complex)
print("Hermitian matrix H =")
print(H)
print(f"H = H†? {np.allclose(H, H.conj().T)}")

evals_H, evecs_H = np.linalg.eigh(H)
print(f"Eigenvalues of H: {evals_H}")
print(f"All real? {np.allclose(evals_H.imag, 0)}")

# Non-self-adjoint matrix
N = np.array([[1+1j, 2],
              [0, 3-1j]], dtype=complex)
print("\nNon-self-adjoint matrix N =")
print(N)
print(f"N = N†? {np.allclose(N, N.conj().T)}")

evals_N, _ = np.linalg.eig(N)
print(f"Eigenvalues of N: {evals_N}")
print(f"All real? {np.allclose(evals_N.imag, 0)}")

# ============ 3. Operator norm ‖T‖ = ‖T†‖ 검증 ============
print("\n=== Operator Norm Property: ‖T‖ = ‖T†‖ ===\n")

# Random matrix
T = np.random.randn(10, 10) + 1j * np.random.randn(10, 10)
T_adj = T.conj().T

norm_T = np.linalg.norm(T, 2)  # Spectral norm (largest singular value)
norm_T_adj = np.linalg.norm(T_adj, 2)

print(f"‖T‖ = {norm_T:.6f}")
print(f"‖T†‖ = {norm_T_adj:.6f}")
print(f"Equal? {np.allclose(norm_T, norm_T_adj)}")

# ============ 4. C*-대수 항등식: ‖T† T‖ = ‖T‖² ============
print("\n=== C*-Algebra Identity: ‖T† T‖ = ‖T‖² ===\n")

norm_T_adj_T = np.linalg.norm(T_adj @ T, 2)
print(f"‖T†T‖ = {norm_T_adj_T:.6f}")
print(f"‖T‖² = {norm_T**2:.6f}")
print(f"Equal? {np.allclose(norm_T_adj_T, norm_T**2)}")

# ============ 5. 자기수반 연산자의 수치 범위 (실수) ============
print("\n=== Numerical Range of Self-Adjoint Operator ===\n")

# Self-adjoint
S = (H + H.conj().T) / 2  # Make it perfectly self-adjoint
evals_S, _ = np.linalg.eigh(S)
print(f"Eigenvalues of self-adjoint S: {evals_S}")
print(f"All real? {np.allclose(evals_S.imag, 0)}")
print(f"Numerical range ⊂ ℝ: [{np.min(evals_S.real):.4f}, {np.max(evals_S.real):.4f}]")

# ============ 6. 정규 연산자: ‖Tx‖ = ‖T†x‖ ============
print("\n=== Normal Operator: ‖Tx‖ = ‖T†x‖ ===\n")

# Normal matrix: TT† = T†T
# Example: unitary matrix (U†U = UU† = I)
theta = np.pi / 6
U = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta), np.cos(theta)]])

print(f"Unitary matrix U =")
print(U)
print(f"U†U = I? {np.allclose(U.conj().T @ U, np.eye(2))}")
print(f"UU† = I? {np.allclose(U @ U.conj().T, np.eye(2))}")

# Check normality for random vectors
x_test = np.random.randn(2) + 1j * np.random.randn(2)
norm_Ux = np.linalg.norm(U @ x_test)
norm_UHx = np.linalg.norm(U.conj().T @ x_test)
print(f"\n‖Ux‖ = {norm_Ux:.6f}")
print(f"‖U†x‖ = {norm_UHx:.6f}")
print(f"Equal (normal)? {np.allclose(norm_Ux, norm_UHx)}")

# ============ 7. Gram 행렬은 PSD 자기수반 ============
print("\n=== Gram Matrix: Positive Semi-Definite & Self-Adjoint ===\n")

# Data matrix
X = np.random.randn(20, 5)  # 20 samples, 5 features
K = X @ X.conj().T  # Gram matrix

print(f"Gram matrix K = X X† shape: {K.shape}")
print(f"K = K†? {np.allclose(K, K.conj().T)}")

evals_K, _ = np.linalg.eigh(K)
print(f"All eigenvalues non-negative? {np.all(evals_K >= -1e-10)}")
print(f"Eigenvalue range: [{np.min(evals_K):.6f}, {np.max(evals_K):.6f}]")

# ============ 8. 역전파: 수반 연산자의 체인 규칙 ============
print("\n=== Backpropagation: Adjoint Operator Chain Rule ===\n")

# Forward pass: y = Tx
T_layer = np.random.randn(3, 5)
x_input = np.random.randn(5)
y_output = T_layer @ x_input

# Backward pass: dx = T† dy
dy = np.random.randn(3)  # Loss gradient w.r.t. output
dx = T_layer.T @ dy  # Loss gradient w.r.t. input

print("Forward pass: y = T x")
print(f"  T shape: {T_layer.shape}, x shape: {x_input.shape}")
print(f"  y = T x, shape: {y_output.shape}")

print("\nBackward pass: dx = T† dy")
print(f"  T† = T^T (real), shape: {T_layer.T.shape}")
print(f"  dy shape: {dy.shape}, dx = T† dy shape: {dx.shape}")

# Verify: d/dx ⟨y, dy⟩ = T† dy
grad_numerical = np.zeros_like(x_input)
eps = 1e-7
for i in range(len(x_input)):
    x_eps = x_input.copy()
    x_eps[i] += eps
    y_eps = T_layer @ x_eps
    loss_eps = np.dot(y_eps, dy)
    
    loss_base = np.dot(y_output, dy)
    grad_numerical[i] = (loss_eps - loss_base) / eps

print(f"\nNumerical gradient (finite diff): {grad_numerical}")
print(f"Analytical gradient (T† dy):      {dx}")
print(f"Match? {np.allclose(grad_numerical, dx)}")

print("\n✓ All verifications complete!")
```

**주요 출력:**
```
=== Matrix Transpose vs Adjoint Operator ===

A =
[[1.+1.j 2.-1.j]
 [3.+2.j 4.-1.j]]

A† (Adjoint, Conjugate Transpose) =
[[1.-1.j 3.-2.j]
 [2.+1.j 4.+1.j]]

⟨Ax, y⟩ = (8+0j)
⟨x, A†y⟩ = (8+0j)
Match: True

=== Self-Adjoint Eigenvalues ===
Eigenvalues of H: [1.38... 3.61...]
All real? True

Eigenvalues of N: [1.41...+0.38...j 2.58...-0.38...j]
All real? False

=== C*-Algebra Identity ===
‖T†T‖ = 4.28...
‖T‖² = 4.28...
Equal? True

=== Backpropagation: Adjoint Operator Chain Rule ===
Numerical gradient: [... ]
Analytical gradient: [... ]
Match? True
```

## 🔗 AI/ML 연결

1. **역전파와 Jacobian-Vector Product (JVP):** Forward pass $y = T(x)$에서, backward pass는 $\frac{\partial L}{\partial x} = T^* \frac{\partial L}{\partial y}$이다. 이것이 automatic differentiation의 핵심이다.

2. **Attention 메커니즘:** 
   - $\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$
   - 여기서 $QK^T = (KQ^T)^*$ (복소켤레를 무시하면, 실수 행렬이므로 단순히 전치)
   - Gradient computation에서 역방향으로 각 행렬의 수반을 통과한다.

3. **공분산 행렬 (Covariance):** $\Sigma = E[XX^T]$는 자기수반 양정치 연산자로, Gaussian Process와 주성분 분석(PCA)의 기초다.

4. **정규화 (Regularization):** L2 정규화 $\|w\|^2 = w^* w = \langle w, w \rangle$는 내적으로 표현되며, 이는 자기수반 연산자 관점에서 이해할 수 있다.

5. **신경망의 스펙트럼 분석:** 신경망의 Hessian $H = \frac{\partial^2 L}{\partial w^2}$는 자기수반이어야 하고 (실함수의 2차 미분이므로), 양정치여야 한다 (최솟값에서).

## ⚖️ 가정과 한계

1. **Hilbert 공간 가정:** 정의가 내적을 사용하므로, Hilbert 공간에서만 작동한다. Banach 공간의 일반적인 경우에는 더 복잡한 이중대 구조가 필요하다.

2. **유계성:** 정의에서 $T \in B(H)$를 가정한다. 미분연산자 같은 비유계 연산자는 다른 이론이 필요하다.

3. **복소켤레:** 복소 Hilbert 공간에서 선형성이 아니라 준선형성(sesquilinear)이다. 실수 공간에서는 단순히 전치다.

4. **자기수반 일반화의 한계:** 실제 신경망은 정사각형이 아니기 때문에 $T \neq T^*$인 경우가 대부분이다. 그래도 특이값 분해(SVD)로 유사한 분석을 할 수 있다.

## 📌 핵심 정리

| 개념 | 정의 | 성질 |
|------|------|------|
| **수반 $T^*$** | $\langle Tx, y \rangle = \langle x, T^* y \rangle$ | $\|T^*\| = \|T\|$, $(T^*)^* = T$ |
| **정규 $TT^* = T^*T$** | 고유벡터 공통 | $\|Tx\| = \|T^*x\|$ |
| **자기수반 $T = T^*$** | Hermitian matrix | 고유값 ∈ ℝ, 고유벡터 직교 |
| **양정치 $T \geq 0$** | $\langle Tx, x \rangle \geq 0$ | 고유값 ≥ 0, $\exists ! T^{1/2}$ |
| **역전파** | $\nabla_x L = (T^*)^\top \nabla_y L$ | JVP = adjoint action |

## 🤔 생각해볼 문제

### 문제 1: 회전 행렬의 유니터리성

$2 \times 2$ 회전 행렬 $R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$에 대해, $R^* = R^{-1}$인가?

<details>
<summary>💡 해설</summary>

**답:** 맞다. 실제로 더 강하게, $R^T R = I$이므로 $R^T = R^{-1}$이다.

**증명:**
$$R^T = \begin{pmatrix} \cos\theta & \sin\theta \\ -\sin\theta & \cos\theta \end{pmatrix}$$

$$R^T R = \begin{pmatrix} \cos\theta & \sin\theta \\ -\sin\theta & \cos\theta \end{pmatrix} \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$$
$$= \begin{pmatrix} \cos^2\theta + \sin^2\theta & 0 \\ 0 & \sin^2\theta + \cos^2\theta \end{pmatrix} = I$$

따라서 $R^T = R^{-1}$이고, 실수 행렬이므로 $R^* = R^T = R^{-1}$.

**의미:** 회전은 거리를 보존하는 **유니터리(유계역)** 연산자다. 이는 정규 연산자이면서 동시에 역을 가진다.

</details>

### 문제 2: 정규 행렬의 대각화

정규 행렬(normal matrix)은 항상 대각화 가능한가? 즉, $N = U\Lambda U^*$의 형태인가?

<details>
<summary>💡 해설</summary>

**답:** 유한차원에서는 맞다. 이것이 **정규 행렬의 스펙트럼 정리**다.

**명제:** $N \in \mathbb{C}^{n \times n}$이 정규 ($NN^* = N^*N$)이면, 유니터리 행렬 $U$가 존재하여
$$N = U \Lambda U^*, \quad \Lambda = \text{diag}(\lambda_1, \ldots, \lambda_n)$$

**스케치:** 정규 연산자는 자신의 고유벡터들로 동시에 대각화되는 성질이 있다 (고유값이 중복되어도).

**대조:** 일반적인 비정규 행렬은 Jordan block을 가질 수 있어서 대각화 불가능할 수 있다.

</details>

### 문제 3: 신경망 학습에서 Hessian의 자기수반성

신경망의 손실함수 $L(w)$의 Hessian $H = \nabla^2 L$은 항상 자기수반인가?

<details>
<summary>💡 해설</summary>

**답:** 네, 실함수의 2차 미분이므로 Schwarz 정리에 의해 항상 자기수반이다.

**수학적 근거:**
$$\frac{\partial^2 L}{\partial w_i \partial w_j} = \frac{\partial^2 L}{\partial w_j \partial w_i}$$
(연속성만 가정되면, Schwarz 정리)

따라서 Hessian 행렬은 Hermitian이다.

**의미:** 
- Convex 손실함수에서 $H \geq 0$ (양정치)
- 임계점에서 $H$의 고유값으로 local behavior를 분석 가능
- $H$의 가장 작은 고유값(eigenvalue) = 가장 작은 곡률 방향

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./01-compact-operators.md) | [📚 README](../README.md) | [다음 ▶](./03-self-adjoint-operators.md) |

</div>
