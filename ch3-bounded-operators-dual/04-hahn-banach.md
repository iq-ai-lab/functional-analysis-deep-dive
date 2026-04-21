# 4. Hahn-Banach 정리 — 범함수의 노름보존 확장과 볼록 분리

## 🎯 핵심 질문
- 부분공간에서 정의된 유계 범함수를 전체 공간으로 확장할 수 있는가? 노름을 보존하면서?
- 두 볼록 집합이 "분리"된다는 것은 무엇인가?
- SVM과 최적화의 강쌍대성과 Hahn-Banach의 관계는?

## 🔍 왜 이 이론이 AI에서 중요한가

Hahn-Banach는 함수해석학의 **기초 정리**로, 다음을 보장한다:
- **분류 가능성**: 두 클래스의 특성(convex hull)이 서로소이면 초평면으로 분리 가능 → **SVM의 이론적 기초**
- **최대 마진**: SVM에서 최대 마진 초평면의 존재성 보증
- **강쌍대성**: 볼록 최적화에서 쌍대 문제와 원래 문제의 최적값이 같음 (Slater 조건 하에) → **라그랑주 승수 방법**
- **분포 분리**: GAN에서 판별자(discriminator)가 실제 vs 생성 분포를 구분하는 것의 기반

## 📐 수학적 선행 조건

- **쌍대공간 $X^*$**: 유계 선형범함수들의 공간
- **볼록 집합**: $C$가 볼록 ⟺ $x, y \in C, \lambda \in [0,1] \Rightarrow \lambda x + (1-\lambda)y \in C$
- **볼록껍질**: $\text{conv}(S) = \{\sum \lambda_i x_i : x_i \in S, \lambda_i \geq 0, \sum \lambda_i = 1\}$
- **선형함수형 초평면**: $φ(x) = c$ (단, $φ \in X^*$, $c \in \mathbb{K}$)

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서**:
$\mathbb{R}^n$에서 두 볼록 집합이 서로소이면, 초평면 $\{x : \mathbf{a}^T \mathbf{x} = c\}$ (단, $\mathbf{a} \in \mathbb{R}^n$)으로 분리 가능하다. 이는 선형대수의 기본 사실.

**무한차원에서**:
"초평면" 개념이 선형범함수 $φ: X \to \mathbb{K}$로 일반화된다. Hahn-Banach는 부분공간에서 정의된 범함수를 **노름을 보존하면서** 전체 공간으로 확장할 수 있음을 보장한다. 이것이 없으면 분리 정리를 증명할 수 없다.

## ✏️ 엄밀한 정의

**정의 4.1 (선형범함수의 확장)**

$Y \subset X$ (부분공간), $φ ∈ Y^*$ (유계 선형범함수)일 때, $Φ ∈ X^*$가 *φ의 확장(extension)*이면:
- $Φ|_Y = φ$ (즉, $Φ(y) = φ(y)$ ∀$y \in Y$)
- 가능하면 $\|Φ\|_{X^*} = \|φ\|_{Y^*}$ (노름 보존)

**정의 4.2 (초평면으로의 분리)**

$A, B \subset X$가 두 집합이고 $φ \in X^*, c \in \mathbb{K}$일 때, 초평면 $H = \{x : φ(x) = c\}$가:
- **약한 분리**: $φ(x) ≤ c$ ∀$x \in A$, $φ(x) ≥ c$ ∀$x \in B$
- **강한 분리**: $φ(x) < c < φ(y)$ ∀$x \in A, y \in B$ (또는 $≤$와 $≥$)

**정의 4.3 (볼록 위치)**

집합 $A, B$가 *볼록 위치*에 있으면, 초평면이 존재하여 $A$는 한쪽, $B$는 다른 쪽에 있다.

## 🔬 정리와 증명

**정리 4.1 (Hahn-Banach, 해석적 형태)**

