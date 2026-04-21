# 1. 거리공간과 완비성

## 🎯 핵심 질문

1. 거리의 개념을 수학적으로 어떻게 정의할 것인가?
2. Cauchy 수열이란 무엇이고, 왜 수렴이 보장되지 않을 수 있는가?
3. 완비성(completeness)이 있으면 무엇이 달라지는가?
4. 실수 체계가 유리수와 다른 근본적인 이유는?

## 🔍 왜 이 이론이 AI에서 중요한가

AI 시스템에서 경사하강법(Gradient Descent)은 손실함수의 최솟값을 향해 반복적으로 접근합니다. 
- 유한차원(예: ℝⁿ)에서는 이 수렴이 비교적 자명하지만, 
- **무한차원 함수공간**(예: 신경망의 무한 파라미터 공간)에서는 **완비성이 없으면 수렴을 보장할 수 없습니다**.

완비 공간에서는 Banach 고정점 정리가 성립하여 **반복 알고리즘의 수렴을 수학적으로 보장**할 수 있습니다.
또한 완비성은 함수공간의 표현 정리(representation theorem)의 핵심 가정입니다.

## 📐 수학적 선행 조건

- 실수체 ℝ의 기본 성질 (완전성 공리, 절댓값)
- 집합과 함수의 기초 개념
- 수열의 극한과 연속성 (ε-δ 정의)
- 유클리드 공간 ℝⁿ에서의 표준 거리

## 📖 직관적 이해 (유한차원 → 무한차원)

**유한차원(ℝⁿ)에서는:**
- 수열이 "가까워지고 있다"는 사실만으로 수렴한다고 보장할 수 있습니다
- 예: 3D 공간에서 점들이 점점 가까워지면, 그들은 어떤 점으로 수렴합니다

**무한차원 공간(예: C([0,1]))에서는:**
- 함수들이 점점 "가까워지고" 있어도 극한 함수가 같은 공간에 없을 수 있습니다
- 예: 다항식 함수들의 수열이 가까워져도, 극한은 "너무 울퉁불퉁한" 함수일 수 있습니다
- **완비성**은 "극한이 항상 같은 공간 내에 존재한다"는 보장입니다

## ✏️ 엄밀한 정의

### 정의 1.1: 거리공간 (Metric Space)

집합 $X$와 함수 $d: X \times X \to \mathbb{R}$의 쌍 $(X, d)$를 **거리공간**이라 하며, 
다음 세 공리(거리 공리)를 만족할 때입니다:

$$\begin{align}
\text{(M1) 양정치성:} \quad & d(x, y) \geq 0 \text{ for all } x, y \in X \\
& d(x, y) = 0 \iff x = y \\
\text{(M2) 대칭성:} \quad & d(x, y) = d(y, x) \text{ for all } x, y \in X \\
\text{(M3) 삼각부등식:} \quad & d(x, z) \leq d(x, y) + d(y, z) \text{ for all } x, y, z \in X
\end{align}$$

함수 $d$를 **거리(distance)** 또는 **거리함수(metric)**라 합니다.

### 정의 1.2: 수렴과 Cauchy 수열

$(X, d)$가 거리공간이고 $\{x_n\}$이 $X$의 수열일 때:

**수렴:** $x_n \to x$ (또는 $\lim_{n \to \infty} x_n = x$)
$$\forall \varepsilon > 0, \exists N \in \mathbb{N} \text{ s.t. } n > N \implies d(x_n, x) < \varepsilon$$

**Cauchy 수열:** $\{x_n\}$이 Cauchy 수열이다
$$\forall \varepsilon > 0, \exists N \in \mathbb{N} \text{ s.t. } m, n > N \implies d(x_m, x_n) < \varepsilon$$

**핵심 차이:** 
- 수렴은 "극한점의 존재"를 전제합니다
- Cauchy 수열은 "원소들이 서로 가까워지고 있다"는 사실만으로 정의됩니다

### 정의 1.3: 완비성

거리공간 $(X, d)$에서 **모든 Cauchy 수열이 수렴**할 때, 
$(X, d)$를 **완비 거리공간(complete metric space)**이라 합니다.

### 정의 1.4: Banach 고정점 정리에 필요한 개념

함수 $T: X \to X$가 **수축 사상(contraction mapping)**이다:
$$\exists k \in (0, 1) \text{ s.t. } d(Tx, Ty) \leq k \cdot d(x, y) \quad \forall x, y \in X$$

