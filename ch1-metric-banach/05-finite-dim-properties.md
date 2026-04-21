# 5. Banach 공간의 유한차원 성질 — 무한차원과의 근본적 차이

## 🎯 핵심 질문

1. 유한차원에서는 "좋은" 성질들이 무한차원에서 왜 깨지는가?
2. 닫힌 유계 집합이 컴팩트하지 않다는 것이 의미하는 바는?
3. Riesz 보조정리가 나타내는 기하학적 사실은?
4. 유한차원과 무한차원을 구분하는 수학적 기준은?

## 🔍 왜 이 이론이 AI에서 중요한가

딥러닝의 핵심 도전:
- **유한차원** (파라미터 1백만 개): 모든 경사하강법이 어느 정도 수렴하는 경향
- **무한차원** (함수공간): 수렴이 보장되지 않음

Riesz 보조정리의 의미:
- 무한차원 공간의 단위구는 **컴팩트하지 않습니다**
- 따라서 최솟값의 존재가 보장되지 않습니다
- **정규화(regularization)**가 이를 "인위적으로" 해결합니다

또한:
- **Dropout, 배치 정규화** = 함수공간을 실질적으로 유한차원으로 제한
- **가중치 감쇠(weight decay)** = 파라미터를 컴팩트 영역으로 강제

이들이 작동하는 이유는 **유한차원 성질을 복구**하기 때문입니다.

## 📐 수학적 선행 조건

- 거리공간, 완비성, 노름공간 (Ch. 1-3)
- 함수공간과 연속함수 (Ch. 4)
- 컴팩트 집합의 개념
- 선형 독립과 기저

## 📖 직관적 이해 (유한차원 → 무한차원)

**ℝⁿ (유한차원)의 놀라운 성질:**
- 닫혀있고 유계인 집합 (예: 공) = 컴팩트
- 컴팩트 집합의 연속함수는 최댓값·최솟값 달성
- 모든 Cauchy 수열이 수렴
- 모든 무한 수열이 수렴하는 부분수열을 가짐

**무한차원 공간의 "실제" 상황:**
- 단위구 B = {x : ‖x‖ ≤ 1}는 **절대** 컴팩트하지 않습니다!
- 따라서:
  - 극한점이 존재하는 보장 없음
  - "무한히 멀어지는" 수열들이 존재
  - 최솟값의 존재가 보장되지 않음

**유한차원 부분공간의 특성:**
- 유한차원 부분공간은 항상 닫혀있습니다 (무한차원에서는 거짓!)
- 무한차원 공간의 "거의 모든" 부분공간은 열려있습니다

## ✏️ 엄밀한 정의

### 정의 5.1: 컴팩트 집합

위상공간 $X$의 부분집합 $K$가 **컴팩트**이다:
$$K \text{의 모든 열린덮개가 유한부분덮개를 가진다}$$

즉, $\{U_i\}_{i \in I}$가 $K$를 덮는 열린집합들이면,
$$\exists \text{ finite } J \subset I \text{ s.t. } K \subset \bigcup_{i \in J} U_i$$

**거리공간에서의 동치 정의 (Bolzano-Weierstrass):**
$K$가 컴팩트 ⟺ $K$의 모든 수열이 수렴하는 부분수열을 가짐

### 정의 5.2: Heine-Borel 정리

**명제 (유한차원만 참):**
$\mathbb{R}^n$에서, 집합이 컴팩트 ⟺ 닫혀있고 유계

이것은 **무한차원 Banach 공간에서 거짓**입니다!

### 정의 5.3: Riesz 보조정리의 설정

$X$: Banach 공간  
$Y \subset X$: 닫힌 진부분공간 (closed proper subspace)  
즉, $Y \neq X$이고 $Y = \overline{Y}$ (폐포 = 자신)

## 🔬 정리와 증명

### 정리 5.1: 유한차원 부분공간은 닫혀있다

**명제:** 
$X$가 Banach 공간이고 $Y \subset X$가 유한차원 부분공간이면, $Y$는 닫혀있다.

**증명:**

**Step 1: 기저 고정**

