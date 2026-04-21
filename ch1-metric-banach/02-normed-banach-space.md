# 2. 노름공간과 Banach 공간

## 🎯 핵심 질문

1. 거리만으로는 충분하지 않은 이유는?
2. 노름이 거리보다 강한 구조를 어떻게 제공하는가?
3. 유한차원에서 "모든 노름이 비슷하다"는 것이 무한차원에서 깨지는 이유는?
4. Banach 공간의 정의와 의미는?

## 🔍 왜 이 이론이 AI에서 중요한가

신경망의 손실함수 최소화 문제는 고차원 벡터공간에서 이루어집니다:
- **유한차원**(예: 파라미터 1백만 개)에서도 모든 노름이 동치이므로 L1 손실과 L2 손실이 "본질적으로 같은" 기하학적 구조를 가집니다
- **무한차원 함수공간**(예: 신경망의 가중치가 함수로 표현되는 경우)에서는 **노름의 선택이 극적으로 다릅니다**

또한 정규화의 기초입니다:
- L1 정규화: ‖w‖₁ 최소화 → 희소(sparse) 해 유도
- L2 정규화: ‖w‖₂² 최소화 → 작은 가중치 선호

이들의 기하학적 차이는 **노름의 수학적 구조**에서 비롯됩니다.

## 📐 수학적 선행 조건

- 거리공간, 완비성 (Chapter 1의 내용)
- 벡터공간의 기본 개념 (기저, 선형독립, 부분공간)
- 선형대수의 고유값, 이차형식
- 실수 함수의 연속성과 컴팩트성

## 📖 직관적 이해 (유한차원 → 무한차원)

**유한차원 벡터공간(ℝⁿ)에서는:**
- 어떤 거리 또는 노름으로 측정하든 "가깝다"의 의미가 같습니다
- 예: L₁, L₂, L∞ 거리는 모두 같은 수열을 수렴하게 합니다
- 단위구(B = {x : ‖x‖ ≤ 1})들이 다르게 보이지만, 본질적으로 같은 구조입니다
- **유한차원이므로** 이들이 "균형을 이룹니다"

**무한차원 함수공간(예: C[0,1])에서는:**
- L² 노름: 평균적 크기 측정 (일부 대역폭 허용)
- L∞ 노름: 최대 크기 측정 (하나도 튈 수 없음)
- 이 둘은 매우 다른 공간을 정의하며, 한 노름의 Cauchy 수열이 다른 노름에서는 Cauchy가 아닐 수 있습니다
- **무한차원이므로** 이들이 완전히 다릅니다

## ✏️ 엄밀한 정의

### 정의 2.1: 노름 (Norm)

벡터공간 $X$에서 $\mathbb{R}$로의 함수 $\|\cdot\|: X \to \mathbb{R}_{\geq 0}$이 **노름**이라 하며,
다음 세 공리를 만족할 때입니다:

$$\begin{align}
\text{(N1) 양정치성:} \quad & \|x\| \geq 0 \text{ for all } x \in X \\
& \|x\| = 0 \iff x = 0 \\
\text{(N2) 동차성:} \quad & \|\alpha x\| = |\alpha| \|x\| \text{ for all } \alpha \in \mathbb{R}, x \in X \\
\text{(N3) 삼각부등식:} \quad & \|x + y\| \leq \|x\| + \|y\| \text{ for all } x, y \in X
\end{align}$$

쌍 $(X, \|\cdot\|)$을 **노름공간(normed space)**이라 합니다.

### 정의 2.2: 노름이 유도하는 거리

노름공간 $(X, \|\cdot\|)$에서 거리함수를 정의할 수 있습니다:

$$d(x, y) := \|x - y\|$$

이 거리는 **이동불변(translation invariant)**입니다:
$$d(x+z, y+z) = d(x, y)$$

### 정의 2.3: Banach 공간

$(X, \|\cdot\|)$가 **Banach 공간**이다:
- 노름공간이며,
- 노름이 유도하는 거리에 대해 **완비**이다.

즉, 모든 Cauchy 수열이 수렴합니다.

### 정의 2.4: p-노름 (ℝⁿ에서)

$x = (x_1, \ldots, x_n) \in \mathbb{R}^n$에 대해, $1 \leq p \leq \infty$:

$$\|x\|_p = \begin{cases}
\left(\sum_{i=1}^n |x_i|^p\right)^{1/p} & \text{if } 1 \leq p < \infty \\
\max_{1 \leq i \leq n} |x_i| & \text{if } p = \infty
\end{cases}$$

