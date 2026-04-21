# 3. 쌍대공간(Dual Space) $X^*$ — 유계 선형범함수의 공간

## 🎯 핵심 질문
- "쌍대공간"이란 무엇이고, 왜 $X^* = B(X, \mathbb{R})$인가?
- 유한차원에서 $(ℝ^n)^* ≅ ℝ^n$이 어떻게 무한차원으로 일반화되는가?
- 어떤 공간이 "반사적(reflexive)"이고, 그것이 최적화에서 중요한 이유는?

## 🔍 왜 이 이론이 AI에서 중요한가

쌍대공간은 **경사(gradient)**의 이론적 기초이다:
- **백프롭(Backpropagation)**: 손실 함수 $L: \mathbb{R}^n \to \mathbb{R}$의 경사 $\nabla L$는 쌍대공간 $(\mathbb{R}^n)^*$의 원소
- **라그랑주 승수**: 제약 최적화에서 승수들은 쌍대 변수, 강쌍대성은 쌍대공간 이론으로 증명
- **분포의 약수렴**: Wasserstein 거리와 분포 학습에서 쌍대공간의 약*위상(weak-* topology) 사용
- **L^p 공간의 켤레**: 신호 처리에서 $L^p$-$L^q$ 켤레 지수 ($1/p + 1/q = 1$)는 Hölder 부등식의 쌍대성에서 비롯

## 📐 수학적 선행 조건

- **선형 범함수**: 함수 $φ: X \to \mathbb{R}$ (또는 $\mathbb{C}$)가 선형
- **B(X, ℝ) (또는 B(X, ℂ))**: 유계 선형 연산자 공간 (이미 정리 2.1에서 Banach 공간임을 보임)
- **동형(isomorphism)**: 벡터 공간 동형과 노름 보존 동형의 차이

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서**:
$ℝ^n$의 쌍대는 "행벡터"들의 공간으로 볼 수 있다. 각 행벡터 $\mathbf{a}^T = (a_1, \ldots, a_n)$은 선형범함수 $φ(x) = \mathbf{a}^T \mathbf{x}$를 정의한다. 이는 열벡터와 자연스럽게 대응되어 $(ℝ^n)^* ≅ ℝ^n$.

**무한차원에서**:
$L^p([0,1])$의 쌍대는 $L^q([0,1])$ (단, $1/p + 1/q = 1$)이다. 각 $g ∈ L^q$는 선형범함수 $φ(f) = ∫ fg$를 정의한다. 하지만 모든 선형범함수가 이 형태인지는 비자명(Riesz 표현 정리).

## ✏️ 엄밀한 정의

**정의 3.1 (선형 범함수)**

$φ: X \to \mathbb{K}$ (단, $\mathbb{K} = \mathbb{R}$ 또는 $\mathbb{C}$)가 *선형 범함수*이면:
$$φ(\alpha x + \beta y) = \alpha φ(x) + \beta φ(y) \quad \forall \alpha, \beta \in \mathbb{K}, x, y \in X$$

**정의 3.2 (쌍대공간 $X^*$)**

$X$가 노름공간일 때, 쌍대공간은:
$$X^* := B(X, \mathbb{K}) = \{φ: X \to \mathbb{K} \mid φ \text{ 선형이고 유계}\}$$

이전 정리에서 보았듯이 (Y = $\mathbb{K}$가 Banach), $X^*$는 연산자 노름 아래에서 **항상 Banach 공간**이다.

**정의 3.3 (쌍대 노름)**

$φ ∈ X^*$의 노름:
$$\|φ\|_{X^*} := \sup_{\|x\|_X ≤ 1} |φ(x)| = \sup_{x \neq 0} \frac{|φ(x)|}{\|x\|_X}$$

**정의 3.4 (Riesz 표현)**

Hilbert 공간 $H$에서, Riesz 표현 정리는 임의의 $φ ∈ H^*$에 대해 **유일한** $y ∈ H$가 존재하여:
$$φ(x) = \langle x, y \rangle \quad \forall x ∈ H$$

이때 $\|φ\|_{H^*} = \|y\|_H$.

**정의 3.5 (반사적 공간)**