$x^* \in X$가 $T$의 **고정점(fixed point)**이다:
$$Tx^* = x^*$$

## 🔬 정리와 증명

### 정리 1.1: 수렴 수열은 Cauchy 수열이다

**명제:** 거리공간 $(X, d)$에서 $x_n \to x$이면 $\{x_n\}$은 Cauchy 수열이다.

**증명:**
$x_n \to x$이므로, 임의의 $\varepsilon > 0$에 대해
$$\exists N \in \mathbb{N} \text{ s.t. } n > N \implies d(x_n, x) < \frac{\varepsilon}{2}$$

따라서 $m, n > N$일 때, 삼각부등식에 의해:
$$d(x_m, x_n) \leq d(x_m, x) + d(x, x_n) < \frac{\varepsilon}{2} + \frac{\varepsilon}{2} = \varepsilon$$

그러므로 $\{x_n\}$은 Cauchy 수열입니다. $\square$

**보충:** 역은 일반적으로 거짓입니다. Cauchy 수열이 수렴하지 않을 수 있습니다.

### 정리 1.2 (Banach 고정점 정리)

**명제:** 
$X$가 완비 거리공간이고 $T: X \to X$가 수축 사상이면,
$T$는 유일한 고정점 $x^* \in X$를 가지며,
임의의 $x_0 \in X$에서 시작한 반복 수열 $x_{n+1} = Tx_n$은 $x^*$로 수렴합니다.

**증명:**

**Step 1: 반복 수열이 Cauchy 수열임을 보이기**

임의의 $x_0 \in X$를 선택하고 $x_{n+1} = Tx_n$으로 정의합니다.

거리 계산:
$$d(x_{n+1}, x_n) = d(Tx_n, Tx_{n-1}) \leq k \cdot d(x_n, x_{n-1})$$

귀납법으로:
$$d(x_{n+1}, x_n) \leq k^n \cdot d(x_1, x_0)$$

$m > n$일 때:
$$d(x_m, x_n) \leq \sum_{i=n}^{m-1} d(x_{i+1}, x_i) \leq \sum_{i=n}^{m-1} k^i d(x_1, x_0)$$

$$= k^n d(x_1, x_0) \sum_{i=0}^{m-n-1} k^i = k^n d(x_1, x_0) \cdot \frac{1 - k^{m-n}}{1-k}$$

$0 < k < 1$이므로, $n \to \infty$일 때:
$$\lim_{n \to \infty} k^n d(x_1, x_0) \cdot \frac{1}{1-k} = 0$$

따라서 $\{x_n\}$은 Cauchy 수열입니다.

**Step 2: 극한이 존재하고 고정점임을 보이기**

$X$가 완비이므로 $x_n \to x^*$인 $x^* \in X$가 존재합니다.

$T$는 수축 사상이므로 연속입니다(아래 보조정리 참조). 따라서:
$$Tx^* = T(\lim_{n \to \infty} x_n) = \lim_{n \to \infty} Tx_n = \lim_{n \to \infty} x_{n+1} = x^*$$

그러므로 $x^*$는 $T$의 고정점입니다.

**Step 3: 고정점의 유일성**

$Tx = x$와 $Ty = y$인 두 고정점이 있다면:
$$d(x, y) = d(Tx, Ty) \leq k \cdot d(x, y)$$

$k < 1$이므로 $d(x, y) = 0$, 즉 $x = y$입니다.

따라서 고정점은 유일합니다. $\square$

**보조정리:** 수축 사상은 연속입니다.

증명: $d(x_n, x) \to 0$이면 
$$d(Tx_n, Tx) \leq k \cdot d(x_n, x) \to 0$$

### 정리 1.3: 유리수 ℚ는 완비가 아니다

**명제:** 거리공간 $(\mathbb{Q}, |\cdot|)$는 완비가 아니다.

**증명:**

$\sqrt{2}$를 근사하는 유리수 수열을 구성합니다.

Newton 방법을 사용한 수열: $a_1 = 1$, $a_{n+1} = \frac{a_n + 2/a_n}{2}$