$Y \subset X$ (부분공간), $X$가 노름공간, $φ \in Y^*$이라 하자. 그러면 $Φ \in X^*$가 존재하여:
$$Φ|_Y = φ \quad \text{and} \quad \|Φ\|_{X^*} = \|φ\|_{Y^*}$$

**증명** (실수체 $\mathbb{K} = \mathbb{R}$ 경우, Zorn 보조정리):

**Step 1**: Zorn 보조정리를 이용하여 "부분공간들의 확장 가능한 체인"의 극대원소를 찾는다.

부분공간 $Z$와 확장 $ψ: Z \to \mathbb{R}$의 쌍 $(Z, ψ)$를 고려하되, 조건:
- $Y \subseteq Z \subseteq X$
- $ψ|_Y = φ$
- $\|ψ\|_{Z^*} = \|φ\|_{Y^*}$

이 조건들을 만족하는 $(Z, ψ)$들의 집합 $\mathcal{P}$를 부분순서로 정렬: $(Z_1, ψ_1) ≤ (Z_2, ψ_2)$ ⟺ $Z_1 \subseteq Z_2$ and $ψ_2|_{Z_1} = ψ_1$.

**Step 2**: 임의의 chain $\{(Z_i, ψ_i)\}_{i \in I}$에 대해 상한이 존재:
$$Z_\infty := \bigcup_{i \in I} Z_i, \quad ψ_\infty(z) := ψ_i(z) \text{ if } z \in Z_i$$

$ψ_\infty$는 well-defined (다양한 $i$에 대해 일치), 선형, 같은 노름.

**Step 3**: Zorn 보조정리에 의해 극대원소 $(Z^*, ψ^*)$가 존재한다.

**Step 4**: $Z^* = X$임을 보이자 (귀류법).

$Z^* \neq X$라 하자. 그러면 $x_0 \in X \setminus Z^*$이 존재한다. $Z' := Z^* \oplus \mathbb{R}x_0 = \{z + \lambda x_0 : z \in Z^*, \lambda \in \mathbb{R}\}$로 놓자.

$Z'$에서 $ψ$를 확장하려면, $\lambda \neq 0$에 대해 $ψ'(z + \lambda x_0) = ψ^*(z) + \lambda c$로 정의하고, $c$를 선택하여 $\|ψ'\| = \|ψ^*\|$를 유지해야 한다.

이는 다음과 동치:
$$|ψ^*(z_1) + c| ≤ \|ψ^*\| \|z_1 + x_0\|, \quad |ψ^*(z_2) - c| ≤ \|ψ^*\| \|z_2 - x_0\|$$

모든 $z_1, z_2 \in Z^*$에 대해.

Hahn-Banach 보조정리(lemma)를 사용하면 그러한 $c$가 존재한다는 것을 보일 수 있다 (복소수 경우는 Bohnenblust-Sobczyk 정리).

따라서 $(Z', ψ')$은 $\mathcal{P}$의 원소이고 $(Z^*, ψ^*) < (Z', ψ')$인데, 이는 극대성에 모순.

그러므로 $Z^* = X$. $\square$

---

**정리 4.2 (Hahn-Banach, 기하적 형태: 분리 정리)**

$A, B \subset X$가 공집합이 아닌 볼록 집합이고, $A \cap B = \emptyset$라 하자. 또한 $A$는 열린집합이라고 하자. 그러면 초평면 $H = \{x : φ(x) = c\}$ (단, $φ \in X^*$, $c \in \mathbb{K}$, $φ ≠ 0$)이 존재하여:
$$φ(x) < c ≤ φ(y) \quad \forall x \in A, y \in B$$

또는 (약한 분리)
$$φ(x) ≤ c ≤ φ(y) \quad \forall x \in A, y \in B$$

**증명 스케치**:

Minkowski functional과 해석적 형태를 조합하여 증명한다. $A - B := \{a - b : a \in A, b \in B\}$를 정의하면, $A \cap B = \emptyset$로부터 $0 \notin A - B$. Minkowski functional을 구성하고, 부분공간에서 정의된 범함수를 Hahn-Banach로 확장하면, 분리 초평면을 얻는다.