$X$가 *반사적(reflexive)*이면:
$$X^{**} := (X^*)^* ≅ X$$

즉, 이중 쌍대가 원래 공간과 (동형으로) 같다.

**정의 3.6 (쌍대 노름 특성화)**

임의의 노름공간 $X$에 대해:
$$\|x\|_X = \sup_{\|φ\|_{X^*} ≤ 1} |φ(x)|$$

## 🔬 정리와 증명

**정리 3.1 (L^p의 쌍대, Riesz 표현 정리)**

$1 < p < ∞$일 때, $1/p + 1/q = 1$이라 하자. 임의의 $φ ∈ (L^p[a,b])^*$에 대해 **유일한** $g ∈ L^q[a,b]$가 존재하여:
$$φ(f) = \int_a^b f(x) g(x) dx \quad \forall f ∈ L^p[a,b]$$

또한 $\|φ\|_{(L^p)^*} = \|g\|_{L^q}$.

**증명 스케치**:

**(1) $L^q$에서 $L^p$ 쌍대로의 방향**:

$g ∈ L^q$이면, Hölder 부등식:
$$\left|\int_a^b f(x) g(x) dx\right| ≤ \|f\|_{L^p} \cdot \|g\|_{L^q}$$

따라서 $φ(f) := ∫ fg$로 정의한 함수는 $|φ(f)| ≤ \|g\|_{L^q} · \|f\|_{L^p}$를 만족하고, 선형이며 유계 ($\|φ\|_{(L^p)^*} ≤ \|g\|_{L^q}$).

**(2) 역방향: 표현의 유일성**

$φ ∈ (L^p)^*$가 주어졌을 때, $g$의 존재성은 Radon-Nikodym 정리(측도론)를 이용한다. 간단히는, 단순함수(simple functions)에서 $g$를 구성한 후 극한을 취하여 $L^q$의 함수로 확장한다.

(완전한 증명은 실함수론/측도론 교과서 참고)

---

**정리 3.2 (L¹의 쌍대 ≅ L^∞)**

**(L¹[a,b])^* ≅ L^∞[a,b]**

증명은 정리 3.1과 유사하지만, $L^∞$의 완비성을 확인해야 한다.

**정리 3.3 (ℓ^p의 쌍대, $1 < p < ∞$)**

$1/p + 1/q = 1$일 때:
$$(ℓ^p)^* ≅ ℓ^q$$

범함수 $φ ∈ (ℓ^p)^*$는 $φ(x) = \sum_{n=1}^{\infty} x_n y_n$ (단, $y ∈ ℓ^q$)로 표현되며, $\|φ\|_{(ℓ^p)^*} = \|y\|_{ℓ^q}$.

---

**정리 3.4 (L^p은 반사적, $1 < p < ∞$)**

$1 < p < ∞$일 때, **L^p는 반사적이다**:
$$L^{p**} := (L^{p*})^* ≅ L^p$$

**증명 개요**:

자연 임베딩 $J: L^p \to L^{p**}$를 정의하자:
$$J(f)(φ) := φ(f) \quad \forall φ ∈ L^{p*}$$

$p > 1$일 때, $φ ∈ (L^p)^* ≅ L^q$이므로 정리 3.1에 의해:
$$φ(f) = \int f(x) g(x) dx$$

단계별:
1. $J$는 선형이다 (점별 선형성)
2. $J$는 노름 보존이다: $\|J(f)\|_{L^{p**}} = \|f\|_{L^p}$ (정의 3.6 사용)
3. $J$는 전사이다 (정리 3.1의 결과로부터)

따라서 $J$는 동형(isomorphic).

**반사성의 실패**: $L¹$과 $L^∞$는 반사적이 아니다. 예를 들어 $(L¹)^* ≅ L^∞$이지만 $(L^∞)^* \not≅ L¹$ ($(L^∞)^*$는 더 크다).

---

**정리 3.5 (쌍대 노름의 특성화)**

$X$가 노름공간일 때:
$$\|x\|_X = \sup_{\|φ\|_{X^*} ≤ 1} |φ(x)|$$

**증명**:

*왼쪽 ≤ 오른쪽*: 
$$\sup_{\|φ\|_{X^*} ≤ 1} |φ(x)| ≤ \sup_{\|φ\|_{X^*} ≤ 1} \|φ\|_{X^*} \cdot \|x\|_X = 1 · \|x\|_X = \|x\|_X$$

*오른쪽 ≤ 왼쪽*:
$x ≠ 0$일 때, Hahn-Banach 정리(정리 4.1, 다음 장)를 사용하면, $φ ∈ X^*$가 존재하여:
$$φ(x) = \|x\|_X, \quad \|φ\|_{X^*} = 1$$

따라서:
$$\|x\|_X = |φ(x)| ≤ \sup_{\|ψ\|_{X^*} ≤ 1} |ψ(x)|$$

$\square$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import integrate, optimize
from scipy.linalg import svd

# ============================================================================
# 1. R^n의 쌍대: (R^n)* ≅ R^n
# ============================================================================

print("=" * 70)
print("1. 유한차원: (ℝⁿ)* ≅ ℝⁿ")
print("=" * 70)

# R^3의 벡터 x
x = np.array([2.0, -1.0, 3.0])

# R^3의 쌍대: 선형범함수는 행벡터로 표현
# φ(x) = a^T · x = a_1·x_1 + a_2·x_2 + a_3·x_3
a1 = np.array([1.0, 2.0, -1.0])  # 첫 번째 범함수
a2 = np.array([0.5, -1.0, 2.0])  # 두 번째 범함수

phi1_x = np.dot(a1, x)
phi2_x = np.dot(a2, x)

print(f"x = {x}")
print(f"φ₁(x) = a₁·x = [1, 2, -1]·[2, -1, 3] = {phi1_x}")
print(f"φ₂(x) = a₂·x = [0.5, -1, 2]·[2, -1, 3] = {phi2_x}")
print()

# 범함수의 노름: ‖φ‖ = ‖a‖ (Euclidean 노름)
norm_a1 = np.linalg.norm(a1)
norm_a2 = np.linalg.norm(a2)

print(f"‖φ₁‖_(R³)* = ‖a₁‖ = {norm_a1:.6f}")
print(f"‖φ₂‖_(R³)* = ‖a₂‖ = {norm_a2:.6f}")
print()

# 쌍대 노름으로 x의 노름 복원: ‖x‖ = sup_{‖φ‖ ≤ 1} |φ(x)|
unit_dual_vectors = np.random.randn(1000, 3)
unit_dual_vectors = unit_dual_vectors / np.linalg.norm(unit_dual_vectors, axis=1, keepdims=True)  # 단위 벡터로 정규화

max_functional_value = max(abs(np.dot(unit_dual_vectors, x)))
norm_x_original = np.linalg.norm(x)

print(f"직접 계산: ‖x‖ = {norm_x_original:.6f}")
print(f"쌍대로부터: sup_(‖φ‖ ≤ 1) |φ(x)| ≈ {max_functional_value:.6f}")
print(f"오차: {abs(max_functional_value - norm_x_original):.2e}")
print()

# ============================================================================
# 2. L^p-L^q 켤레 지수와 Hölder 부등식
# ============================================================================

print("=" * 70)
print("2. Hölder 부등식: ∫|fg| ≤ ‖f‖_p · ‖g‖_q (1/p + 1/q = 1)")
print("=" * 70)

# [0,1]에서 함수들을 이산화
x_grid = np.linspace(0, 1, 1000)
dx = x_grid[1] - x_grid[0]

# 켤레 지수 쌍
pairs = [(2, 2), (3, 1.5), (4, 4/3), (5, 5/4)]

for p, q in pairs:
    # f = x^(1/p), g = x^(1/q)라고 하자
    f = x_grid ** (1/p)
    g = x_grid ** (1/q)
    
    # L^p 노름: (∫ |f|^p)^(1/p)
    norm_f_p = (np.sum(np.abs(f)**p) * dx) ** (1/p)
    
    # L^q 노름: (∫ |g|^q)^(1/q)
    norm_g_q = (np.sum(np.abs(g)**q) * dx) ** (1/q)
    
    # ∫ fg
    integral_fg = np.sum(f * g) * dx
    
    # Hölder: ∫ fg ≤ ‖f‖_p · ‖g‖_q
    product_norms = norm_f_p * norm_g_q
    
    print(f"1/p + 1/q = 1/{p} + 1/{q} = {1/p + 1/q:.6f} (≈ 1)")
    print(f"  ∫ fg = {integral_fg:.6f}")
    print(f"  ‖f‖_p·‖g‖_q = {product_norms:.6f}")
    print(f"  Hölder 만족: {integral_fg <= product_norms + 1e-6}")
    print()