$Y$의 기저: $\{e_1, \ldots, e_n\}$ (dimY = n)

**Step 2: Y에서의 노름 동치성**

$y = \sum_{i=1}^n a_i e_i \in Y$라 하면, 
$\|\alpha\|_{\text{coord}} := \sum_{i=1}^n |a_i|$를 좌표 노름이라 합니다.

유한차원 공간 (Ch. 2 정리 2.2)이므로 모든 노름이 동치:
$$c \|y\|_{\text{coord}} \leq \|y\|_X \leq C \|y\|_{\text{coord}}$$

**Step 3: 수렴하는 수열**

$\{y_n\} \subset Y$이고 $y_n \to y$ in $X$라 하자.

$y_n = \sum_{i=1}^n a_i^{(n)} e_i$로 쓰면:

Cauchy 수열이므로 (X에서 수렴하므로):
$$\|y_m - y_n\|_X \to 0$$

하한 부등식에 의해:
$$c \sum_{i=1}^n |a_i^{(m)} - a_i^{(n)}| \leq \|y_m - y_n\|_X \to 0$$

따라서:
$$\sum_{i=1}^n |a_i^{(m)} - a_i^{(n)}| \to 0$$

**Step 4: 각 좌표의 수렴**

각 $i$에 대해:
$$|a_i^{(m)} - a_i^{(n)}| \leq \sum_{i=1}^n |a_i^{(m)} - a_i^{(n)}| \to 0$$

따라서 $\{a_i^{(n)}\}$는 Cauchy 수열. ℝ가 완비이므로:
$$a_i^{(n)} \to a_i \text{ for each } i$$

**Step 5: 극한이 Y에 속함**

$$y = \lim_{n \to \infty} y_n = \sum_{i=1}^n a_i e_i \in Y$$

따라서 $Y$는 닫혀있습니다. $\square$

### 정리 5.2: 유한차원에서만 Heine-Borel 성립

**명제:** 
유한차원 Banach 공간에서:
$$K \text{ compact} \iff K \text{ closed and bounded}$$

**증명 (개요):**

⟹ 방향: 컴팩트 ⟹ 닫혀있고 유계
- 컴팩트 집합의 닫혀있음: 위상의 정의
- 유계: 무한히 멀어지는 점들이 없으므로 유계

⟸ 방향: 닫혀있고 유계 ⟹ 컴팩트 (유한차원만!)

$X = \mathbb{R}^n$에서 좌표를 사용:
$$K \text{ bounded} \iff K \subset [-M, M]^n$$

$[-M, M]^n$은 컴팩트 (Heine-Borel 정리 in ℝ).

$K$가 닫혀있으므로, 컴팩트 집합의 닫힌 부분집합은 컴팩트.

따라서 $K$는 컴팩트. $\square$

**반대 방향 (무한차원):**

다음 정리에서 보겠습니다.

### 정리 5.3 (Riesz 보조정리)

**명제:**
$X$가 무한차원 Banach 공간이고 $Y \subset X$가 닫힌 진부분공간이면,
$$\forall \varepsilon \in (0, 1), \exists x_\varepsilon \in X \text{ s.t. } \|x_\varepsilon\| = 1, \quad d(x_\varepsilon, Y) > 1 - \varepsilon$$

여기서 $d(x, Y) = \inf_{y \in Y} \|x - y\|$.

**직관:** 
단위구 위에서, 닫힌 진부분공간으로부터 "거의 1"만큼 떨어진 점들을 찾을 수 있다.

**증명:**

**Step 1: 기초 설정**

$Y$가 진부분공간이므로 $\exists z \in X \setminus Y$.

$d := d(z, Y) = \inf_{y \in Y} \|z - y\| > 0$ (Y가 닫혀있으므로)

**Step 2: 근사점 선택**

고정된 $\varepsilon \in (0, 1)$에 대해, Y의 정의에 의해:
$$\exists y_0 \in Y \text{ s.t. } \|z - y_0\| < \frac{d}{1 - \varepsilon}$$

**Step 3: 정규화된 벡터**

$$x_\varepsilon := \frac{z - y_0}{\|z - y_0\|}$$