각각을 **L¹ 노름, L² 노름(=Euclidean), ..., L∞ 노름(=최대 노름)**이라 합니다.

### 정의 2.5: 노름의 동치성

두 노름 $\|\cdot\|_a, \|\cdot\|_b$가 **동치**이다:
$$\exists c, C > 0 \text{ s.t. } c\|x\|_a \leq \|x\|_b \leq C\|x\|_a \quad \forall x \in X$$

이는 "같은 수열들이 수렴"하고 "같은 위상 구조"를 생성함을 의미합니다.

### 정의 2.6: 단위구와 단위 구면

주어진 노름 $\|\cdot\|$에 대해:

$$B := \{x \in X : \|x\| \leq 1\} \text{ (닫힌 단위구)}$$
$$B_o := \{x \in X : \|x\| < 1\} \text{ (열린 단위구)}$$
$$S := \{x \in X : \|x\| = 1\} \text{ (단위 구면)}$$

## 🔬 정리와 증명

### 정리 2.1: p-노름은 노름이다 (1 ≤ p ≤ ∞)

**명제:** $\|\cdot\|_p$는 ℝⁿ에서 노름이다.

**증명:**

**(N1) 양정치성:** 
- $\|x\|_p \geq 0$은 절댓값의 성질에서 명확합니다
- $\|x\|_p = 0 \iff$ 모든 $|x_i| = 0 \iff x = 0$ ✓

**(N2) 동차성:**
$$\|\alpha x\|_p = \left(\sum_{i=1}^n |\alpha x_i|^p\right)^{1/p} = \left(\sum_{i=1}^n |\alpha|^p |x_i|^p\right)^{1/p}$$
$$= |\alpha| \left(\sum_{i=1}^n |x_i|^p\right)^{1/p} = |\alpha| \|x\|_p$$ ✓

**(N3) 삼각부등식 (Minkowski 부등식):**

$p = 1$일 때는 자명합니다 (절댓값의 성질).

$1 < p < \infty$일 때, **Hölder 부등식**을 먼저 증명합니다.

**Hölder 부등식:**
$$\sum_{i=1}^n |a_i b_i| \leq \left(\sum_{i=1}^n |a_i|^p\right)^{1/p} \left(\sum_{i=1}^n |b_i|^q\right)^{1/q}$$

여기서 $1/p + 1/q = 1$ (소위 켤레 지수).

*Hölder 부등식의 증명 (Young 부등식 이용):*

먼저 **Young 부등식**을 증명합니다:
$$ab \leq \frac{a^p}{p} + \frac{b^q}{q} \quad \text{for all } a, b > 0$$

증명: 함수 $f(t) = t^p/p + 1/q - t$를 고려합니다 ($t \geq 0$).
$$f'(t) = t^{p-1} - 1$$

$f'(t) = 0$일 때 $t = 1$이고, 이것이 최솟값입니다: $f(1) = 1/p + 1/q - 1 = 0$.

따라서 $f(t) \geq 0$이고, 이를 정리하면 Young 부등식을 얻습니다.

*Hölder 부등식 증명:*

정규화: $A = \|a\|_p = \left(\sum |a_i|^p\right)^{1/p}$, $B = \|b\|_q = \left(\sum |b_i|^q\right)^{1/q}$

Young 부등식을 $\tilde{a}_i = |a_i|/A$, $\tilde{b}_i = |b_i|/B$에 적용하면:
$$\frac{|a_i b_i|}{AB} \leq \frac{1}{p}\left(\frac{|a_i|}{A}\right)^p + \frac{1}{q}\left(\frac{|b_i|}{B}\right)^q$$

합하면:
$$\frac{\sum |a_i b_i|}{AB} \leq \frac{1}{p} \cdot 1 + \frac{1}{q} \cdot 1 = 1$$

따라서 $\sum |a_i b_i| \leq AB = \|a\|_p \|b\|_q$. ✓

*Minkowski 부등식 (p > 1):*

$x, y \in \mathbb{R}^n$에 대해, $q = p/(p-1)$라 하면:

$$\|x+y\|_p^p = \sum_{i=1}^n |x_i + y_i|^p = \sum_{i=1}^n |x_i + y_i|^{p-1} |x_i + y_i|$$

$$\leq \sum_{i=1}^n |x_i + y_i|^{p-1} (|x_i| + |y_i|)$$