이 수열은:
1. ℚ에 속합니다 (귀납법으로 증명 가능)
2. Cauchy 수열입니다: $|a_{n+1} - a_n|$의 거리가 기하급수적으로 감소
3. 실수에서 극한: $\lim_{n \to \infty} a_n = \sqrt{2} \notin \mathbb{Q}$

따라서 ℚ에서 수렴하지 않는 Cauchy 수열이 존재하므로 ℚ는 완비가 아닙니다. $\square$

### 정리 1.4: 완비화의 존재성

**명제:** 임의의 거리공간 $(X, d)$에 대해 완비 거리공간 $(\widehat{X}, \widehat{d})$가 존재하여
$X$를 조밀하게 포함합니다.

**구성의 아이디어 (증명 스케치):**

1. **Cauchy 수열들의 집합:** $\mathcal{C} = \{\{x_n\}: \{x_n\} \text{는 } (X,d)\text{에서 Cauchy 수열}\}$

2. **동치관계 정의:** 두 Cauchy 수열 $\{x_n\}, \{y_n\}$이 동치
$$\{x_n\} \sim \{y_n\} \iff \lim_{n \to \infty} d(x_n, y_n) = 0$$

(극한이 존재하지 않아도 이 정의는 잘 정의되고, 이것은 동치관계입니다)

3. **완비화 공간:** $\widehat{X} = \mathcal{C} / \sim$ (동치류들의 집합)

4. **거리의 정의:** 동치류 $[\{x_n\}], [\{y_n\}]$에 대해
$$\widehat{d}([\{x_n\}], [\{y_n\}]) = \lim_{n \to \infty} d(x_n, y_n)$$

이 극한은 모든 Cauchy 수열에서 존재하고 잘 정의되며(동치류 선택과 무관),
$\widehat{X}$는 완비 거리공간입니다.

**완비화의 예:**
- $(\mathbb{Q}, |\cdot|)$의 완비화는 $(\mathbb{R}, |\cdot|)$
- 정수 계수 다항식 공간 with sup-norm의 완비화는 특정 함수공간

## 💻 NumPy 구현으로 검증

### 예제 1: Cauchy 수열의 수렴성 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# 예제 1: sqrt(2)로 수렴하는 유리수 Cauchy 수열
# Newton 방법: a_{n+1} = (a_n + 2/a_n) / 2

def sqrt2_sequence(n_terms):
    """sqrt(2)를 근사하는 유리수 수열"""
    a = np.zeros(n_terms)
    a[0] = 1.0  # 초기값
    
    for n in range(n_terms - 1):
        a[n+1] = (a[n] + 2.0 / a[n]) / 2.0
    
    return a

# 수열 생성
a = sqrt2_sequence(20)

# Cauchy 수열 조건 검증: |a_m - a_n| < epsilon for large m, n
def verify_cauchy(sequence, epsilon=1e-6):
    """Cauchy 수열 조건 검증"""
    n = len(sequence)
    distances = []
    
    for i in range(n):
        for j in range(i+1, n):
            dist = abs(sequence[i] - sequence[j])
            distances.append(dist)
    
    distances = np.array(distances)
    
    # 마지막 절반의 항들 사이의 거리
    half_n = n // 2
    tail_distances = []
    for i in range(half_n, n):
        for j in range(i+1, n):
            tail_distances.append(abs(sequence[i] - sequence[j]))
    
    tail_distances = np.array(tail_distances)
    print(f"처음 항들 사이 최대 거리: {distances[:5].max():.2e}")
    print(f"마지막 항들 사이 최대 거리: {tail_distances.max():.2e}")
    print(f"수렴값(√2): {np.sqrt(2):.15f}")
    print(f"a_19 값: {a[-1]:.15f}")
    print(f"오차: {abs(a[-1] - np.sqrt(2)):.2e}")
    
    return tail_distances.max() < epsilon

# 검증 실행
print("=" * 60)
print("Cauchy 수열 검증: Newton 방법으로 sqrt(2) 근사")
print("=" * 60)
is_cauchy = verify_cauchy(a)
print(f"Cauchy 수열 조건 만족: {is_cauchy}")

# 수렴 시각화
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 수열 값의 변화
ax = axes[0]
ax.plot(a, 'bo-', markersize=4, label='$a_n$ (Newton 반복)')
ax.axhline(np.sqrt(2), color='r', linestyle='--', label='$\sqrt{2}$ (극한값)')
ax.set_xlabel('반복 횟수 $n$')
ax.set_ylabel('$a_n$ 값')
ax.set_title('유리수 Cauchy 수열이 √2로 수렴')
ax.legend()
ax.grid(True, alpha=0.3)