(완전한 증명은 함수해석학 교과서 참고)

---

**정리 4.3 (SVM과의 연결: 최대 마진 초평면)**

두 유한 점 집합 $A = \{a_1, \ldots, a_m\}$, $B = \{b_1, \ldots, b_n\}$ (in $\mathbb{R}^d$)이 선형 분리 가능하면, 최대 마진 초평면:
$$\max_{w, b} \min_{i,j} \{d(a_i, H), d(b_j, H)\}$$

이 문제는 수학적으로 equivalent to:
$$\min_{w} \|w\|_2 \quad \text{s.t.} \quad \mathbf{w}^T a_i - b ≥ 1, \mathbf{w}^T b_j - b ≤ -1$$

Hahn-Banach의 분리 정리가 이러한 초평면의 존재를 보장한다.

---

**정리 4.4 (강쌍대성과 Slater 조건)**

볼록 최적화 문제:
$$\min_x f(x) \quad \text{s.t.} \quad g_i(x) ≤ 0 \quad (i=1,\ldots,m)$$

Slater 조건 (∃$x_0$: $g_i(x_0) < 0$ ∀$i$)이 성립하면, 라그랑주 함수의 쌍대 최적값이 원래 최적값과 같다:
$$p^* = d^*$$

**직관**: Slater 조건 + Hahn-Banach 분리 정리 ⟹ 제약이 "확실히 분리 가능" ⟹ 쌍대 변수의 존재 ⟹ 강쌍대성.

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import linprog
from scipy.spatial import ConvexHull

# ============================================================================
# 1. R^2에서 선형 분리: 두 점 집합
# ============================================================================

print("=" * 70)
print("1. R²에서 Hahn-Banach 분리: 두 점 집합을 초평면으로 분리")
print("=" * 70)

# 클래스 A: 빨간 점들 (around (1,1))
np.random.seed(42)
A = np.array([
    [1.0, 1.0],
    [1.5, 1.0],
    [1.0, 1.5],
    [0.8, 0.9],
    [1.3, 1.2]
])

# 클래스 B: 파란 점들 (around (3,3))
B = np.array([
    [3.0, 3.0],
    [3.5, 3.0],
    [3.0, 3.5],
    [2.8, 2.9],
    [3.3, 3.2]
])

# 볼록껍질 (convex hull)
hull_A = ConvexHull(A)
hull_B = ConvexHull(B)

print(f"점 집합 A: {len(A)}개 점")
print(f"점 집합 B: {len(B)}개 점")

# 선형 분리 여부 확인: SVM으로 해결
from sklearn.svm import SVC

X_train = np.vstack([A, B])
y_train = np.hstack([np.zeros(len(A)), np.ones(len(B))])

clf = SVC(kernel='linear')
clf.fit(X_train, y_train)

# 분리 초평면
w = clf.coef_[0]
b = clf.intercept_[0]

print(f"\nSVM 분리 초평면:")
print(f"w = {w}")
print(f"b = {b}")
print(f"방정식: {w[0]:.3f}·x₁ + {w[1]:.3f}·x₂ = {-b:.3f}")
print(f"정규화 방향: {w / np.linalg.norm(w)}")
print()

# 마진 계산
margin = 1.0 / np.linalg.norm(w)
print(f"마진 (초평면 사이 거리의 절반): {margin:.6f}")
print()

# 각 점이 올바르게 분류되는지 확인
for i, point in enumerate(A):
    score = np.dot(w, point) + b
    print(f"A[{i}]: φ(x) = {score:.3f} {'>' if score > 0 else '<'} 0 (예상: > 0)")

print()

for i, point in enumerate(B):
    score = np.dot(w, point) + b
    print(f"B[{i}]: φ(x) = {score:.3f} {'>' if score > 0 else '<'} 0 (예상: < 0)")

print()