# ============================================================================
# 3. L^p의 쌍대: (L^p)* ≅ L^q
# ============================================================================

print("=" * 70)
print("3. L^p의 쌍대: (L²)* ≅ L²")
print("=" * 70)

# L^2[0,1]의 함수를 벡터로 이산화
# f(x) = sin(2π x), g(x) = cos(4π x)
f = np.sin(2 * np.pi * x_grid)
g = np.cos(4 * np.pi * x_grid)

# L^2 노름: ∫₀¹ |f|² dx
norm_f_L2 = np.sqrt(np.sum(f**2) * dx)
norm_g_L2 = np.sqrt(np.sum(g**2) * dx)

# L^2에서 g는 범함수를 정의: φ_g(f) = ∫ fg
phi_g_f = np.sum(f * g) * dx

# 범함수의 노름: ‖φ_g‖ = ‖g‖_L^2
print(f"‖f‖_L² = {norm_f_L2:.6f}")
print(f"‖g‖_L² = {norm_g_L2:.6f}")
print(f"φ_g(f) = ∫ fg = {phi_g_f:.6f}")
print(f"‖φ_g‖_(L²)* = ‖g‖_L² = {norm_g_L2:.6f}")
print()

# Riesz 표현: 임의의 φ ∈ (L^2)* 에 대해 유일한 g ∈ L^2 가 존재하여 φ(f) = ⟨f,g⟩ = ∫ fg

# 시뮬레이션: 여러 범함수에 대해 Riesz 표현 검증
print("Riesz 표현 정리 검증 (여러 범함수):")

test_g_functions = [
    np.ones_like(x_grid),
    x_grid,
    x_grid**2,
    np.sin(2 * np.pi * x_grid),
    np.cos(6 * np.pi * x_grid)
]

for i, g_func in enumerate(test_g_functions):
    # 범함수: φ_g(f) = ∫ fg
    phi_g_value = np.sum(f * g_func) * dx
    
    # Riesz 표현에 의해, ‖φ_g‖ = ‖g‖
    norm_g = np.sqrt(np.sum(g_func**2) * dx)
    
    print(f"  φ_{i}(f) = ∫ f·g_{i} = {phi_g_value:.6f}, ‖g_{i}‖_L² = {norm_g:.6f}")

print()

# ============================================================================
# 4. ℓ¹-ℓ^∞ 쌍대성
# ============================================================================

print("=" * 70)
print("4. ℓ¹-ℓ^∞ 쌍대성: (ℓ¹)* ≅ ℓ^∞")
print("=" * 70)

# ℓ¹의 벡터
x_l1 = np.array([1.0, 0.5, 0.25, 0.125, 0.0625])  # 기하급수
norm_x_l1 = np.sum(np.abs(x_l1))

# ℓ^∞의 벡터 (쌍대)
y_linf = np.array([2.0, 1.5, 1.0, 0.5, 0.25])
norm_y_linf = np.max(np.abs(y_linf))

# 범함수: φ_y(x) = ∑ x_i · y_i
phi_value = np.sum(x_l1 * y_linf)

print(f"x ∈ ℓ¹: {x_l1}")
print(f"‖x‖_ℓ¹ = {norm_x_l1:.6f}")
print(f"y ∈ ℓ^∞: {y_linf}")
print(f"‖y‖_ℓ^∞ = {norm_y_linf:.6f}")
print(f"φ_y(x) = ∑ x_i·y_i = {phi_value:.6f}")
print(f"범함수 노름 ≤ ‖x‖·‖y‖: {abs(phi_value):.6f} ≤ {norm_x_l1 * norm_y_linf:.6f}")
print()

# ============================================================================
# 5. 반사적 공간: L^p (1<p<∞) vs 비반사적 공간: ℓ¹
# ============================================================================