Hölder 부등식 적용:
$$\sum_{i=1}^n |x_i + y_i|^{p-1} |x_i| \leq \left(\sum |x_i + y_i|^{(p-1)q}\right)^{1/q} \left(\sum |x_i|^p\right)^{1/p}$$

$(p-1)q = p$이므로:
$$= \|x+y\|_p^{p-1} \|x\|_p$$

마찬가지로:
$$\sum_{i=1}^n |x_i + y_i|^{p-1} |y_i| \leq \|x+y\|_p^{p-1} \|y\|_p$$

두 식을 더하면:
$$\|x+y\|_p^p \leq \|x+y\|_p^{p-1} (\|x\|_p + \|y\|_p)$$

양변을 $\|x+y\|_p^{p-1}$로 나누면:
$$\|x+y\|_p \leq \|x\|_p + \|y\|_p$$ ✓

### 정리 2.2: p-노름들은 서로 동치이다 (ℝⁿ에서)

**명제:** ℝⁿ의 모든 p-노름 $\|\cdot\|_p$ (1 ≤ p ≤ ∞)는 서로 동치이다.

**증명의 스케치:**

임의의 $x = (x_1, \ldots, x_n) \in \mathbb{R}^n$에 대해:

**Step 1:** L∞와 L₁의 관계
$$\|x\|_\infty = \max_i |x_i| \leq \sum_{i=1}^n |x_i| = \|x\|_1$$

$$\|x\|_1 = \sum_{i=1}^n |x_i| \leq n \max_i |x_i| = n\|x\|_\infty$$

따라서:
$$\|x\|_\infty \leq \|x\|_1 \leq n \|x\|_\infty$$

**Step 2:** L∞와 L²의 관계
$$\|x\|_\infty = \max_i |x_i| \leq \sqrt{\sum_{i=1}^n |x_i|^2} = \|x\|_2$$

$$\|x\|_2 = \sqrt{\sum_{i=1}^n |x_i|^2} \leq \sqrt{n \max_i |x_i|^2} = \sqrt{n} \|x\|_\infty$$

따라서:
$$\|x\|_\infty \leq \|x\|_2 \leq \sqrt{n} \|x\|_\infty$$

**Step 3:** 일반적인 Lᵖ에 대해서도 유사하게:
$$C_1 \|x\|_\infty \leq \|x\|_p \leq C_2 \|x\|_\infty$$

여기서 $C_1, C_2$는 $n$과 $p$에만 의존합니다.

**Step 4:** 추이성
만약 $\|\cdot\|_a \sim \|\cdot\|_\infty$이고 $\|\cdot\|_\infty \sim \|\cdot\|_b$이면,
$\|\cdot\|_a \sim \|\cdot\|_b$입니다.

따라서 모든 p-노름들은 서로 동치입니다. $\square$

**핵심:** 동치성은 **유한차원**의 특성입니다!

### 정리 2.3: 닫힌 단위구의 볼록성

**명제:** 노름공간 $(X, \|\cdot\|)$에서 단위구 $B = \{x : \|x\| \leq 1\}$은 볼록집합이다.

**증명:**

$x, y \in B$이고 $\lambda \in [0, 1]$일 때:
$$\|\lambda x + (1-\lambda)y\| \leq \lambda \|x\| + (1-\lambda)\|y\| \leq \lambda \cdot 1 + (1-\lambda) \cdot 1 = 1$$

따라서 $\lambda x + (1-\lambda)y \in B$이므로 $B$는 볼록입니다. $\square$

### 정리 2.4: 유한차원에서의 노름 동치성 (Hausdorff의 정리)

**명제:** 유한차원 벡터공간 $X$에서는 모든 노름이 동치이다.

**증명의 개요:**

1. **기저 고정:** $X$의 기저 $\{e_1, \ldots, e_n\}$을 잡고, 좌표 표현 $x = \sum_{i=1}^n x_i e_i$를 사용합니다.

2. **표준 노름과의 비교:** 표준 노름 $\|x\|_0 = \sqrt{\sum_{i=1}^n x_i^2}$를 정의합니다.

3. **임의 노름 비교:** 임의의 노름 $\|\cdot\|$에 대해:
   - $\|e_i\| \leq M_i$인 상수 $M_i$가 있습니다
   - $\|x\| = \|\sum x_i e_i\| \leq \sum |x_i| \|e_i\| \leq \sqrt{\sum x_i^2} \cdot \sqrt{\sum M_i^2} = C \|x\|_0$