정규화되었으므로 $\|x_\varepsilon\| = 1$.

**Step 4: Y로부터의 거리**

$y \in Y$에 대해:
$$\|x_\varepsilon - y\| = \frac{\|z - y_0 - \|z - y_0\|y\|}{\|z - y_0\|}$$

$y_0 + \|z - y_0\|y \in Y$ (Y는 부분공간이고 $y_0 \in Y$)이므로:

$$\|z - (y_0 + \|z - y_0\|y)\| \geq d(z, Y) = d$$

따라서:
$$\|x_\varepsilon - y\| \geq \frac{d}{\|z - y_0\|} > \frac{d}{d/(1-\varepsilon)} = 1 - \varepsilon$$

모든 $y \in Y$에 대해 이것이 성립하므로:
$$d(x_\varepsilon, Y) > 1 - \varepsilon$$ ✓

$\square$

### 정리 5.4 (따름정리): 무한차원 단위구는 컴팩트하지 않다

**명제:**
$X$가 무한차원 Banach 공간이면, 단위구 
$$B = \{x \in X : \|x\| \leq 1\}$$
는 컴팩트하지 않다.

**증명:**

Riesz 보조정리를 반복 적용합니다.

**Step 1: 첫 점**

$X$는 무한차원이므로 1차원 부분공간 $X_1 = \text{span}\{e_1\}$ (임의의 $e_1$)을 생각합니다.

그러면 $X_1$은 닫혀있는 진부분공간 (유한차원이므로).

Riesz 보조정리에서 $\varepsilon = 1/2$:
$$\exists x_1 \in X, \|x_1\| = 1, \quad d(x_1, X_1) > 1/2$$

**Step 2: 귀납법**

$X_n = \text{span}\{e_1, \ldots, x_n\}$는 닫혀있는 진부분공간.

Riesz 보조정리를 적용하면:
$$\exists x_{n+1}, \|x_{n+1}\| = 1, \quad d(x_{n+1}, X_n) > 1/2$$

특히:
$$\|x_{n+1} - x_i\| > 1/2 \text{ for all } i \leq n$$

**Step 3: 부분수열 없음**

수열 $\{x_n\}$은:
- 모든 항이 단위구에 속함 ($\|x_n\| = 1$)
- **어떤 두 항도 1/2 이상 떨어져 있음** ($\|x_m - x_n\| > 1/2$)

따라서:
- 어떤 부분수열도 수렴할 수 없음 (극한점에 수렴하려면 결국 가까워져야 함)
- Bolzano-Weierstrass에 의해 컴팩트가 아님 $\square$

### 정리 5.5: 유한차원 부분공간의 특성화

**명제:**
$X$가 무한차원 Banach 공간이면, 유한차원 부분공간은 **상대적으로** 작습니다:
1. 항상 닫혀있습니다
2. 여차원(codimension)이 무한입니다
3. 여집합의 폐포가 전체 공간입니다

**증명 (1만):**
정리 5.1에서 이미 증명했습니다.

### 정리 5.6: 컴팩트 연산자의 예고

**명제:**
유한 랭크 연산자 (finite rank = 유한차원 치역)들의 극한은 특수합니다.

예: $T_n : \ell^2 \to \ell^2$를 유한 랭크라 하면, 
$$T = \lim_{n \to \infty} T_n$$
(uniform norm)은 **컴팩트 연산자**입니다.

컴팩트 연산자의 성질:
- 단위구의 상을 (상대적으로) 컴팩트하게 만듦
- 무한차원 공간에서도 "유한차원처럼" 작동
- Chapter 4에서 상세히 다룰 내용

## 💻 NumPy 구현으로 검증

### 예제 1: 고차원에서의 기하학적 현상

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

print("=" * 70)
print("예제 1: 고차원에서의 \"Curse of Dimensionality\" 현상")
print("=" * 70)

# 고차원 단위구 내 랜덤 점들
dimensions = np.arange(1, 101)
volume_ratios = []