# ============================================================================
# 2. 볼록 집합의 분리: 원과 직선의 분리
# ============================================================================

print("=" * 70)
print("2. 볼록 집합의 분리: Hahn-Banach 분리 정리")
print("=" * 70)

# 원판 A: center (1, 1), radius 0.5
theta = np.linspace(0, 2*np.pi, 20)
A_circle = np.array([[1 + 0.5*np.cos(t), 1 + 0.5*np.sin(t)] for t in theta])

# 직선 B: y = 3x - 2 (from x=0.5 to x=1.5)
x_vals = np.linspace(0.5, 1.5, 20)
B_line = np.array([[x, 3*x - 2] for x in x_vals])

# 분리 초평면 찾기: convex hull을 이용한 최단 거리
# A의 convex hull과 B의 convex hull 사이의 최단 거리를 찾는 방향이 w

# 간단히: A의 중심과 B의 중심을 잇는 직선을 사용
center_A = A_circle.mean(axis=0)
center_B = B_line.mean(axis=0)

# 분리 방향
separation_direction = center_B - center_A
separation_direction = separation_direction / np.linalg.norm(separation_direction)

print(f"A (원)의 중심: {center_A}")
print(f"B (직선)의 중심: {center_B}")
print(f"분리 방향: {separation_direction}")

# 각 점에서 분리 범함수의 값
phi_A = np.dot(A_circle, separation_direction)
phi_B = np.dot(B_line, separation_direction)

print(f"\nA의 φ 값 범위: [{phi_A.min():.3f}, {phi_A.max():.3f}]")
print(f"B의 φ 값 범위: [{phi_B.min():.3f}, {phi_B.max():.3f}]")
print(f"겹침 여부: {not (phi_A.max() < phi_B.min())}")
print()

# 분리 상수 c를 A와 B 사이에서 찾기
c = (phi_A.max() + phi_B.min()) / 2
print(f"분리 상수 c: {c:.6f}")
print(f"분리 성공: φ(x) ≤ {c:.6f} for x ∈ A, φ(y) ≥ {c:.6f} for y ∈ B")
print()

# ============================================================================
# 3. 강쌍대성: 2차 계획법
# ============================================================================

print("=" * 70)
print("3. 강쌍대성: 볼록 최적화 (Quadratic Programming)")
print("=" * 70)

# 최적화 문제:
# min 0.5·‖x‖² (+ 선형 항)
# s.t. -x₁ - x₂ ≤ -1  (즉, x₁ + x₂ ≥ 1)
#      x₁ ≤ 2
#      x₂ ≤ 2

# QP 표준 형태: min 0.5·x^T P x + q^T x + r
#              s.t. G x ≤ h, A x = b

P = np.eye(2)  # Hessian of 0.5·‖x‖²
q = np.array([0.0, 0.0])  # 선형 항 없음

# 부등식 제약
G = np.array([
    [-1, -1],  # -x₁ - x₂ ≤ -1
    [1, 0],    # x₁ ≤ 2
    [0, 1]     # x₂ ≤ 2
])
h = np.array([-1, 2, 2])

from scipy.optimize import minimize

def objective(x):
    return 0.5 * np.sum(x**2)

def constraint1(x):
    return x[0] + x[1] - 1  # ≥ 1

def constraint2(x):
    return 2 - x[0]  # ≤ 2

def constraint3(x):
    return 2 - x[1]  # ≤ 2

constraints = [
    {'type': 'ineq', 'fun': constraint1},
    {'type': 'ineq', 'fun': constraint2},
    {'type': 'ineq', 'fun': constraint3}
]

result = minimize(objective, x0=[1, 1], constraints=constraints, method='SLSQP')

print(f"원 문제 최적값: {result.fun:.6f}")
print(f"최적 x: {result.x}")
print()

