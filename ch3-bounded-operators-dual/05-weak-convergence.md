# 5. 약수렴과 약*수렴 — 무한차원의 다양한 수렴 위상

## 🎯 핵심 질문
- "약수렴"이란 무엇이고, 강수렴과 어떻게 다른가?
- 약*수렴은 약수렴과 무엇이 다른가?
- Banach-Alaoglu 정리는 왜 중요하고, 최적화에서 어떻게 사용되는가?

## 🔍 왜 이 이론이 AI에서 중요한가

**약수렴은 무한차원에서의 "손실 완화" 전략**이다:
- **변분법의 직접법**: 함수 수열이 강수렴하지 않아도 약수렴하면, 변분 문제의 최솟값이 달성됨 → 최적화 알고리즘의 수렴성 보증
- **Wasserstein 거리**: 확률분포의 약수렴은 생성모델의 수렴성을 정의. GAN에서 판별자의 손실을 약*위상으로 분석
- **반사적 공간에서의 유계 수열**: 항상 약수렴 부분수열을 가짐 → 신경망 가중치의 수렴성 보장
- **연산자의 약 수렴**: 신경망 레이어들 (연산자로 본) 수열이 약으로 수렴해도 성질 유지

## 📐 수학적 선행 조건

- **노름 수렴(강수렴)**: $\|x_n - x\| \to 0$
- **쌍대공간 $X^*$**: 유계 선형범함수들
- **점별 수렴**: 각 $x$에 대해 $(T_n(x))$ 수렴
- **컴팩트성과 위상**: 약위상, 약*위상

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서**:
$\mathbb{R}^n$에서는 "수렴"이 거의 하나: Euclidean 노름 수렴. 유계 수열은 항상 수렴 부분수열을 가진다(Bolzano-Weierstrass).

**무한차원에서**:
$\ell^2$ 같은 공간에서 표준 기저 $e_n = (0, \ldots, 0, 1, 0, \ldots)$ (n번째만 1)를 생각하면, $\|e_n\| = 1$이므로 유계이지만, 어떤 점으로도 강수렴하지 않는다. 하지만 약수렴은 한다!

**약수렴**: 모든 선형범함수 $φ \in X^*$에 대해 $φ(x_n) \to φ(x)$. 예: $\langle e_n, y \rangle \to 0$ (∀$y \in \ell^2$).

## ✏️ 엄밀한 정의

**정의 5.1 (약수렴, Weak Convergence)**

$X$가 노름공간이고 $(x_n) \subset X$, $x \in X$일 때, **$x_n$ 약수렴** to $x$ (기호: $x_n \rightharpoonup x$):
$$φ(x_n) \to φ(x) \quad \forall φ \in X^*$$

**정의 5.2 (강수렴, Strong Convergence)**

$$\|x_n - x\|_X \to 0$$

(정의 확인: 정리 1.1에서 강수렴 ⟺ 연속)

**정의 5.3 (약*수렴, Weak-star Convergence)**

$X$가 노름공간이고 $(φ_n) \subset X^*$, $φ \in X^*$일 때, **$φ_n$ 약*수렴** to $φ$:
$$φ_n(x) \to φ(x) \quad \forall x \in X$$

(주의: 이는 $X^{**}$의 부분집합 $J(X) \subset X^{**}$에서의 점별 수렴)

**정의 5.4 (Hilbert 공간에서의 약수렴)**

$H$가 Hilbert 공간이고 $x_n, x \in H$일 때:
$$x_n \rightharpoonup x \quad \Leftrightarrow \quad \langle x_n, y \rangle \to \langle x, y \rangle \quad \forall y \in H$$

(Riesz 표현 정리에 의해 $φ(x) = \langle x, y \rangle$ 형태)

**정의 5.5 (Banach-Alaoglu, 약*컴팩트)**

$X$가 노름공간일 때, 쌍대공간 $X^*$의 단위공 $B_{X^*} = \{\phi \in X^* : \|\phi\|_{X^*} \leq 1\}$은 약*위상에서 컴팩트이다.

## 🔬 정리와 증명