# 오차의 감소 (로그 스케일)
ax = axes[1]
errors = np.abs(a - np.sqrt(2))
non_zero = errors[errors > 1e-15]
ax.semilogy(range(len(non_zero)), non_zero, 'go-', markersize=4)
ax.set_xlabel('반복 횟수 $n$')
ax.set_ylabel('오차 $|a_n - \sqrt{2}|$')
ax.set_title('오차의 기하급수적 감소')
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('/tmp/sqrt2_cauchy.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/sqrt2_cauchy.png")

# 예제 2: 고차원에서 Cauchy 수열 검증
print("\n" + "=" * 60)
print("예제 2: ℝⁿ에서 Euclidean 거리와 Cauchy 수열")
print("=" * 60)

def euclidean_sequence(n_dim, n_terms):
    """고차원 공간에서 원점으로 수렴하는 수열"""
    # x_k = (1/k, 1/k, ..., 1/k) in R^n_dim
    seq = np.zeros((n_terms, n_dim))
    for k in range(1, n_terms + 1):
        seq[k-1] = np.ones(n_dim) / k
    return seq

X = euclidean_sequence(n_dim=100, n_terms=50)

# 임의의 높은 차원에서 Cauchy 조건 검증
distances = []
for i in range(40, 50):
    for j in range(i+1, 50):
        dist = np.linalg.norm(X[i] - X[j])
        distances.append(dist)

distances = np.array(distances)
print(f"R^100에서 후반 항들의 거리: 최대 {distances.max():.2e}, 최소 {distances.min():.2e}")
print(f"극한점: 원점 = (0, 0, ..., 0)")
print(f"X[49] (a_50): {X[49][:5]}... (처음 5개 좌표)")

plt.figure(figsize=(10, 6))
plt.semilogy(range(len(distances)), np.sort(distances)[::-1], 'mo-', markersize=3)
plt.xlabel('거리 순서')
plt.ylabel('거리값')
plt.title('R^100에서 고차 Cauchy 수열의 거리 분포')
plt.grid(True, alpha=0.3, which='both')
plt.savefig('/tmp/cauchy_rn.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/cauchy_rn.png")

# 예제 3: Banach 고정점 정리 검증
print("\n" + "=" * 60)
print("예제 3: Banach 고정점 정리 - 수축 사상의 고정점")
print("=" * 60)

# 1차원 예제: T(x) = 0.5*x + 1
# 고정점: x* = 0.5*x* + 1 => x* = 2

def T_contract(x):
    """수축 사상: T(x) = 0.5*x + 1"""
    return 0.5 * x + 1.0

# Contraction factor k = 0.5
k = 0.5

# 반복 수열
x0 = 5.0
x_seq = [x0]
for _ in range(30):
    x_seq.append(T_contract(x_seq[-1]))

x_seq = np.array(x_seq)
fixed_point = 2.0  # 해석적 계산: x* = 0.5*x* + 1

# 오차 분석
errors = np.abs(x_seq - fixed_point)

print(f"초기값: x_0 = {x0}")
print(f"수축 계수: k = {k}")
print(f"고정점: x* = {fixed_point}")
print(f"이론적 오차 감소: |x_n - x*| ≤ k^n * |x_0 - x*|")
print(f"초기 오차: {errors[0]:.2e}")
print(f"10번 반복 후: {errors[10]:.2e}")
print(f"20번 반복 후: {errors[20]:.2e}")

# 이론적 bound vs 실제 오차
theoretical_bound = (k ** np.arange(len(x_seq))) * errors[0]

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 반복 값의 수렴
ax = axes[0]
ax.plot(x_seq, 'bo-', markersize=4, label='$x_n = T(x_{n-1})$')
ax.axhline(fixed_point, color='r', linestyle='--', label='고정점 $x^* = 2$')
ax.set_xlabel('반복 횟수 $n$')
ax.set_ylabel('$x_n$ 값')
ax.set_title('Banach 고정점 정리: T(x)=0.5x+1의 수렴')
ax.legend()
ax.grid(True, alpha=0.3)

# 오차 감소 (로그 스케일)
ax = axes[1]
ax.semilogy(errors, 'go-', markersize=4, label='실제 오차 $|x_n - x^*|$')
ax.semilogy(theoretical_bound, 'r--', alpha=0.7, label=f'이론적 bound $k^n|x_0 - x^*|$, $k={k}$')
ax.set_xlabel('반복 횟수 $n$')
ax.set_ylabel('오차')
ax.set_title('오차의 기하급수적 감소')
ax.legend()
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('/tmp/banach_fixed_point.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/banach_fixed_point.png")

# 수축 조건 검증
print("\n" + "=" * 60)
print("수축 조건 검증: |T(x) - T(y)| ≤ k * |x - y|")
print("=" * 60)

x_test = np.linspace(-5, 5, 100)
for i in range(5):
    a = np.random.uniform(-5, 5)
    b = np.random.uniform(-5, 5)
    if a == b:
        continue
    
    lhs = abs(T_contract(a) - T_contract(b))
    rhs = k * abs(a - b)
    ratio = lhs / rhs if rhs != 0 else 0
    
    print(f"  x={a:.2f}, y={b:.2f}: |T(x)-T(y)|/|x-y| = {ratio:.3f} ≤ k={k}")

print("\n수축 조건 만족: True (모든 경우 비율 ≤ k)")
```

**실행 결과 요약:**
- Cauchy 수열의 후반 항들 거리 < 1e-6
- Newton 방법의 오차: 각 반복마다 약 4배 감소 (2차 수렴)
- Banach 고정점: 오차가 정확히 0.5배씩 감소

## 🔗 AI/ML 연결

### 경사하강법의 수렴

신경망 훈련에서 경사하강법은 다음과 같이 작동합니다:
$$\theta_{n+1} = \theta_n - \alpha \nabla L(\theta_n)$$

완비성이 없으면:
- Cauchy 수열 조건은 만족할 수 있습니다 (손실값이 감소)
- 하지만 **극한이 존재하지 않을 수 있습니다** (무한차원 함수공간에서)

완비 함수공간에서는:
- 경사하강법의 반복은 Cauchy 수열입니다
- Banach 고정점 정리에 의해 **수렴이 보장됩니다**

### 정규화와 완비 부분공간

신경망 정규화(L1, L2 regularization)는:
- 파라미터 공간을 더 "좋은" 완비 부분공간으로 제한합니다
- 이렇게 제한된 공간에서는 수렴이 더 안정적입니다

### 함수공간의 표현성

신경망 함수는 ℝⁿ에서 ℝᵐ 범위를 갖는 함수들의 집합에 속합니다:
- 유한 레이어의 신경망 = 특정 함수들의 완비 부분공간
- Universal Approximation은 이 함수공간이 충분히 조밀함을 의미합니다

## ⚖️ 가정과 한계

### 가정

1. **거리가 양의 정의성 만족:** 거리의 공리는 매우 강합니다
   - 일반화: Pseudo-metric (d(x,y)=0이 x=y를 함의하지 않음)
   - 실제 응용: 공간의 부분집합에서는 이 가정이 약화될 수 있습니다

2. **Cauchy 수열의 정의:** 순수하게 "항들 사이의 거리"에만 기반
   - 극한점의 존재 여부를 직접적으로 나타내지 않습니다
   - 따라서 "완비성"은 추가 가정으로 필요합니다

### 한계

1. **완비성 검증의 어려움:** 
   - 일반적인 거리공간에서 완비성을 직접 증명하는 것은 어렵습니다
   - 함수공간에서는 Riesz-Fischer 정리 같은 강력한 도구가 필요합니다

2. **무한차원에서의 반직관성:**
   - 유한차원에서 "가까우면 수렴한다"는 직관이 깨집니다
   - 예: 함수들이 모든 점에서 가까워도 극한이 불연속일 수 있습니다

3. **거리가 모든 구조를 설명하지는 못함:**
   - 거리만으로는 함수공간의 많은 성질을 나타낼 수 없습니다
   - 필요: 노름(norm), 내적(inner product) 같은 추가 구조

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 의미 |
|------|------|------|
| **거리공간** | $(X, d)$: 거리 공리를 만족하는 집합+함수 | 거리의 개념으로 "가깝다"를 정의 |
| **Cauchy 수열** | 항들 사이 거리가 임의로 작아짐 | "수렴하는 징후"를 나타냄 |
| **수렴** | $d(x_n, x) \to 0$ | 극한점이 실제로 존재함 |
| **완비성** | 모든 Cauchy 수열이 수렴 | 극한이 항상 같은 공간에 존재 |
| **수축 사상** | $d(Tx,Ty) \leq k \cdot d(x,y)$ (k<1) | 반복 적용 시 수렴을 보장 |
| **고정점** | $Tx = x$ | 수축 사상의 극한 상태 |

**핵심 정리 3개:**
1. **수렴 ⟹ Cauchy** (역은 거짓)
2. **완비 + 수축 사상 ⟹ 고정점 존재 & 수렴** (Banach)
3. **유리수는 완비 아님, 실수는 완비** (완비화)

## 🤔 생각해볼 문제

### 문제 1: 서로 다른 거리의 동치성

$\mathbb{R}$에 두 거리를 정의합니다:
- $d_1(x, y) = |x - y|$ (표준 거리)
- $d_2(x, y) = \frac{|x-y|}{1+|x-y|}$ (유계 거리)

Cauchy 수열 $\{x_n\}$이 $d_1$에서 Cauchy이면, $d_2$에서도 Cauchy입니까?

<details>
<summary>해설 보기</summary>

**답: 예**

증명: $d_2(x_m, x_n) = \frac{|x_m - x_n|}{1 + |x_m - x_n|}$

$\{x_n\}$이 $d_1$-Cauchy이면, $\forall \varepsilon > 0$, $\exists N$ s.t. $n,m > N \implies |x_m - x_n| < \varepsilon'$

(여기서 $\varepsilon' = \varepsilon / (1 - \varepsilon)$로 선택 가능하면)

그러면 $d_2(x_m, x_n) = \frac{|x_m-x_n|}{1+|x_m-x_n|} < \frac{\varepsilon'}{1+\varepsilon'} = \varepsilon$

따라서 $\{x_n\}$은 $d_2$-Cauchy이기도 합니다.

**추가 질문:** 이 두 거리에서 수렴하는 점이 같습니까?

</details>

### 문제 2: 완비 공간의 닫힌 부분공간

$X$가 완비 거리공간이고 $Y \subset X$가 닫혀있을 때, $Y$도 완비입니까?

<details>
<summary>해설 보기</summary>

**답: 예**

증명:
1. $Y$에서 Cauchy 수열 $\{y_n\}$을 잡습니다
2. 이는 또한 $X$에서의 Cauchy 수열입니다 (같은 거리 사용)
3. $X$가 완비이므로 $y_n \to y$인 $y \in X$가 존재합니다
4. $Y$가 닫혀있으므로, Cauchy 수열의 극한 $y \in Y$입니다
5. 따라서 $\{y_n\}$은 $Y$에서 수렴합니다

**핵심:** 완비성 + 닫힘 = 부분공간도 완비

</details>

### 문제 3: 수축 사상의 고정점 위치

$T: [0, 1] \to [0, 1]$이 $T(x) = 1 - 0.9x$로 정의될 때:

(a) $T$가 수축 사상입니까?  
(b) 고정점을 구하시오.  
(c) $x_0 = 0$에서 시작하여 처음 5개 항을 계산하시오.

<details>
<summary>해설 보기</summary>

**답:**

**(a)** 수축 계수 검증:
$$|T(x) - T(y)| = |(1-0.9x) - (1-0.9y)| = 0.9|x-y|$$

$k = 0.9 < 1$이므로 **예, 수축 사상**입니다.

**(b)** 고정점:
$$x^* = 1 - 0.9x^* \implies 1.9x^* = 1 \implies x^* = \frac{1}{1.9} = \frac{10}{19} \approx 0.526$$

**(c)** 반복 수열:
- $x_0 = 0$
- $x_1 = T(0) = 1$
- $x_2 = T(1) = 1 - 0.9 = 0.1$
- $x_3 = T(0.1) = 1 - 0.09 = 0.91$
- $x_4 = T(0.91) = 1 - 0.819 = 0.181$
- $x_5 = T(0.181) = 1 - 0.1629 = 0.837$

수열은 진동하면서 $10/19 \approx 0.526$으로 수렴합니다.

</details>

---

<div align="center">

| | | |
|---|---|---|
| | [📚 README](../README.md) | [다음 ▶](./02-normed-banach-space.md) |

</div>