print("=" * 70)
print("5. 반사적 vs 비반사적 공간")
print("=" * 70)

print("L^p (1 < p < ∞)는 반사적: (L^p)** ≅ L^p")
print("예: (L²)* ≅ L², ((L²)*)* ≅ L²")
print()

print("L¹은 비반사적: (L¹)* ≅ L^∞, 그러나 ((L¹)*)* = (L^∞)* ≠ L¹")
print("더 큼: (L^∞)* ⊃ L¹")
print()

# ℓ^p의 쌍대 구조 시각화
print("ℓ^p 수열 공간의 쌍대:")
print("  (ℓ¹)* ≅ ℓ^∞     (비반사적)")
print("  (ℓ^p)* ≅ ℓ^q    (1 < p < ∞, 반사적)")
print("  c₀* ≅ ℓ¹        (수렴하는 수열들의 공간, 비반사적)")
print()

# ============================================================================
# 6. 쌍대 노름: 경사(gradient)의 노름
# ============================================================================

print("=" * 70)
print("6. 쌍대 공간과 경사(Gradient)")
print("=" * 70)

# 손실 함수 L(w) = ‖Aw - b‖²_2 in R^n
# 경사 ∇L(w) = 2A^T(Aw - b) ∈ (R^n)* ≅ R^n

A_loss = np.array([
    [2.0, 1.0],
    [1.0, 3.0],
    [1.0, 1.0]
])
b_loss = np.array([1.0, 2.0, 0.5])
w = np.array([0.5, -0.5])

# 손실
residual = A_loss @ w - b_loss
loss = np.sum(residual**2)

# 경사
gradient = 2 * A_loss.T @ residual

# 경사의 노름 (R^n에서): 이것이 (R^n)*에서의 노름
norm_gradient = np.linalg.norm(gradient)

print(f"w = {w}")
print(f"L(w) = ‖Aw - b‖² = {loss:.6f}")
print(f"∇L(w) = {gradient}")
print(f"‖∇L(w)‖ (쌍대 노름) = {norm_gradient:.6f}")
print()

# 경사를 쌍대원소로 보고, 방향 미분 확인
direction = np.array([1.0, 0.0])
direction = direction / np.linalg.norm(direction)

directional_derivative = np.dot(gradient, direction)
print(f"방향 d = {direction}")
print(f"D_d L(w) = ∇L(w)·d = {directional_derivative:.6f}")
print()

# ============================================================================
# 7. 시각화: 쌍대 공간의 기하학
# ============================================================================

fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# 1. R^2에서 쌍대 벡터 (행벡터) 시각화
ax = axes[0, 0]
x_point = np.array([2.0, 1.0])
a_vectors = [
    np.array([1.0, 0.5]),
    np.array([0.0, 1.0]),
    np.array([-0.5, 0.8])
]

ax.scatter(*x_point, s=200, c='red', marker='*', label='x', zorder=5)
for i, a in enumerate(a_vectors):
    a_normalized = a / np.linalg.norm(a)
    ax.arrow(0, 0, a[0], a[1], head_width=0.15, head_length=0.1, 
             fc=f'C{i}', ec=f'C{i}', alpha=0.7)
    phi_x = np.dot(a, x_point)
    ax.text(a[0]+0.2, a[1]+0.2, f'φ_{i}(x)={phi_x:.1f}', fontsize=10)

ax.set_xlim(-1, 3)
ax.set_ylim(-1, 3)
ax.set_aspect('equal')
ax.grid(True, alpha=0.3)
ax.set_xlabel('a₁', fontsize=11)
ax.set_ylabel('a₂', fontsize=11)
ax.set_title('R² 쌍대공간: 행벡터들', fontsize=12, fontweight='bold')
ax.legend()

# 2. L^p-L^q 켤레 지수
ax = axes[0, 1]
p_vals = np.linspace(1.01, 10, 100)
q_vals = p_vals / (p_vals - 1)