**정리 5.1 (강수렴 ⟹ 약수렴)**

$x_n \to x$ (강수렴) ⟹ $x_n \rightharpoonup x$ (약수렴)

**증명**:
$φ \in X^*$일 때, $φ$는 유계이므로:
$$|φ(x_n) - φ(x)| = |φ(x_n - x)| \leq \|φ\|_{X^*} \cdot \|x_n - x\|_X \to 0$$

따라서 $φ(x_n) \to φ(x)$. $\square$

---

**정리 5.2 (역은 일반적으로 거짓: 반례)**

$\ell^2$의 표준 기저: $e_n = (0, \ldots, 0, 1, 0, \ldots)$ (n번째만 1)

- $\|e_n\|_{\ell^2} = 1$ (유계)
- 어떤 $x \in \ell^2$로도 강수렴 안 함 ($\|e_m - e_n\| = \sqrt{2}$ for $m \neq n$)
- 하지만 $e_n \rightharpoonup 0$ (약수렴!)

**증명**:
$φ \in (\ell^2)^* \cong \ell^2$이면 $φ(y) = \langle e_n, y \rangle$는 $e_n$과 $y$의 내적.

$y = (y_1, y_2, \ldots)$일 때, $\langle e_n, y \rangle = y_n \to 0$ as $n \to \infty$ (∵ $(y_n)$은 $\ell^2$이므로 $\sum |y_n|^2 < \infty$ ⟹ $y_n \to 0$).

따라서 $e_n \rightharpoonup 0$. $\square$

---

**정리 5.3 (Hilbert 공간: 약수렴의 특성화)**

$H$가 Hilbert 공간이고 $x_n, x \in H$일 때:
$$x_n \rightharpoonup x \quad \text{and} \quad \|x_n\| \to \|x\| \quad \Rightarrow \quad x_n \to x \text{ (강수렴)}$$

**증명**:
$$\|x_n - x\|^2 = \langle x_n - x, x_n - x \rangle = \|x_n\|^2 - 2\langle x_n, x \rangle + \|x\|^2$$

약수렴 가정: $\langle x_n, x \rangle \to \langle x, x \rangle = \|x\|^2$

노름 가정: $\|x_n\|^2 \to \|x\|^2$

따라서:
$$\|x_n - x\|^2 \to \|x\|^2 - 2\|x\|^2 + \|x\|^2 = 0$$

즉, $x_n \to x$ (강수렴). $\square$

---

**정리 5.4 (Banach-Alaoglu Compactness)**

$X$가 노름공간일 때, 단위공 $B_{X^*} = \{φ \in X^* : \|φ\| \leq 1\}$은 약*위상에서 컴팩트이다.

**증명 스케치** (Tychonoff 정리 이용):

약*위상을 생각하자: $φ_n \to^* φ$ ⟺ $φ_n(x) \to φ(x)$ ∀$x \in X$.

각 $x \in X$에 대해, $φ(x) \in \mathbb{K}$는 $|φ(x)| \leq \|φ\| \cdot \|x\| \leq \|x\|$로 제한된다.

$B_{X^*}$의 점 $φ$는 함수족 $\{φ(x) : x \in X\}$로 표현되고, 각 좌표는 컴팩트 집합 $\{c \in \mathbb{K} : |c| \leq \|x\|\}$에 속한다.

Tychonoff의 무한 곱 컴팩트 정리: 컴팩트 집합들의 곱은 product topology에서 컴팩트 ⟹ $B_{X^*}$는 약*위상에서 컴팩트. $\square$

---

**정리 5.5 (반사적 공간에서의 유계 수열)**

$X$가 반사적 Banach 공간(예: $L^p$, $1 < p < \infty$)이면, 임의의 유계 수열 $(x_n)$은 약수렴 부분수열을 가진다.

**증명 개요**:

$(x_n)$이 유계: ∃$M > 0$ s.t. $\|x_n\| \leq M$ ∀$n$.

$B_X = \{x : \|x\| \leq M\} \subset X$라 하자.

반사적이므로 $X = X^{**}$ (동형). $B_X$는 약위상에서 컴팩트 (Banach-Alaoglu를 dual에 적용).