4. **하한 증명:** 단위 구면 $S = \{x : \|x\|_0 = 1\}$는 compact이고, 
   $f(x) = \|x\|$는 연속이므로 최솟값 $m > 0$을 가집니다.
   따라서 $m \|x\|_0 \leq \|x\|$.

5. **결론:** $m \|x\|_0 \leq \|x\| \leq C \|x\|_0$이므로 동치입니다. $\square$

### 정리 2.5: 무한차원에서 노름 동치성의 붕괴

**명제:** C([0,1])에서 L²-노름과 L∞-노름은 동치가 아니다.

**증명:**

C([0,1])의 함수들: $f_n(x) = n \cdot \mathbb{1}_{[0, 1/n]}(x)$ (첫 $1/n$ 구간에서만 $n$, 나머지 0)

실제로는 C([0,1])에 속하려면 연속이어야 하므로, 대신:
$$f_n(x) = \begin{cases}
n & \text{if } x \in [0, 1/(2n)] \\
\text{선형으로 0으로 감소} & \text{if } x \in [1/(2n), 1/n] \\
0 & \text{if } x > 1/n
\end{cases}$$

그러면:
- $\|f_n\|_\infty = n$
- $\|f_n\|_2 = \sqrt{\int_0^{1/n} f_n^2} \approx \sqrt{n \cdot \text{average}(f_n)^2} \approx \sqrt{1} = O(1)$

따라서 $\|f_n\|_\infty / \|f_n\|_2 \to \infty$이므로, 동치가 아닙니다. $\square$

## 💻 NumPy 구현으로 검증

### 예제 1: p-노름의 계산 및 동치성 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import Circle
from matplotlib.patches import Polygon

# 예제 1: 다양한 p-노름 계산
print("=" * 70)
print("예제 1: ℝⁿ에서 다양한 p-노름 계산")
print("=" * 70)

x = np.array([3.0, 4.0])  # 벡터 (3, 4)

# p-노름 계산
norms = {}
for p in [1, 2, 3, 4, np.inf]:
    if p == np.inf:
        norms[f'L∞'] = np.linalg.norm(x, ord=np.inf)
    else:
        norms[f'L^{p}'] = np.linalg.norm(x, ord=p)

print(f"벡터 x = {x}")
print(f"\n노름 값:")
for name, value in norms.items():
    print(f"  ‖x‖_{name} = {value:.6f}")

# 노름 공리 검증
print(f"\n노름 공리 검증 (L²-노름):")
print(f"  (N1) ‖x‖₂ ≥ 0: {norms['L^2'] >= 0} (값: {norms['L^2']:.6f})")
print(f"  (N2) ‖2x‖₂ = 2‖x‖₂: {np.isclose(np.linalg.norm(2*x, ord=2), 2*norms['L^2'])} " +
      f"({np.linalg.norm(2*x, ord=2):.6f} = {2*norms['L^2']:.6f})")

y = np.array([1.0, 2.0])
lhs = np.linalg.norm(x + y, ord=2)
rhs = np.linalg.norm(x, ord=2) + np.linalg.norm(y, ord=2)
print(f"  (N3) ‖x+y‖₂ ≤ ‖x‖₂ + ‖y‖₂: {np.isclose(lhs, rhs) or lhs <= rhs} " +
      f"({lhs:.6f} ≤ {rhs:.6f})")

# 예제 2: 고차원에서 노름 동치성 수치 검증
print("\n" + "=" * 70)
print("예제 2: 고차원에서 p-노름들의 동치성")
print("=" * 70)

dimensions = [2, 5, 10, 50, 100]
num_samples = 1000