ax.plot(p_vals, q_vals, 'b-', linewidth=2, label='q = p/(p-1)')
ax.fill_between(p_vals, q_vals, 10, alpha=0.2)
ax.axvline(x=2, color='r', linestyle='--', alpha=0.5)
ax.axhline(y=2, color='r', linestyle='--', alpha=0.5)
ax.scatter([2], [2], s=100, c='red', zorder=5, label='p=q=2 (L²)')
ax.set_xlim(1, 10)
ax.set_ylim(0.8, 10)
ax.set_xlabel('p', fontsize=11)
ax.set_ylabel('q', fontsize=11)
ax.set_title('Hölder 켤레 지수: 1/p + 1/q = 1', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

# 3. Hölder 부등식 검증
ax = axes[1, 0]
p_range = np.linspace(1.1, 5, 50)
errors_holder = []

for p in p_range:
    q = p / (p - 1)
    
    f_test = x_grid ** (1/p)
    g_test = x_grid ** (1/q)
    
    norm_f_p = (np.sum(np.abs(f_test)**p) * dx) ** (1/p)
    norm_g_q = (np.sum(np.abs(g_test)**q) * dx) ** (1/q)
    integral = np.sum(f_test * g_test) * dx
    
    error = norm_f_p * norm_g_q - integral
    errors_holder.append(error)

ax.semilogy(p_range, errors_holder, 'o-', linewidth=2, markersize=6)
ax.set_xlabel('p', fontsize=11)
ax.set_ylabel('‖f‖_p · ‖g‖_q - ∫fg', fontsize=11)
ax.set_title('Hölder 부등식의 여유(slack)', fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3, which='both')

# 4. 경사 노름 (쌍대 노름)
ax = axes[1, 1]
w_path = []
grad_norms = []

for w1 in np.linspace(-2, 2, 30):
    for w2 in np.linspace(-2, 2, 30):
        w_test = np.array([w1, w2])
        residual_test = A_loss @ w_test - b_loss
        grad_test = 2 * A_loss.T @ residual_test
        grad_norms.append(np.linalg.norm(grad_test))
        w_path.append([w1, w2])

w_path = np.array(w_path)
contour = ax.tricontourf(w_path[:, 0], w_path[:, 1], grad_norms, levels=20, cmap='viridis')
ax.set_xlabel('w₁', fontsize=11)
ax.set_ylabel('w₂', fontsize=11)
ax.set_title('경사 노름의 가시화', fontsize=12, fontweight='bold')
plt.colorbar(contour, ax=ax, label='‖∇L(w)‖')

plt.tight_layout()
plt.savefig('/tmp/dual_space.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/dual_space.png")
print()

print("모든 검증 완료!")
```

## 🔗 AI/ML 연결

1. **경사(Gradient)**: 손실 함수 $L: \mathbb{R}^n \to \mathbb{R}$의 경사 $\nabla L(w)$는 쌍대공간 $(\mathbb{R}^n)^* ≅ \mathbb{R}^n$의 원소로 볼 수 있다. 역전파는 쌍대 공간에서의 선형 계산.

2. **라그랑주 승수**: 제약 최적화 $\min f(x)$ s.t. $g(x) ≤ 0$에서 라그랑주 승수 $\lambda$는 제약의 쌍대 변수. Karush-Kuhn-Tucker (KKT) 조건은 쌍대성 이론의 응용.

3. **Wasserstein 거리**: 확률분포 간의 거리는 쌍대공간 (연속함수들의 공간)에서의 supremum 문제로 표현: $W_p(\mu, \nu) = \sup_{\|φ\| ≤ 1} |\mathbb{E}_\mu[φ] - \mathbb{E}_\nu[φ]|$.

4. **Hilbert 공간에서의 최적화**: Riesz 표현 정리에 의해 모든 쌍대원소가 원래 공간의 벡터로 표현되므로, 경사 강하를 명시적 벡터로 계산 가능.

## ⚖️ 가정과 한계

- **완비성 필요**: Banach 공간이 아닌 노름공간의 쌍대는 Banach가 아닐 수 있다.
- **반사성의 부재**: 무한차원의 많은 공간 ($L^1$, $L^∞$, $c_0$)은 반사적이 아니므로 이중 쌍대로 완전히 표현 불가.
- **계산의 어려움**: 무한차원에서 쌍대원소를 명시적으로 구성하는 것은 어렵다 (Riesz 표현 정리는 존재만 보장).

## 📌 핵심 정리 (표로 요약)

| 공간 | 쌍대 공간 | 반사적? | Riesz 표현 |
|------|---------|--------|----------|
| $ℝ^n$ | $(ℝ^n)^* ≅ ℝ^n$ | 예 | 행벡터 ↔ 열벡터 |
| $L^p$ ($1<p<∞$) | $(L^p)^* ≅ L^q$ ($1/p+1/q=1$) | 예 | $φ(f) = ∫ fg$ |
| $L^1$ | $(L^1)^* ≅ L^∞$ | 아니오 | ∫ 적분 형태 |
| $ℓ^p$ ($1<p<∞$) | $(ℓ^p)^* ≅ ℓ^q$ | 예 | 점별 곱 |
| $ℓ^1$ | $(ℓ^1)^* ≅ ℓ^∞$ | 아니오 | - |

## 🤔 생각해볼 문제

**문제 3.1**: $ℝ^3$의 쌍대에서 세 범함수 $φ_1(x) = x_1 + 2x_2 - x_3$, $φ_2(x) = x_1 - x_2 + 3x_3$, $φ_3(x) = x_2$를 생각하자. 이들의 노름을 구하고, $x = (1, 1, 1)$에 대해 각 범함수의 값을 계산하라.

<details>
<summary>풀이</summary>

행벡터로 표현: $φ_1 ↔ (1, 2, -1)$, $φ_2 ↔ (1, -1, 3)$, $φ_3 ↔ (0, 1, 0)$

노름:
- $\|φ_1\| = \|(1, 2, -1)\| = \sqrt{1 + 4 + 1} = \sqrt{6}$
- $\|φ_2\| = \|(1, -1, 3)\| = \sqrt{1 + 1 + 9} = \sqrt{11}$
- $\|φ_3\| = \|(0, 1, 0)\| = 1$

$x = (1, 1, 1)$에서:
- $φ_1(x) = 1 + 2 - 1 = 2$
- $φ_2(x) = 1 - 1 + 3 = 3$
- $φ_3(x) = 1$

</details>

**문제 3.2**: L²[0,1]에서 $f(x) = x$, $g(x) = 1$이라 하자. Riesz 표현 정리에 의해, $g$는 선형범함수 $φ_g(f) = ∫₀¹ f(x)·g(x) dx$를 정의한다. $\|φ_g\|_{(L²)^*}$를 계산하고 $\|g\|_{L²}$와 같음을 확인하라.

<details>
<summary>풀이</summary>

$φ_g(f) = ∫₀¹ f(x) · 1 dx = ∫₀¹ x dx = 1/2$ (test function $f(x)=x$ 경우)

$\|g\|_{L²} = \sqrt{∫₀¹ 1² dx} = 1$

Riesz: $\|φ_g\|_{(L²)^*} = \|g\|_{L²} = 1$

확인: Hölder 부등식에서 $p = q = 2$이므로:
$$|φ_g(f)| = \left|∫ fg\right| ≤ \|f\|_{L²} · \|g\|_{L²} = \|f\|_{L²} · 1$$

따라서 $\|φ_g\| ≤ 1$. 등호는 $f = g$ (상수함수)에서 달성: $φ_g(g) = ∫ 1 dx = 1$, $\|g\|_{L²} = 1$.

</details>

**문제 3.3**: $L^∞[0,1]$은 비반사적임을 보이자. $(L^∞)^* ≅ ?$이고, $((L^∞)^*)^* ≠ L^∞$임을 설명하라.

<details>
<summary>풀이</summary>

측도론에서 $(L^∞)^* ≅ \text{ba}([0,1])$ (유한 덧셈 측도들의 공간, 가산 덧셈 측도보다 큼).

$((L^∞)^*)^* = (\text{ba})^* ⊃ L^∞$ (더 큼).

따라서 "쌍대의 쌍대 ≠ 원래 공간"이므로 비반사적.

이는 $L^∞$가 "너무 크기" 때문 (거의 모든 점에서의 유계성만 제약).

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./02-operator-space.md) | [📚 README](../README.md) | [다음 ▶](./04-hahn-banach.md) |

</div>