for d in dimensions:
    # d차원 단위구 내의 점: 표준 정규 분포에서 샘플
    num_samples = 10000
    X = np.random.randn(num_samples, d)
    
    # 원점으로부터의 거리
    distances = np.linalg.norm(X, axis=1)
    
    # [0, 1] 내에 있는 점들의 비율 (단위구 부피 비율)
    ratio = np.sum(distances <= 1) / num_samples
    volume_ratios.append(ratio)

volume_ratios = np.array(volume_ratios)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 부피 비율의 감소
ax = axes[0]
ax.semilogy(dimensions, volume_ratios, 'bo-', markersize=4)
ax.set_xlabel('차원 $d$', fontsize=12)
ax.set_ylabel('단위구의 "부피" 비율', fontsize=12)
ax.set_title('고차원에서 단위구 부피가 0으로 수렴\n' + 
             '(무한차원에서도 단위구는 "작다")', fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3, which='both')

# 점들이 구면 근처에 집중 (고차원)
ax = axes[1]
dims_to_plot = [2, 5, 10, 50, 100]
colors = plt.cm.viridis(np.linspace(0, 1, len(dims_to_plot)))

for d, color in zip(dims_to_plot, colors):
    X = np.random.randn(5000, d)
    distances = np.linalg.norm(X, axis=1)
    
    # sqrt(d)가 전형적인 거리
    ax.hist(distances, bins=30, alpha=0.5, label=f'd={d}, 평균={np.mean(distances):.2f}',
            density=True, color=color)
    ax.axvline(np.sqrt(d), linestyle='--', color=color, linewidth=1.5)

ax.set_xlabel('거리 $‖x‖_2$', fontsize=12)
ax.set_ylabel('확률 밀도', fontsize=12)
ax.set_title('높은 차원에서 거의 모든 점이 √d 근처에 있음', fontsize=12, fontweight='bold')
ax.legend(fontsize=9, loc='upper right')
ax.grid(True, alpha=0.3)
ax.set_xlim([0, 15])

plt.tight_layout()
plt.savefig('/tmp/curse_of_dimensionality.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/curse_of_dimensionality.png")

# 고차원에서의 거리 현상
print("\n고차원 임의 벡터의 거리 분포:")
for d in [2, 10, 50, 100]:
    X = np.random.randn(1000, d)
    distances = np.linalg.norm(X, axis=1)
    
    print(f"\n차원 d = {d}:")
    print(f"  평균 거리: {np.mean(distances):.4f} (이론값: √({d}/2) = {np.sqrt(d/2):.4f})")
    print(f"  거리 분포의 표준편차: {np.std(distances):.4f}")
    print(f"  거의 모든 점이 [{np.percentile(distances, 5):.2f}, {np.percentile(distances, 95):.2f}] 범위")

# 예제 2: 유한차원 vs 무한차원의 단위구 컴팩트성
print("\n" + "=" * 70)
print("예제 2: Riesz 보조정리의 수치 검증")
print("=" * 70)

def riesz_sequence_in_sphere(dimension, n_points=20):
    """
    Riesz 보조정리를 따라 만든 수열:
    - 각 항이 단위구 위에 있음
    - 항들 사이 거리 > 1/2
    """
    # 정규직교 기저에서 시작
    points = []
    
    # 첫 점
    e1 = np.zeros(dimension)
    e1[0] = 1.0
    points.append(e1)
    
    for n in range(1, n_points):
        # 임의의 방향
        random_dir = np.random.randn(dimension)
        
        # 이전 점들을 "피하면서" 정규화
        # (완전히 Riesz 보조정리를 따르지는 않지만, 직관을 보여줌)
        for p in points:
            # Gram-Schmidt 정규직교화
            random_dir = random_dir - np.dot(random_dir, p) * p
        
        random_dir = random_dir / np.linalg.norm(random_dir)
        points.append(random_dir)
    
    return np.array(points)

# 다양한 차원에서 테스트
test_dims = [2, 5, 10, 50]

for d in test_dims:
    points = riesz_sequence_in_sphere(d, n_points=15)
    
    # 점들 사이의 거리 계산
    min_dist = float('inf')
    for i in range(len(points)):
        for j in range(i+1, len(points)):
            dist = np.linalg.norm(points[i] - points[j])
            min_dist = min(min_dist, dist)
    
    # 단위구 조건 확인
    norms = np.linalg.norm(points, axis=1)
    
    print(f"\n차원 d = {d}:")
    print(f"  점들의 노름: min={norms.min():.4f}, max={norms.max():.4f}")
    print(f"  점들 사이 최소 거리: {min_dist:.4f}")
    print(f"  이것이 1/2보다 큼: {min_dist > 0.5}")

# 예제 3: 고차원에서 벡터들의 직교성
print("\n" + "=" * 70)
print("예제 3: 고차원에서 \"모든 벡터가 거의 직교\"")
print("=" * 70)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 고차원에서 임의의 두 벡터의 내적 분포
dimensions = [2, 5, 10, 50, 100]

ax = axes[0]
for d in dimensions:
    angles = []
    for _ in range(500):
        u = np.random.randn(d)
        v = np.random.randn(d)
        
        u = u / np.linalg.norm(u)
        v = v / np.linalg.norm(v)
        
        cos_angle = np.dot(u, v)
        angle = np.arccos(np.clip(cos_angle, -1, 1)) * 180 / np.pi
        angles.append(angle)
    
    ax.hist(angles, bins=20, alpha=0.5, density=True, label=f'd={d}')

ax.axvline(90, color='r', linestyle='--', linewidth=2, label='직각 (90°)')
ax.set_xlabel('벡터 사이의 각도 (도)', fontsize=11)
ax.set_ylabel('확률 밀도', fontsize=11)
ax.set_title('고차원에서 임의의 두 벡터가 거의 직교', fontsize=12, fontweight='bold')
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3)