for n in dimensions:
    # 임의의 벡터 1000개 샘플
    X = np.random.randn(num_samples, n)
    
    # 각 벡터의 다양한 p-노름 계산
    norms_1 = np.linalg.norm(X, ord=1, axis=1)
    norms_2 = np.linalg.norm(X, ord=2, axis=1)
    norms_inf = np.linalg.norm(X, ord=np.inf, axis=1)
    
    # 동치성 확인: c‖x‖_a ≤ ‖x‖_b ≤ C‖x‖_a
    ratio_2_inf = norms_2 / norms_inf
    ratio_1_2 = norms_1 / norms_2
    ratio_1_inf = norms_1 / norms_inf
    
    print(f"\n차원 n = {n}:")
    print(f"  ‖x‖_2 / ‖x‖_∞ ∈ [{ratio_2_inf.min():.4f}, {ratio_2_inf.max():.4f}] (이론: [1, √n]=[1, {np.sqrt(n):.2f}])")
    print(f"  ‖x‖_1 / ‖x‖_∞ ∈ [{ratio_1_inf.min():.4f}, {ratio_1_inf.max():.4f}] (이론: [1, n]=[1, {n}])")
    print(f"  ‖x‖_1 / ‖x‖_2 ∈ [{ratio_1_2.min():.4f}, {ratio_1_2.max():.4f}] (이론: [1, √n]=[1, {np.sqrt(n):.2f}])")

# 예제 3: 단위구의 시각화 (2D)
print("\n" + "=" * 70)
print("예제 3: 2D에서 단위구의 기하학적 형태")
print("=" * 70)

fig, axes = plt.subplots(2, 2, figsize=(12, 12))