따라서 $(x_n)$은 약수렴 부분수열을 가진다. $\square$

---

**정리 5.6 (약*수렴의 기하학)**

$φ_n \to^* φ$ (약*수렴) ⟺ $\{φ_n\}$의 그래프가 특정 의미에서 수렴

다시 말해, "모든 점에서의 값"이 수렴:
$$\lim_{n \to \infty} φ_n(x) = φ(x) \quad \forall x \in X$$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import quad
from scipy.optimize import minimize

# ============================================================================
# 1. ℓ² 표준 기저의 약수렴: strong vs weak
# ============================================================================

print("=" * 70)
print("1. ℓ²의 표준 기저: 강수렴 안 함, 약수렴 함")
print("=" * 70)

# 표준 기저 e_n = (0, ..., 0, 1, 0, ...) (n번째만 1)
# 이산화된 ℓ²의 첫 1000개 차원으로 근사
dim = 1000
n_basis_vectors = 20

basis_vectors = []
for n in range(1, n_basis_vectors + 1):
    e_n = np.zeros(dim)
    e_n[n - 1] = 1.0
    basis_vectors.append(e_n)

# 강수렴: ‖e_m - e_n‖₂ = √2 for m ≠ n
print("강수렴 분석:")
distances_strong = []
for i in range(len(basis_vectors)):
    for j in range(i + 1, len(basis_vectors)):
        dist = np.linalg.norm(basis_vectors[i] - basis_vectors[j])
        distances_strong.append(dist)

print(f"  ‖e_m - e_n‖ for m ≠ n: {distances_strong[0]:.6f} (항상 √2 = {np.sqrt(2):.6f})")
print(f"  강수렴: 아니오 (거리가 0으로 가지 않음)")
print()

# 약수렴: ⟨e_n, y⟩ → 0 as n → ∞ (모든 y ∈ ℓ²에 대해)
print("약수렴 분석:")

# 테스트 벡터 y = (1, 1/2, 1/4, 1/8, ...)  (기하급수, ℓ² ∈)
y = np.array([1.0 / (2**k) for k in range(dim)])
y_norm = np.linalg.norm(y)

print(f"테스트 벡터 y: 기하급수 (y_k = 1/2^k)")
print(f"‖y‖₂ = {y_norm:.6f}")
print()

# 각 기저벡터와의 내적
print("⟨e_n, y⟩ 값들:")
inner_products = []
for n, e_n in enumerate(basis_vectors):
    inner_prod = np.dot(e_n, y)
    inner_products.append(inner_prod)
    print(f"  ⟨e_{n+1}, y⟩ = {inner_prod:.6f}")

print(f"\n⟨e_n, y⟩ → 0 as n → ∞: {inner_products[-1] < 1e-3}")
print("따라서 e_n ⇀ 0 (약수렴)")
print()

# 시각화: 여러 테스트 벡터에 대해
test_vectors = [
    (np.array([1.0 / (2**k) for k in range(dim)]), "기하급수"),
    (np.array([1.0 / np.sqrt(k+1) for k in range(dim)]), "1/√k"),
    (np.ones(dim) / np.sqrt(dim), "상수 벡터")
]

fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# 강수렴: 거리
ax = axes[0]
ax.plot(range(1, len(distances_strong) + 1), distances_strong, 'o-', 
        linewidth=2, markersize=6, color='red', label='‖e_m - e_n‖')
ax.axhline(np.sqrt(2), color='red', linestyle='--', linewidth=1, alpha=0.5)
ax.set_xlabel('쌍의 번호', fontsize=11)
ax.set_ylabel('거리', fontsize=11)
ax.set_title('강수렴: 기저벡터 간 거리 (수렴 안 함)', fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3)
ax.set_ylim([1.3, 1.5])

# 약수렴: 내적
ax = axes[1]
colors = ['red', 'blue', 'green']
for (test_vec, label), color in zip(test_vectors, colors):
    inner_prods = [np.dot(e_n, test_vec) for e_n in basis_vectors]
    ax.plot(range(1, len(inner_prods) + 1), inner_prods, 'o-',
            linewidth=2, markersize=6, color=color, label=f'⟨e_n, {label}⟩', alpha=0.7)