# 쌍대 문제 설정
# 라그랑주: L(x, λ) = 0.5·‖x‖² - λ₁(x₁ + x₂ - 1) - λ₂(2 - x₁) - λ₃(2 - x₂)
#                     = 0.5·‖x‖² + λ₁(1 - x₁ - x₂) + λ₂(x₁ - 2) + λ₃(x₂ - 2)

# 쌍대 함수: g(λ) = min_x L(x, λ)

# x에 대해 최소화: ∇_x L = x - λ₁(1, 1) + λ₂(1, 0) + λ₃(0, 1) = 0
# x = λ₁(1, 1) - λ₂(1, 0) - λ₃(0, 1) = (λ₁ - λ₂, λ₁ - λ₃)

# 쌍대 최적화: max_{λ ≥ 0} g(λ)

def dual_function(lam):
    x = np.array([lam[0] - lam[1], lam[0] - lam[2]])
    L = 0.5 * np.sum(x**2) + lam[0] * (1 - x[0] - x[1]) + \
        lam[1] * (x[0] - 2) + lam[2] * (x[1] - 2)
    return L

# 쌍대 최적화 (최대화 = 최소화의 음수)
def neg_dual(lam):
    return -dual_function(lam)

result_dual = minimize(neg_dual, x0=[0.5, 0, 0], 
                       bounds=[(0, None), (0, None), (0, None)],
                       method='L-BFGS-B')

print(f"쌍대 문제 최적값: {-result_dual.fun:.6f}")
print(f"최적 λ: {result_dual.x}")
print()

# 강쌍대성 확인
print(f"원 최적값: {result.fun:.6f}")
print(f"쌍대 최적값: {-result_dual.fun:.6f}")
print(f"강쌍대성 성립: {abs(result.fun - (-result_dual.fun)) < 1e-4}")
print()

# ============================================================================
# 4. 시각화: Hahn-Banach 분리와 SVM
# ============================================================================

fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# 1. SVM 분리
ax = axes[0, 0]
ax.scatter(A[:, 0], A[:, 1], c='red', s=100, marker='o', label='클래스 A', zorder=5)
ax.scatter(B[:, 0], B[:, 1], c='blue', s=100, marker='o', label='클래스 B', zorder=5)

# 분리 초평면과 마진
x_range = np.linspace(-0.5, 4.5, 100)
y_separator = (-w[0] * x_range - b) / w[1]
y_margin1 = (-w[0] * x_range - b - 1) / w[1]
y_margin2 = (-w[0] * x_range - b + 1) / w[1]

ax.plot(x_range, y_separator, 'k-', linewidth=2, label='분리 초평면')
ax.plot(x_range, y_margin1, 'k--', linewidth=1, alpha=0.5)
ax.plot(x_range, y_margin2, 'k--', linewidth=1, alpha=0.5)
ax.fill_between(x_range, y_margin1, y_margin2, alpha=0.1, color='gray', label='마진')