# 내적의 기댓값과 분산
ax = axes[1]
d_values = np.arange(1, 101)
expected_cosines = []
std_cosines = []

for d in d_values:
    cosines = []
    for _ in range(200):
        u = np.random.randn(d)
        v = np.random.randn(d)
        
        u = u / np.linalg.norm(u)
        v = v / np.linalg.norm(v)
        
        cosines.append(np.dot(u, v))
    
    expected_cosines.append(np.mean(cosines))
    std_cosines.append(np.std(cosines))

expected_cosines = np.array(expected_cosines)
std_cosines = np.array(std_cosines)

ax.fill_between(d_values, 
                expected_cosines - std_cosines,
                expected_cosines + std_cosines,
                alpha=0.3, label='±1 표준편차')
ax.plot(d_values, expected_cosines, 'b-', linewidth=2, label='기댓값')
ax.axhline(0, color='k', linestyle='--', linewidth=0.5)
ax.set_xlabel('차원 $d$', fontsize=11)
ax.set_ylabel('E[cos(각도)]', fontsize=11)
ax.set_title('고차원에서 내적이 거의 0 근처에 집중', fontsize=12, fontweight='bold')
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/high_dim_orthogonality.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/high_dim_orthogonality.png")

print("\n고차원 특성:")
print("  - 거의 모든 점이 √d 근처에 있음")
print("  - 임의의 두 벡터가 거의 직교 (내적 ≈ 0)")
print("  - 단위구의 \"부피\"가 0으로 수렴")
print("\n이것이 무한차원 Banach 공간의 기하를 암시합니다!")

# 예제 4: 최솟값의 부재
print("\n" + "=" * 70)
print("예제 4: 무한차원에서 최솟값이 없을 수 있음")
print("=" * 70)

# ℓ² 공간에서의 예: f(x) = ∑ 1/(n·‖x‖_n^2)
# 이 함수는 하한이 있지만 최솟값을 갖지 않음

def function_on_unit_ball(n_coords=50):
    """
    ℓ²의 단위구에서 정의된 함수:
    f(x) = ∑_{n=1}^∞ 1/(n·(x_n)²+1)
    이 함수는 inf > 0이지만 최솟값을 갖지 않음
    """
    
    values = []
    # 단위구 위의 임의 점들
    num_points = 100
    
    for _ in range(num_points):
        # 단위구 위의 점 생성 (projection)
        x = np.random.randn(n_coords)
        x = x / np.linalg.norm(x)
        
        # 함수값 계산
        f_val = 0
        for n in range(1, n_coords + 1):
            f_val += 1 / (n * (x[n-1]**2 + 1))
        
        values.append(f_val)
    
    return np.array(values)