ax.axhline(0, color='black', linestyle='-', linewidth=0.5)
ax.set_xlabel('n (기저 인덱스)', fontsize=11)
ax.set_ylabel('⟨e_n, y⟩', fontsize=11)
ax.set_title('약수렴: 내적이 0으로 수렴', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/weak_convergence_basis.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/weak_convergence_basis.png")
print()

# ============================================================================
# 2. L² 함수열의 약수렴
# ============================================================================

print("=" * 70)
print("2. L²[0,1] 함수열의 약수렴")
print("=" * 70)

# 그리드 생성
x_grid = np.linspace(0, 1, 1000)
dx = x_grid[1] - x_grid[0]

# 진동하는 함수열: f_n(x) = sin(2πnx)
def f_n(x, n):
    return np.sin(2 * np.pi * n * x)

# 극한 함수: f(x) = 0
# ⟨f_n, g⟩ = ∫₀¹ sin(2πnx) g(x) dx → 0 for most g

# 테스트 함수들
test_funcs = [
    (lambda x: np.ones_like(x), "상수 1"),
    (lambda x: x, "선형 x"),
    (lambda x: np.sin(x), "sin(x)"),
    (lambda x: np.exp(-x), "exp(-x)")
]

print("f_n(x) = sin(2πnx)의 약수렴 검증:")
print()

inner_prods_oscillating = []

for test_func, label in test_funcs:
    g = test_func(x_grid)
    g_l2_norm = np.sqrt(np.sum(g**2) * dx)
    
    print(f"테스트 함수: {label} (‖g‖₂ = {g_l2_norm:.3f})")
    
    # 각 n에 대해 ⟨f_n, g⟩ 계산
    inner_vals = []
    for n in range(1, 11):
        f = f_n(x_grid, n)
        inner = np.sum(f * g) * dx
        inner_vals.append(inner)
    
    print(f"  ⟨f_n, {label}⟩: {inner_vals}")
    print(f"  수렴?: {abs(inner_vals[-1]) < abs(inner_vals[0])}")
    print()
    
    inner_prods_oscillating.append(inner_vals)

# ============================================================================
# 3. Hilbert 공간: 약수렴 + 노름 수렴 = 강수렴
# ============================================================================

print("=" * 70)
print("3. Hilbert 공간: x_n ⇀ x + ‖x_n‖→‖x‖ ⟹ 강수렴")
print("=" * 70)

# ℓ²에서 생성한 점열
# x_n = (1, 1/√n, 1/√n, ..., 1/√n, 0, 0, ...) (처음 n개)
dim_test = 100
x_target = np.zeros(dim_test)
x_target[0] = 1.0

sequence_weak = []
sequence_strong = []
norms_strong = []

for n in range(1, dim_test // 2):
    x_n = np.zeros(dim_test)
    x_n[0] = 1.0
    x_n[1:n+1] = 1.0 / np.sqrt(n)
    
    sequence_weak.append(x_n)
    norm_x_n = np.linalg.norm(x_n)
    norms_strong.append(norm_x_n)
    
    # 강수렴 거리
    dist_to_target = np.linalg.norm(x_n - x_target)
    sequence_strong.append(dist_to_target)

print("수열: x_n = (1, 1/√n, 1/√n, ..., 1/√n, 0, ...)")
print(f"극한: x = (1, 0, 0, ...)")
print()

# 약수렴 확인: ⟨x_n, y⟩ → ⟨x, y⟩
y_test = np.random.randn(dim_test)
y_test = y_test / np.linalg.norm(y_test)

inner_products_n = [np.dot(x_n, y_test) for x_n in sequence_weak]
inner_target = np.dot(x_target, y_test)

print("약수렴 확인 (random y):")
print(f"⟨x_n, y⟩ → {inner_target:.6f}: {inner_products_n[-5:]}")
print()

# 노름 수렴 확인
norm_target = np.linalg.norm(x_target)
print("노름 수렴 확인:")
print(f"‖x_target‖ = {norm_target:.6f}")
print(f"‖x_n‖ 마지막 5개: {norms_strong[-5:]}")
print(f"수렴?: {abs(norms_strong[-1] - norm_target) < 0.01}")
print()

# 강수렴
print("강수렴 거리:")
print(f"‖x_n - x‖ 마지막 5개: {sequence_strong[-5:]}")
print(f"강수렴?: {sequence_strong[-1] < 0.1}")
print()

# 시각화
fig, axes = plt.subplots(1, 3, figsize=(14, 4))

# 1. 약수렴 (여러 y에 대해)
ax = axes[0]
for _ in range(5):
    y = np.random.randn(dim_test)
    y = y / np.linalg.norm(y)
    inner = [np.dot(x_n, y) for x_n in sequence_weak]
    ax.plot(range(len(inner)), inner, '-', alpha=0.5)

ax.set_xlabel('n', fontsize=11)
ax.set_ylabel('⟨x_n, y⟩', fontsize=11)
ax.set_title('약수렴: 여러 y에 대해 ⟨x_n, y⟩ 수렴', fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3)

# 2. 노름 수렴
ax = axes[1]
ax.plot(range(len(norms_strong)), norms_strong, 'o-', linewidth=2, markersize=5, label='‖x_n‖')
ax.axhline(norm_target, color='r', linestyle='--', linewidth=2, label=f'‖x‖ = {norm_target:.3f}')
ax.set_xlabel('n', fontsize=11)
ax.set_ylabel('노름', fontsize=11)
ax.set_title('노름 수렴: ‖x_n‖ → ‖x‖', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

# 3. 강수렴
ax = axes[2]
ax.semilogy(range(len(sequence_strong)), sequence_strong, 'o-', linewidth=2, markersize=5, label='‖x_n - x‖')
ax.set_xlabel('n', fontsize=11)
ax.set_ylabel('거리 (log scale)', fontsize=11)
ax.set_title('강수렴: 약수렴 + 노름 수렴 ⟹ 강수렴', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('/tmp/weak_convergence_hilbert.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/weak_convergence_hilbert.png")
print()

# ============================================================================
# 4. Banach-Alaoglu: 약*컴팩트 단위공
# ============================================================================

print("=" * 70)
print("4. Banach-Alaoglu: 쌍대공간의 단위공은 약*위상에서 컴팩트")
print("=" * 70)

# ℝ^n의 경우 (유한차원은 자동으로 컴팩트)
# 하지만 무한차원을 이산화하여 시뮬레이션

# (ℓ²)* ≅ ℓ²의 단위공에서 약*수렴 부분수열 구성
dim_alaoglu = 50

# 무작위 범함수들: φ_n ∈ B_{(ℓ²)*}
functionals = []
np.random.seed(42)

for _ in range(100):
    # 단위공의 무작위 원소 (ℓ²에서)
    phi = np.random.randn(dim_alaoglu)
    phi = phi / np.linalg.norm(phi)
    functionals.append(phi)

# 이들의 값들을 여러 점에서 측정
test_points = [
    np.random.randn(dim_alaoglu) for _ in range(5)
]

print(f"생성된 범함수: {len(functionals)}개")
print(f"테스트 점: {len(test_points)}개")
print()

# 각 테스트 점에서의 범함수 값들이 컴팩트 집합을 이룸을 확인
for i, x in enumerate(test_points):
    values = [np.dot(phi, x) for phi in functionals]
    values.sort()
    print(f"테스트 점 {i}: φ(x) ∈ [{values[0]:.3f}, {values[-1]:.3f}]")

print()
print("Banach-Alaoglu: 이 범함수들의 극한점(cluster point)이 약*위상에서 존재")
print()

# ============================================================================
# 5. 변분 최적화: 약수렴의 응용
# ============================================================================

print("=" * 70)
print("5. 변분법의 직접법: 약수렴의 응용")
print("=" * 70)

# 함수 공간에서 에너지 최소화: ∫ (f'(x))² + f(x)² dx
# subject to: f(0) = 0, f(1) = 1

# 이산화: [0,1]을 격자로
n_grid = 100
x_nodes = np.linspace(0, 1, n_grid)
h = 1.0 / (n_grid - 1)

# f를 f의 값 f_i = f(x_i)로 표현
# 수치 미분: f'(x_i) ≈ (f_{i+1} - f_i) / h

def energy_functional(f, x_nodes):
    """에너지 함수: ∫ (f')² + f² dx"""
    h = x_nodes[1] - x_nodes[0]
    
    # 미분 항
    f_prime = np.diff(f) / h
    deriv_energy = np.sum(f_prime**2) * h
    
    # f² 항
    f_energy = np.sum(f**2) * h
    
    return deriv_energy + f_energy

def constraint_bc(f):
    """경계 조건: f(0) = 0, f(1) = 1"""
    return f[0]**2 + (f[-1] - 1)**2

# 최적화
x0 = np.linspace(0, 1, n_grid)

def objective(f):
    return energy_functional(f, x_nodes) + 1000 * constraint_bc(f)

result_variatonal = minimize(objective, x0, method='BFGS')
f_optimal = result_variatonal.x

print(f"최적 에너지: {energy_functional(f_optimal, x_nodes):.6f}")
print(f"경계 조건: f(0)={f_optimal[0]:.6f}, f(1)={f_optimal[-1]:.6f}")
print()

# 약수렴의 의미: 함수 수열이 강수렴하지 않아도 약수렴하면
# 극한 함수가 에너지 함수를 최소화

# 시각화: 최적 함수
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

ax = axes[0]
ax.plot(x_nodes, f_optimal, 'b-', linewidth=2, label='최적 함수')
ax.plot(x_nodes, x0, 'r--', linewidth=1, alpha=0.5, label='초기 추정')
ax.scatter([0, 1], [0, 1], s=100, c='red', zorder=5, label='경계 조건')
ax.set_xlabel('x', fontsize=11)
ax.set_ylabel('f(x)', fontsize=11)
ax.set_title('변분 문제의 최적해', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

# 에너지 항들
ax = axes[1]
f_nodes_opt = f_optimal
f_prime = np.diff(f_nodes_opt) / h
deriv_energy_opt = np.sum(f_prime**2) * h
f_energy_opt = np.sum(f_nodes_opt**2) * h

energies = [deriv_energy_opt, f_energy_opt]
labels = ["∫ (f')² dx", "∫ f² dx"]
colors = ['blue', 'orange']

bars = ax.bar(labels, energies, color=colors, alpha=0.7, edgecolor='black')
for bar, energy in zip(bars, energies):
    height = bar.get_height()
    ax.text(bar.get_x() + bar.get_width()/2., height,
            f'{energy:.4f}', ha='center', va='bottom', fontsize=11)

ax.set_ylabel('에너지', fontsize=11)
ax.set_title('최적 함수의 에너지 분해', fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3, axis='y')

plt.tight_layout()
plt.savefig('/tmp/weak_convergence_variational.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/weak_convergence_variational.png")
print()

print("모든 검증 완료!")
```

## 🔗 AI/ML 연결

1. **변분 오토인코더 (VAE)**: 잠재 분포의 수렴성은 약수렴으로 분석. KL 발산과 재구성 손실 간 트레이드오프는 약위상에서의 최적화.

2. **Wasserstein GAN**: 판별자는 Wasserstein 거리를 계산하며, 이는 쌍대공간에서의 supremum 문제. 약*수렴으로 분포 수렴 정의.

3. **최적화의 수렴성**: 반사적 공간(대부분의 $L^p$)에서 유계 최적화 수열은 약수렴 부분수열을 가짐. 따라서 변분 문제의 최솟값 달성 보장.

4. **신경망 가중치**: 깊은 신경망의 가중치 행렬들 (연산자로 본) 수열이 약수렴해도 합성 연산자는 의미 있는 극한을 가진다.

5. **분포 학습**: 생성모델이 학습하는 분포들의 수렴은 약*위상(Wasserstein 거리)으로 정의되는 것이 더 현실적.

## ⚖️ 가정과 한계

- **반사성 필요**: 비반사적 공간($L^1$, $L^\infty$, $c_0$)에서는 약수렴 부분수열이 항상 존재하지 않음.
- **컴팩트성 손실**: 무한차원에서 단위공은 약*위상에서만 컴팩트. 강위상에서는 아님.
- **계산 어려움**: 약수렴 판정은 "모든 범함수에 대해 확인"이 필요해 직접 계산 어려움.

## 📌 핵심 정리 (표로 요약)

| 수렴 방식 | 정의 | 강점 | 약점 |
|---------|------|------|------|
| **강수렴** | $\|x_n - x\| \to 0$ | 직관적, 계산 용이 | 제약 많음 |
| **약수렴** | $φ(x_n) \to φ(x)$ ∀φ ∈ X* | 약위상에서 컴팩트 | 확인 어려움 |
| **약*수렴** | $φ_n(x) \to φ(x)$ ∀x ∈ X | 쌍대 공간에서 컴팩트 | Hilbert에서 불필요 |

## 🤔 생각해볼 문제

**문제 5.1**: $\ell^2$에서 수열 $x_n = \frac{1}{n} e_1 + e_n$ (단, $e_i$는 표준 기저)을 생각하자. 이것이 약수렴하는가? 강수렴하는가?

<details>
<summary>풀이</summary>

**약수렴**:
$φ ∈ (\ell^2)^* ≅ ℓ^2$이면 $φ(x) = \sum_{i=1}^∞ a_i x_i$ (단, $(a_i) ∈ ℓ^2$).

$x_n = \frac{1}{n} e_1 + e_n$이므로:
$$φ(x_n) = \frac{a_1}{n} + a_n \to 0 + 0 = 0$$

(∵ $(a_n) ∈ ℓ^2$ ⟹ $a_n \to 0$, $\frac{a_1}{n} \to 0$)

따라서 $x_n \rightharpoonup 0$.

**강수렴**:
$\|x_n\|^2 = \frac{1}{n^2} + 1 \to 1 \neq 0$

따라서 강수렴하지 않음.

</details>

**문제 5.2**: Hilbert 공간 $H$에서 $x_n \rightharpoonup x$이고 $\|x_n\| \to \|x\|$이면 $x_n \to x$ (강수렴)임을 보이라.

<details>
<summary>풀이</summary>

내적의 극한:
$$\langle x_n - x, x_n - x \rangle = \|x_n\|^2 - 2\langle x_n, x \rangle + \|x\|^2$$

약수렴: $\langle x_n, x \rangle \to \langle x, x \rangle = \|x\|^2$

노름 수렴: $\|x_n\|^2 \to \|x\|^2$

따라서:
$$\|x_n - x\|^2 \to \|x\|^2 - 2\|x\|^2 + \|x\|^2 = 0$$

즉, $x_n \to x$.

</details>

**문제 5.3**: 반사적 공간이 아닌 $L^1[0,1]$에서, 유계 수열이 약수렴 부분수열을 갖지 않는 반례를 구성하라.

<details>
<summary>풀이</summary>

$f_n(x) = n \cdot \mathbf{1}_{[0, 1/n]}(x)$ (구간 $[0, 1/n]$의 지시함수에 $n$ 곱하기)

$$\|f_n\|_{L^1} = \int_0^{1/n} n dx = n \cdot \frac{1}{n} = 1$$

유계: $\|f_n\| = 1$.

약극한 후보: $φ ∈ (L^1)^* ≅ L^∞$에 대해
$$φ(f_n) = \int f_n \cdot g dx = \int_0^{1/n} n·g(x) dx$$

$g ∈ L^∞$이고 연속점에서 $g(0)$라 하면, squeeze 정리:
$$φ(f_n) \to n \cdot g(0) \cdot 0 = 0$$

하지만 $φ = \delta_0$ (점에서의 값)을 $L^∞$에서 표현할 수 없음 (Dirac은 $L^∞$의 원소 아님).

따라서 부분수열이 약수렴하지 않음. (비반사성의 결과)

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./04-hahn-banach.md) | [📚 README](../README.md) | |

</div>