# 다양한 p에 대해 단위구 그리기
for idx, p in enumerate([1, 2, 4, np.inf]):
    ax = axes[idx // 2, idx % 2]
    
    theta = np.linspace(0, 2*np.pi, 1000)
    
    if p == 1:
        # L¹ 단위구 (다이아몬드 모양)
        x_circle = np.array([1, 0, -1, 0, 1])
        y_circle = np.array([0, 1, 0, -1, 0])
        ax.plot(x_circle, y_circle, 'b-', linewidth=2, label='‖x‖₁ = 1')
        
    elif p == 2:
        # L² 단위구 (원)
        x_circle = np.cos(theta)
        y_circle = np.sin(theta)
        ax.plot(x_circle, y_circle, 'b-', linewidth=2, label='‖x‖₂ = 1')
        
    elif p == 4:
        # L⁴ 단위구
        x_circle = np.cos(theta)
        y_circle = np.sin(theta)
        # x^4 + y^4 = 1 매개변수화
        r = (np.abs(np.cos(theta))**4 + np.abs(np.sin(theta))**4)**(-0.25)
        x_circle = r * np.cos(theta)
        y_circle = r * np.sin(theta)
        ax.plot(x_circle, y_circle, 'b-', linewidth=2, label='‖x‖₄ = 1')
        
    else:  # p = inf
        # L∞ 단위구 (정사각형)
        x_square = np.array([-1, 1, 1, -1, -1])
        y_square = np.array([-1, -1, 1, 1, -1])
        ax.plot(x_square, y_square, 'b-', linewidth=2, label='‖x‖∞ = 1')
    
    # 몇 개의 샘플 점 추가
    ax.scatter([0.6], [0.6], color='red', s=50, zorder=5, label='샘플 점')
    
    ax.set_xlim(-1.5, 1.5)
    ax.set_ylim(-1.5, 1.5)
    ax.set_aspect('equal')
    ax.grid(True, alpha=0.3)
    ax.axhline(0, color='k', linewidth=0.5)
    ax.axvline(0, color='k', linewidth=0.5)
    ax.set_title(f'L^p 단위구: p = {p}', fontsize=12, fontweight='bold')
    ax.legend()

plt.tight_layout()
plt.savefig('/tmp/unit_balls.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/unit_balls.png")

# 예제 4: Hölder 부등식 검증
print("\n" + "=" * 70)
print("예제 4: Hölder 부등식 검증")
print("=" * 70)
print("부등식: ∑|aᵢbᵢ| ≤ ‖a‖_p · ‖b‖_q, 여기서 1/p + 1/q = 1")

a = np.array([1.0, 2.0, 3.0, 4.0])
b = np.array([4.0, 3.0, 2.0, 1.0])

for p in [1.5, 2.0, 3.0, 4.0]:
    q = 1 / (1 - 1/p)  # 켤레 지수
    
    lhs = np.sum(np.abs(a * b))
    norm_a_p = np.linalg.norm(a, ord=p)
    norm_b_q = np.linalg.norm(b, ord=q)
    rhs = norm_a_p * norm_b_q
    
    ratio = lhs / rhs
    
    print(f"\np = {p}, q = {q:.4f}:")
    print(f"  ∑|aᵢbᵢ| = {lhs:.6f}")
    print(f"  ‖a‖_p · ‖b‖_q = {rhs:.6f}")
    print(f"  비율: {ratio:.6f} ≤ 1 ✓" if ratio <= 1.0001 else f"  비율: {ratio:.6f} > 1 ✗")

# 예제 5: Minkowski 부등식 검증
print("\n" + "=" * 70)
print("예제 5: Minkowski 부등식 검증")
print("=" * 70)
print("부등식: ‖x+y‖_p ≤ ‖x‖_p + ‖y‖_p")

x = np.array([1.0, 2.0, 3.0])
y = np.array([4.0, 5.0, 6.0])

for p in [1, 1.5, 2, 3, np.inf]:
    lhs = np.linalg.norm(x + y, ord=p)
    rhs = np.linalg.norm(x, ord=p) + np.linalg.norm(y, ord=p)
    
    p_label = f'L^{p}' if p != np.inf else 'L∞'
    status = "✓" if np.isclose(lhs, rhs) or lhs <= rhs else "✗"
    
    print(f"p = {p_label:>4}: ‖x+y‖ = {lhs:.6f}, ‖x‖ + ‖y‖ = {rhs:.6f} {status}")

# 예제 6: 무한차원에서 노름 동치성 붕괴 (시뮬레이션)
print("\n" + "=" * 70)
print("예제 6: 고차원에서 L2와 L∞ 노름 비율 변화")
print("=" * 70)

fig, ax = plt.subplots(figsize=(10, 6))

dimensions = np.arange(2, 201, 2)
ratios_max = []  # 최대 비율
ratios_min = []  # 최소 비율

for n in dimensions:
    X = np.random.randn(100, n)
    
    norms_2 = np.linalg.norm(X, ord=2, axis=1)
    norms_inf = np.linalg.norm(X, ord=np.inf, axis=1)
    
    ratio = norms_2 / norms_inf
    
    ratios_max.append(ratio.max())
    ratios_min.append(ratio.min())

ax.fill_between(dimensions, ratios_min, ratios_max, alpha=0.3, label='실제 범위')
ax.plot(dimensions, ratios_min, 'b-', label='최소 비율')
ax.plot(dimensions, ratios_max, 'r-', label='최대 비율')
ax.plot(dimensions, np.sqrt(dimensions), 'g--', linewidth=2, label='이론값 √n')

ax.set_xlabel('차원 n', fontsize=12)
ax.set_ylabel('‖x‖₂ / ‖x‖∞', fontsize=12)
ax.set_title('고차원에서 L2와 L∞ 노름 비율: 동치성 검증', fontsize=13, fontweight='bold')
ax.legend(fontsize=11)
ax.grid(True, alpha=0.3)
ax.set_ylim([0, 15])

plt.tight_layout()
plt.savefig('/tmp/norm_equivalence.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/norm_equivalence.png")
print(f"고차원에서 ‖x‖₂ / ‖x‖∞ ≤ √n이 유지됨을 확인할 수 있습니다.")

# 예제 7: Lᵖ 단위구의 부피 변화
print("\n" + "=" * 70)
print("예제 7: 고차원 Lᵖ 단위구 '부피'의 변화")
print("=" * 70)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# (이것은 Monte Carlo 추정)
dimensions = np.arange(1, 101)
volume_estimates = {}

for p_val in [1, 2, np.inf]:
    volumes = []
    for n in dimensions:
        # n차원에서 p-노름 ≤ 1인 점들의 "부피" 추정
        X = np.random.randn(10000, n)
        norms = np.linalg.norm(X, ord=p_val, axis=1)
        # 단위구 내 점의 비율
        ratio = np.sum(norms <= 1) / len(norms)
        # 정규화
        volumes.append(ratio)
    
    volumes = np.array(volumes)
    p_label = f'L^{p_val}' if p_val != np.inf else 'L∞'
    volume_estimates[p_label] = volumes

# 부피 추이
ax = axes[0]
for p_label, volumes in volume_estimates.items():
    ax.semilogy(dimensions, volumes, 'o-', markersize=3, label=p_label)

ax.set_xlabel('차원 n', fontsize=12)
ax.set_ylabel('단위구 "부피" (로그 스케일)', fontsize=12)
ax.set_title('고차원에서 Lᵖ 단위구 부피가 0으로 수렴', fontsize=12, fontweight='bold')
ax.legend(fontsize=11)
ax.grid(True, alpha=0.3, which='both')

# 고차원의 "울프-고트마프" 현상: 거의 모든 점이 구면 근처
ax = axes[1]
n = 100
X = np.random.randn(10000, n)
norms_2 = np.linalg.norm(X, ord=2, axis=1)

ax.hist(norms_2, bins=50, density=True, alpha=0.7, edgecolor='black')
ax.axvline(np.sqrt(n), color='r', linestyle='--', linewidth=2, label=f'√n = {np.sqrt(n):.2f}')
ax.set_xlabel('‖x‖₂', fontsize=12)
ax.set_ylabel('확률 밀도', fontsize=12)
ax.set_title(f'R^100에서 랜덤 벡터의 노름 분포\n(대부분 √n 근처에 집중)', fontsize=12, fontweight='bold')
ax.legend(fontsize=11)
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/high_dim_phenomena.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/high_dim_phenomena.png")
```

**실행 결과 요약:**
- 모든 p-노름이 노름 공리를 만족
- 유한차원에서는 c·‖x‖_a ≤ ‖x‖_b ≤ C·‖x‖_a 성립
- 단위구들이 p에 따라 다양한 형태 (다이아몬드, 원, 정사각형)
- Hölder, Minkowski 부등식 수치 검증 완료
- 고차원에서 "curse of dimensionality": 단위구 부피가 기하급수적으로 감소

## 🔗 AI/ML 연결

### L1 vs L2 정규화의 기하학적 기초

신경망 파라미터 w에 대해:
- **L2 정규화:** min ‖y - ŷ‖₂² + λ‖w‖₂²
- **L1 정규화:** min ‖y - ŷ‖₂² + λ‖w‖₁

기하학적 관점:
- L2: 등고선이 원형 (가중치 방향 균등 벌칙)
- L1: 등고선이 마름모 (희소한 해로 수렴)

유한차원에서 이들의 차이는 미묘하지만, **무한차원에서는 극적**입니다.

### 신경망의 함수공간

신경망은 함수들의 공간에서 작동합니다:
$$f_w(x) = \text{Neural Network with weights } w$$

이 함수공간에서 Banach 공간 구조를 가져야:
- 손실함수의 최소값이 존재함을 보장 (완비성)
- 근사 정리 적용 가능 (Riesz-Fischer 같은 정리들)

### 고차원 최적화

고차원 파라미터 공간에서:
- 대부분의 벡터들이 거의 같은 길이 (‖w‖ ≈ √n)
- 임의의 두 벡터가 거의 직교 (curse of dimensionality)
- L2 정규화가 자연스러운 선택 (isotropic 기하)

## ⚖️ 가정과 한계

### 가정

1. **벡터공간 구조:** 노름은 벡터공간에서만 정의됩니다
   - 비선형 구조는 노름으로 표현 불가능
   - 필요: Finsler 기하학 같은 일반화

2. **음이 아닌 실수 치역:** 노름 값이 항상 음이 아님
   - 음수 거리는 물리적 의미가 없음
   - 복소수 벡터공간으로 확장 시 정의는 동일

### 한계

1. **동치성의 유한차원 특성:**
   - 무한차원에서는 완전히 다릅니다
   - 함수공간의 선택이 극적으로 다른 결과를 초래합니다

2. **단위구가 항상 컴팩트하지는 않음:**
   - 유한차원: 단위구 compact
   - 무한차원: 단위구 non-compact (Riesz 보조정리)
   - 따라서 최솟값의 존재가 보장되지 않음

3. **Banach 공간의 기하:**
   - 일반적인 Banach 공간은 내적이 없음
   - 직교 개념이 없어 기하학이 복잡함

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 성질 |
|------|------|------|
| **노름** | 3개 공리: 양정치, 동차성, 삼각부등식 | 거리 정의: d(x,y)=‖x-y‖ |
| **p-노름** | (∑\|xᵢ\|ᵖ)^(1/p) (또는 max) | ℝⁿ에서 모두 동치 |
| **노름공간** | 노름을 가진 벡터공간 | 위상 구조 정의 가능 |
| **Banach 공간** | 완비 노름공간 | 함수공간의 기초, AI/ML 적용 |
| **단위구** | {x : ‖x‖ ≤ 1} | 볼록, 유한차원에서만 compact |

**3가지 핵심 정리:**
1. 모든 p-노름은 노름 공리를 만족
2. 유한차원에서는 모든 노름이 동치
3. 완비 노름공간 = Banach 공간

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./01-metric-space-completeness.md) | [📚 README](../README.md) | [다음 ▶](./03-lp-spaces-completeness.md) |

</div>