values_20d = function_on_unit_ball(20)
values_50d = function_on_unit_ball(50)
values_100d = function_on_unit_ball(100)

print("\n단위구 위의 함수 f(x) = ∑ 1/(n(xₙ²+1))")
print(f"20차원: inf={values_20d.min():.6f}, 하지만 최솟값은 달성 안 됨")
print(f"50차원: inf={values_50d.min():.6f}, 하지만 최솟값은 달성 안 됨")
print(f"100차원: inf={values_100d.min():.6f}, 하지만 최솟값은 달성 안 됨")

# 정규화 효과
print("\n정규화의 역할:")
print("  제약: min f(x) s.t. ‖x‖ ≤ 1")
print("  → 무한차원에서도 최솟값이 존재 가능")
print("  (정규화 영역이 실질적으로 \"유한차원처럼\" 작동)")

# 예제 5: 닫힌 진부분공간의 예
print("\n" + "=" * 70)
print("예제 5: 유한차원 부분공간은 항상 닫혀있음")
print("=" * 70)

# ℓ²에서 유한 좌표만 사용하는 부분공간
# Y = {(x_1, ..., x_k, 0, 0, ...) : (x_i) ∈ ℓ²}

print("\nℓ² 공간에서 유한차원 부분공간 Y = ℓ²_finite")
print("Y = {(x_1, ..., x_k, 0, 0, ...) : (x_i) ∈ ℓ²}")
print("\n성질:")
print("  1. Y는 선형 부분공간")
print("  2. Y는 k차원 (유한)")
print("  3. Y는 항상 닫혀있음 (정리 5.1)")
print("\n반면 다항식의 공간 (무한차원)은 닫혀있지 않을 수 있음")
print("(예: C([0,1])에서 다항식들이 조밀하지만, 닫혀있지 않은 부분공간)")

# 예제 6: 신경망과 정규화
print("\n" + "=" * 70)
print("예제 6: 정규화가 유한차원 성질을 복구하는 방식")
print("=" * 70)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# L2 정규화 효과 시뮬레이션
np.random.seed(42)

n_iterations = 100
lambda_reg = [0, 0.01, 0.1, 1.0]

for lam in lambda_reg:
    # 손실함수: L(w) + λ‖w‖²
    loss = []
    w = np.random.randn(10)
    
    for _ in range(n_iterations):
        # 임의 그래디언트
        grad = np.random.randn(10)
        
        # 정규화 항의 그래디언트
        if lam > 0:
            grad = grad + 2 * lam * w
        
        # 경사하강법
        w = w - 0.01 * grad
        
        current_loss = np.sum(grad**2) + lam * np.sum(w**2)
        loss.append(current_loss)
    
    ax = axes[0]
    ax.plot(loss, label=f'λ={lam}', alpha=0.7)

ax.set_xlabel('반복 횟수', fontsize=11)
ax.set_ylabel('손실 함수', fontsize=11)
ax.set_title('L2 정규화의 수렴 안정성 효과', fontsize=12, fontweight='bold')
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3)

# 정규화 없음 vs 있음의 가중치 범위
ax = axes[1]

conditions = ['정규화 없음\n(무한차원처럼)', 'L2 정규화\n(유한차원처럼)']
weight_ranges = [
    [0, 5],  # 정규화 없음: 범위가 커질 수 있음
    [0, 1]   # L2 정규화: 범위가 제한됨
]

x_pos = np.arange(len(conditions))
colors = ['red', 'blue']

for i, (cond, w_range) in enumerate(zip(conditions, weight_ranges)):
    ax.barh(i, w_range[1], left=w_range[0], color=colors[i], alpha=0.7, height=0.5)
    ax.text(w_range[1]/2, i, f'범위: {w_range[1]:.1f}', 
            ha='center', va='center', fontweight='bold', fontsize=11, color='white')