ax.set_xlim(0, 4)
ax.set_ylim(0, 4)
ax.set_aspect('equal')
ax.set_xlabel('x₁', fontsize=11)
ax.set_ylabel('x₂', fontsize=11)
ax.set_title('SVM 분리: Hahn-Banach 응용', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

# 2. 원과 직선의 분리
ax = axes[0, 1]
ax.scatter(A_circle[:, 0], A_circle[:, 1], c='red', s=50, marker='o', label='원판 A', zorder=5)
ax.scatter(B_line[:, 0], B_line[:, 1], c='blue', s=50, marker='o', label='직선 B', zorder=5)

# 분리 초평면
x_range = np.linspace(-1, 3, 100)
# φ(x) = separation_direction · x = c
y_sep = (c - separation_direction[0] * x_range) / separation_direction[1]
ax.plot(x_range, y_sep, 'k-', linewidth=2, label=f'분리 초평면 (c={c:.2f})')

ax.set_xlim(-0.5, 2.5)
ax.set_ylim(-2, 4)
ax.set_aspect('equal')
ax.set_xlabel('x₁', fontsize=11)
ax.set_ylabel('x₂', fontsize=11)
ax.set_title('볼록 집합의 분리', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

# 3. 범함수 값의 분포
ax = axes[1, 0]
ax.hist(phi_A, bins=10, alpha=0.6, label='A의 φ 값', color='red', edgecolor='black')
ax.hist(phi_B, bins=10, alpha=0.6, label='B의 φ 값', color='blue', edgecolor='black')
ax.axvline(c, color='green', linestyle='--', linewidth=2, label=f'분리 상수 c={c:.3f}')
ax.set_xlabel('φ(x)', fontsize=11)
ax.set_ylabel('빈도', fontsize=11)
ax.set_title('범함수 φ(x) = separation_direction · x의 분포', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3, axis='y')

# 4. 강쌍대성: 원과 쌍대 문제 간 격차 (duality gap)
ax = axes[1, 1]
n_points = 50
lambda_range = np.linspace(0, 2, n_points)
dual_values = []

for lam in lambda_range:
    # 단순화: λ의 1차원으로 제약
    lam_full = np.array([lam, 0, 0])
    dual_values.append(dual_function(lam_full))

ax.plot(lambda_range, dual_values, 'b-', linewidth=2, label='쌍대 함수 g(λ)')
ax.axhline(result.fun, color='r', linestyle='--', linewidth=2, label=f'원 최적값 = {result.fun:.3f}')
ax.fill_between(lambda_range, result.fun, np.array(dual_values), 
                 where=(np.array(dual_values) <= result.fun + 0.1),
                 alpha=0.2, color='gray', label='쌍대 격차(Duality Gap)')

ax.set_xlabel('λ₁ (라그랑주 승수)', fontsize=11)
ax.set_ylabel('g(λ)', fontsize=11)
ax.set_title('강쌍대성: 원 문제와 쌍대 문제의 최적값 일치', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)
ax.set_ylim([0, 2])

plt.tight_layout()
plt.savefig('/tmp/hahn_banach.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/hahn_banach.png")
print()

print("모든 검증 완료!")
```

## 🔗 AI/ML 연결

1. **Support Vector Machine (SVM)**: 최대 마진 초평면의 존재성은 Hahn-Banach 분리 정리에서 비롯. 두 클래스의 convex hull이 서로소이면 분리 초평면이 존재한다.

2. **강쌍대성 (Strong Duality)**: 볼록 최적화 문제에서 Slater 조건 + Hahn-Banach ⟹ 쌍대 격차(duality gap) = 0 ⟹ 라그랑주 승수 방법 적용 가능.

3. **GAN 훈련**: 판별자는 실제 분포와 생성 분포를 "분리"하는 함수를 학습. Hahn-Banach 분리 정리는 이러한 분리 함수의 존재를 보장.

4. **제약 최적화**: KKT 조건의 라그랑주 승수는 Hahn-Banach와 강쌍대성으로부터 유도.

## ⚖️ 가정과 한계

- **비유계성**: Hahn-Banach는 유계 범함수에만 적용. 비유계 범함수는 확장 불가능.
- **노름 보존**: 서로 다른 노름을 가진 공간 간의 확장은 노름이 변할 수 있음.
- **계산 난해**: 실제로 확장을 구성하는 것은 매우 어려움 (Zorn 보조정리는 비구성적).

## 📌 핵심 정리 (표로 요약)

| 정리 | 가정 | 결론 |
|------|------|------|
| **Hahn-Banach (해석적)** | Y ⊂ X 부분공간, φ ∈ Y* | Φ ∈ X* 확장, ‖Φ‖ = ‖φ‖ |
| **분리 정리** | A, B 볼록, 공집합, A ∩ B = ∅, A 열림 | 초평면 φ(x) = c로 분리 |
| **SVM 최대 마진** | 선형 분리 가능 | 최대 마진 초평면 존재 |
| **강쌍대성** | 볼록 최적화 + Slater 조건 | 원 최적값 = 쌍대 최적값 |

## 🤔 생각해볼 문제

**문제 4.1**: 부분공간 Y = {(x, x) : x ∈ ℝ} ⊂ ℝ²이고, φ: Y → ℝ를 φ(x, x) = 3x로 정의하자. 이를 ℝ² 전체로 확장하는 선형범함수를 찾아라. (확장이 유일한가?)

<details>
<summary>풀이</summary>

φ ∈ Y*이고 ‖φ‖_Y* = 3 (∵ |φ(x,x)| = 3|x| = 3·‖(x,x)‖_∞)

Hahn-Banach에 의해 확장 Φ ∈ (ℝ²)*이 존재하여 ‖Φ‖ = 3.

ℝ²의 쌍대는 ℝ²이므로 (행벡터), Φ(x₁, x₂) = a₁x₁ + a₂x₂ 형태.

조건: Φ(x,x) = φ(x,x) = 3x
⟹ a₁x + a₂x = 3x ∀x
⟹ a₁ + a₂ = 3

또한 ‖Φ‖ = max{‖(a₁, a₂)‖} = √(a₁² + a₂²) = 3

a₁ + a₂ = 3과 a₁² + a₂² = 9를 풀면:
a₂ = 3 - a₁
a₁² + (3-a₁)² = 9
2a₁² - 6a₁ + 9 - 9 = 0
2a₁(a₁ - 3) = 0
⟹ a₁ = 0 또는 a₁ = 3

따라서 두 가지 확장: (a₁, a₂) = (0, 3) 또는 (3, 0).

**확장이 유일하지 않음** (Y가 원래 공간 ℝ²의 부분공간이지만 여공간이 있기 때문).

</details>

**문제 4.2**: SVM에서 최대 마진 초평면을 찾는 것이 Hahn-Banach와 어떻게 연결되는지 설명하라.

<details>
<summary>풀이</summary>

두 클래스 A, B가 선형 분리 가능하면, convex hull conv(A)와 conv(B)는 disjoint (볼록집합).

Hahn-Banach 분리 정리: 초평면 φ(x) = c로 conv(A)와 conv(B)를 분리 가능.

최대 마진은 이 분리 초평면에서 A와 B까지의 최소 거리를 최대화하는 문제.

초평면에 수직인 방향 φ/‖φ‖에서 거리는 (마진) = (c - max_{a∈A} φ(a)) / ‖φ‖ = (min_{b∈B} φ(b) - c) / ‖φ‖

이를 최대화 = ‖φ‖ 최소화하면서 분리 조건 유지 (SVM의 목적함수).

따라서 Hahn-Banach ⟹ 분리 초평면 존재 ⟹ 최대 마진 초평면 존재.

</details>

**문제 4.3**: Slater 조건 (제약 최적화에서 ∃x₀: g_i(x₀) < 0 ∀i)이 없으면 강쌍대성이 성립하지 않을 수 있음을 보이는 반례를 구성하자.

<details>
<summary>풀이</summary>

문제: min x s.t. x² ≤ 0

제약 g(x) = x² ≤ 0은 x = 0에서만 만족.

원 최적값: p* = 0 (x = 0)

라그랑주: L(x, λ) = x + λx² (λ ≥ 0)

쌍대 함수: g(λ) = inf_x (x + λx²)

λ > 0이면, ∂/∂x (x + λx²) = 1 + 2λx = 0 ⟹ x = -1/(2λ)

g(λ) = -1/(2λ) + λ/(4λ²) = -1/(2λ) + 1/(4λ) = -1/(4λ) → 0 as λ → ∞

λ = 0이면, g(0) = inf_x x = -∞

쌍대 최적값: d* = sup_λ≥0 g(λ) = 0

여기서 p* = d* = 0 (강쌍대성 성립)... 

더 강한 반례: 제약이 비선형이고 비볼록인 경우를 고려하면 쌍대 격차 발생.

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./03-dual-space.md) | [📚 README](../README.md) | [다음 ▶](./05-weak-convergence.md) |

</div>