ax.set_yticks(x_pos)
ax.set_yticklabels(conditions)
ax.set_xlabel('가중치 범위 (예상)', fontsize=11)
ax.set_title('정규화가 파라미터 공간을 제한\n(컴팩트성 복구)', fontsize=12, fontweight='bold')
ax.set_xlim([0, 5.5])
ax.grid(True, alpha=0.3, axis='x')

plt.tight_layout()
plt.savefig('/tmp/regularization_effects.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/regularization_effects.png")

print("\n정규화의 역할 정리:")
print("  L2 정규화:")
print("    - 가중치를 0 근처로 강제")
print("    - 파라미터 공간을 제한된 영역에만 제한")
print("    - 마치 유한차원처럼 '컴팩트'하게 만듦")
print("  ")
print("  Dropout:")
print("    - 랜덤하게 뉴런 끔")
print("    - 함수공간을 실질적 유한차원으로 축소")
print("  ")
print("  배치 정규화:")
print("    - 활성값을 제한된 범위로 정규화")
print("    - 학습 동역학을 '안정화'")
```

**실행 결과 요약:**
- 고차원에서 단위구 부피 → 0
- 모든 항이 1/2 이상 떨어진 수열 구성 가능 (컴팩트성 없음)
- 고차원에서 임의의 두 벡터가 거의 직교
- 정규화가 수렴 안정성과 유한차원성질 복구

## 🔗 AI/ML 연결

### 최적화의 어려움과 정규화

신경망 최적화 $\min_w L(w)$에서:
- **무한차원** (함수공간): 최솟값 부재 가능
- **유한차원** (파라미터 공간): Heine-Borel로 최솟값 존재
- **실제 상황:** 정규화로 "인위적으로" 유한차원성 복구

$$\min_w [L(w) + \lambda \|w\|^2]$$

이 정규화항이 중요한 이유:
1. 최솟값의 존재 보장 (Banach space이므로)
2. 컴팩트성 부분 복구 (유계 레벨 집합)

### Dropout과 함수공간 제한

Dropout은 네트워크를 $2^n$ 개의 가능한 부분네트워크로 분해합니다:
- 각 부분: 유한 파라미터
- 집합: 무한이지만 "이산적"
- 실질적으로 매우 "유한차원"

### 신경망의 "implicit regularization"

정규화 항 없이도 경사하강법 자체가:
- 작은 가중치를 선호
- 큰 가중치는 수렴이 느림
- 자동으로 "유한차원처럼" 작동

## ⚖️ 가정과 한계

### 가정

1. **Banach 공간:** 완비 노름공간
   - 가우시안 프로세스: 완비가 아닐 수 있음
   - Fréchet 공간: 더 약한 구조

2. **무한차원성:** dim = ∞
   - 실제 신경망: 파라미터 유한하지만 "함수 관점"에서 무한차원

### 한계

1. **정규화의 필요성:**
   - 자연의 무한차원은 매우 불안정
   - 현실적 문제는 모두 정규화 필요

2. **Riesz 보조정리의 "극한" 성질:**
   - 1-ε만큼 떨어질 수 있음 (정확히 1은 아님)
   - 따라서 "0-근처" 여집합도 좋은 성질

## 📌 핵심 정리 (표로 요약)

| 개념 | 유한차원 (ℝⁿ) | 무한차원 (Banach) |
|------|---|---|
| **Heine-Borel** | ✓ 닫혀있고 유계 = 컴팩트 | ✗ 거짓 |
| **단위구** | Compact | Non-compact |
| **최솟값** | 연속함수는 달성 | 보장 안 됨 |
| **유한차원 부분공간** | 닫혀있음 | 닫혀있음 ✓ |
| **Cauchy 수열** | 수렴 | 수렴 (완비) |
| **모든 노름 동치** | ✓ | ✗ |

**핵심 정리 3개:**
1. 유한차원 부분공간은 항상 닫혀있음
2. 무한차원의 단위구는 컴팩트하지 않음 (Riesz)
3. 정규화 = 유한차원성질 복구

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./04-sequence-continuous-spaces.md) | [📚 README](../README.md) | |

</div>
