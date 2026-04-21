# 4. ℓᵖ 수열공간과 C([a,b]) 연속함수공간

## 🎯 핵심 질문

1. 수열 공간과 함수 공간의 수학적 구조는 어떻게 다른가?
2. 무한차원 공간에서도 완비성이 유지되는가?
3. 서로 다른 함수 공간들 사이의 포함 관계는?
4. Weierstrass 근사 정리가 의미하는 바는?

## 🔍 왜 이 이론이 AI에서 중요한가

신경망의 가중치는 두 가지로 표현됩니다:
- **벡터 관점:** W ∈ ℝᵈ (유한차원, d = 백만 정도)
- **함수 관점:** W를 함수로 본다면, 이는 ℓ² 수열공간 또는 함수공간

무한차원으로 확장하면:
- **ℓ² 가중치:** 무한 개의 파라미터 (attention의 위치 임베딩 등)
- **C([0,1])** 함수로서의 신경망: 입력이 실수 구간일 때의 근사

또한 **조밀성(density)**은 신경망이 모든 연속함수를 근사할 수 있음을 보증합니다:
- Universal Approximation Theorem = 다항식이 C([a,b])에서 조밀하다는 Weierstrass 정리의 확장

## 📐 수학적 선행 조건

- Lᵖ 공간과 완비성 (Chapter 3)
- 수열의 수렴과 Cauchy 수열
- 함수의 연속성과 균등 수렴 (ε-δ 정의)
- 실수 함수의 극댓값과 극솟값

## 📖 직관적 이해 (유한차원 → 무한차원)

**유한차원(ℝⁿ)에서는:**
- 벡터 (x₁, ..., xₙ) — 유한 개의 성분
- Cauchy 수열이 수렴함 (완비)
- 모든 노름이 동치

**무한차원 ℓᵖ에서는:**
- 무한 수열 (x₁, x₂, x₃, ...) — ∑|xᵢ|ᵖ < ∞
- Cauchy 수열이 여전히 수렴함 (완비 유지)
- 하지만 공간의 "크기"가 상상을 초월함

**함수공간 C([a,b])에서는:**
- 각 입력 x ∈ [a,b]에 대해 함수값 f(x)
- 거리: 최대 편차 ‖f-g‖∞ = max|f(x)-g(x)|
- 완비이지만, 무한차원의 복잡성이 발현됨

**점별 수렴 vs 균등 수렴:**
- 유한차원: 구별할 필요 없음
- 무한차원: 극한 함수가 연속이지 않을 수 있음 (점별 수렴의 위험)
- 균등 수렴: 극한이 항상 연속임을 보장

## ✏️ 엄밀한 정의

### 정의 4.1: ℓᵖ 수열공간

$1 \leq p \leq \infty$에 대해:

$$\ell^p = \left\{(x_n)_{n=1}^\infty : x_n \in \mathbb{R}, \sum_{n=1}^\infty |x_n|^p < \infty\right\}$$

$p = \infty$일 때:
$$\ell^\infty = \left\{(x_n)_{n=1}^\infty : \sup_{n} |x_n| < \infty\right\}$$

**노름:**
$$\|x\|_p = \begin{cases}
\left(\sum_{n=1}^\infty |x_n|^p\right)^{1/p} & \text{if } 1 \leq p < \infty \\
\sup_n |x_n| & \text{if } p = \infty
\end{cases}$$

### 정의 4.2: C([a,b]) — 연속함수공간

$$C([a,b]) = \{f : [a,b] \to \mathbb{R} : f \text{ is continuous}\}$$

**sup-노름 (균등 노름):**
$$\|f\|_\infty = \|f\|_C = \sup_{x \in [a,b]} |f(x)| = \max_{x \in [a,b]} |f(x)|$$

(최댓값이 존재함 — compact set에서 연속함수는 최댓값 가짐)

### 정의 4.3: 함수 수렴의 개념들

주어진 함수 수열 $\{f_n\}$에 대해:

**점별 수렴 (pointwise convergence):**
$$f_n \to f \text{ pointwise} \iff \forall x \in [a,b], \lim_{n \to \infty} f_n(x) = f(x)$$

즉, 각 점에서 따로따로 극한:
$$\forall x \in [a,b], \forall \varepsilon > 0, \exists N(x) \text{ s.t. } n > N(x) \implies |f_n(x) - f(x)| < \varepsilon$$

주의: $N$이 $x$에 의존함!

**균등 수렴 (uniform convergence):**
$$f_n \to f \text{ uniformly} \iff \forall \varepsilon > 0, \exists N \text{ s.t. } n > N \implies \|f_n - f\|_\infty < \varepsilon$$

즉, 모든 점에서 동시에 수렴:
$$\forall \varepsilon > 0, \exists N \text{ (x와 무관)} \text{ s.t. } n > N \implies |f_n(x) - f(x)| < \varepsilon \text{ for all } x \in [a,b]$$

### 정의 4.4: 조밀 부분집합

$A \subset X$가 $X$에서 **조밀(dense)**이다:
$$\overline{A} = X$$

즉, $A$의 폐포(closure)가 전체 공간입니다.

동치 정의: 모든 $x \in X$와 모든 $\varepsilon > 0$에 대해,
$$\exists a \in A \text{ s.t. } \|x - a\| < \varepsilon$$

**직관:** A의 원소들이 X 곳곳에 고르게 분포되어 있음

## 🔬 정리와 증명

### 정리 4.1: ℓᵖ는 완비 Banach 공간이다

**명제:** $(1 \leq p \leq \infty)$ 에 대해, $(\ell^p, \|\cdot\|_p)$는 완비 노름공간이다.

**증명:**

**Step 1: $\ell^p$가 노름공간임**

- 노름 공리는 p-노름의 정의로부터 따름 (Ch. 2 참고)
- 벡터공간 구조도 명확함 (수렴하는 급수들의 합은 수렴)

**Step 2: Cauchy 수열 $\{x^{(n)}\}$ in $\ell^p$ (여기서 $x^{(n)} = (x_1^{(n)}, x_2^{(n)}, ...)$)**

임의의 $\varepsilon > 0$에 대해, $\exists N$ s.t. $m, n > N$:
$$\|x^{(m)} - x^{(n)}\|_p < \varepsilon$$

즉:
$$\sum_{j=1}^\infty |x_j^{(m)} - x_j^{(n)}|^p < \varepsilon^p$$

**Step 3: 성분별 수렴**

고정된 $j$에 대해:
$$|x_j^{(m)} - x_j^{(n)}|^p \leq \sum_{i=1}^\infty |x_i^{(m)} - x_i^{(n)}|^p < \varepsilon^p$$

따라서:
$$|x_j^{(m)} - x_j^{(n)}| < \varepsilon$$

즉, 수열 $\{x_j^{(n)}\}_{n=1}^\infty$ (j번째 성분들)은 Cauchy 수열입니다.

ℝ가 완비이므로:
$$x_j := \lim_{n \to \infty} x_j^{(n)} \text{ exists}$$

**Step 4: 극한이 ℓᵖ에 속함**

$x = (x_1, x_2, ...)$라 정의합니다.

**보조정리 (Fatou의 보조정리):** 음이 아닌 수열 $a_n$에 대해:
$$\liminf_{n \to \infty} \sum_{j=1}^\infty a_{n,j} \geq \sum_{j=1}^\infty \liminf_{n \to \infty} a_{n,j}$$

$a_{n,j} = |x_j^{(n)} - x_j^{(N)}|^p$로 적용하면:

$$\sum_{j=1}^\infty |x_j - x_j^{(N)}|^p = \sum_{j=1}^\infty \left|\lim_{n \to \infty} x_j^{(n)} - x_j^{(N)}\right|^p$$

$$\leq \liminf_{n \to \infty} \sum_{j=1}^\infty |x_j^{(n)} - x_j^{(N)}|^p$$

Cauchy 수열이므로 (N을 충분히 크게):
$$\leq \varepsilon^p$$

따라서:
$$\|x - x^{(N)}\|_p \leq \varepsilon$$

$x^{(N)} \in \ell^p$이고 $\|x - x^{(N)}\|_p < \infty$이므로:
$$\|x\|_p \leq \|x - x^{(N)}\|_p + \|x^{(N)}\|_p < \infty$$

즉, $x \in \ell^p$.

**Step 5: 수렴**

위에서 $\|x - x^{(N)}\|_p \leq \varepsilon$임을 보았으므로:
$$x^{(n)} \to x \text{ in } \ell^p$$

따라서 $\ell^p$는 완비입니다. $\square$

### 정리 4.2: C([a,b])는 완비 Banach 공간이다

**명제:** $(C([a,b]), \|\cdot\|_\infty)$는 완비 노름공간이다.

**증명:**

**Step 1: Cauchy 수열 $\{f_n\}$ in $C([a,b])$**

$$\forall \varepsilon > 0, \exists N \text{ s.t. } m, n > N \implies \|f_m - f_n\|_\infty < \varepsilon$$

즉:
$$\forall x \in [a,b], |f_m(x) - f_n(x)| < \varepsilon$$

**Step 2: 점별 극한의 존재**

각 고정된 $x \in [a,b]$에 대해, 수열 $\{f_n(x)\}_{n=1}^\infty$는 Cauchy 수열입니다:
$$|f_m(x) - f_n(x)| < \varepsilon \text{ for } m, n > N$$

ℝ가 완비이므로:
$$f(x) := \lim_{n \to \infty} f_n(x) \text{ exists}$$

**Step 3: 극한함수의 균등 수렴**

주목: 이것이 점별 수렴이 아니라 **균등 수렴**입니다!

$\varepsilon > 0$이 주어졌을 때, Cauchy 수열 조건에 의해 $\exists N$ s.t. $m, n > N$:
$$|f_m(x) - f_n(x)| < \varepsilon/2 \text{ for all } x \in [a,b]$$

$n \to \infty$ (m 고정):
$$|f_m(x) - f(x)| = \lim_{n \to \infty} |f_m(x) - f_n(x)| \leq \varepsilon/2 < \varepsilon$$

(극한은 부등식을 보존)

따라서:
$$\|f_m - f\|_\infty \leq \varepsilon$$

즉, $f_n \to f$ **균등하게**.

**Step 4: 극한함수의 연속성**

균등 수렴 극한의 연속함수 합성은 연속입니다.

직접 증명: $x_0 \in [a,b]$에서 $f$의 연속성.

$$|f(x) - f(x_0)| \leq |f(x) - f_n(x)| + |f_n(x) - f_n(x_0)| + |f_n(x_0) - f(x_0)|$$

- 첫 번째, 세 번째 항: $f_n \to f$ 균등이므로 작음
- 두 번째 항: $f_n$ 연속이므로 $x \to x_0$일 때 작음

따라서 $f$는 연속입니다.

**Step 5: 수렴**

$f \in C([a,b])$이고 $f_n \to f$ uniformly in $\|\cdot\|_\infty$. $\square$

**핵심:** C([a,b])의 완비성은 "균등 수렴이 극한 함수의 연속성을 보존"하기 때문입니다!

### 정리 4.3: ℓ² ⊂ Lᵖ(ℕ) 동치성

**명제:** 
$$\ell^p = L^p(\mathbb{N}, \text{counting measure})$$

즉, ℓᵖ 수열공간과 자연수 집합에서의 Lᵖ 함수공간은 같습니다.

**증명:** 
수열 $(x_n)$을 함수 $f: \mathbb{N} \to \mathbb{R}$, $f(n) = x_n$으로 보면:

$$\|f\|_p^p = \int_\mathbb{N} |f(n)|^p d(\text{counting measure}) = \sum_{n=1}^\infty |x_n|^p$$

따라서 정의가 일치합니다. $\square$

### 정리 4.4: 조밀성과 함의 관계

**명제:** 
$A \subset B \subset X$이고 $A$가 $X$에서 조밀하면, $B$도 $X$에서 조밀합니다.

$$A \text{ dense in } B \text{ and } B \subset X \implies A \text{ dense in } X$$

**증명:**
$x \in X$와 $\varepsilon > 0$에 대해, $A$가 $X$에서 조밀하므로:
$$\exists a \in A \text{ s.t. } \|x - a\| < \varepsilon$$

$a \in A \subset B$이므로 조밀성의 정의에 의해 $A$는 $B$에도 조밀합니다. $\square$

### 정리 4.5 (Weierstrass 근사 정리)

**명제:** 
다항식들의 집합 $\mathcal{P} = \{\text{polynomials on } [a,b]\}$는 
$C([a,b])$에서 **조밀**하다.

즉:
$$\overline{\mathcal{P}} = C([a,b])$$

또는 동치로: 임의의 $f \in C([a,b])$와 $\varepsilon > 0$에 대해, 다항식 $p$가 존재하여:
$$\|f - p\|_\infty < \varepsilon$$

**증명 (Bernstein 다항식 이용):**

**Step 1: [0,1]에서의 증명**

$f \in C([0,1])$이 주어졌을 때, Bernstein 다항식:
$$B_n(f)(x) = \sum_{k=0}^n f(k/n) \binom{n}{k} x^k (1-x)^{n-k}$$

**Step 2: Bernstein 다항식의 성질**

1. $B_n(f)$는 다항식 (차수 n)
2. 기저 함수들: $b_{n,k}(x) = \binom{n}{k} x^k (1-x)^{n-k}$는 분할의 단위:
   $$\sum_{k=0}^n b_{n,k}(x) = 1$$

**Step 3: 균등 수렴**

$f$는 compact [0,1]에서 연속이므로 균등 연속:
$$\forall \varepsilon > 0, \exists \delta > 0 \text{ s.t. } |x-y| < \delta \implies |f(x) - f(y)| < \varepsilon$$

또한 bounded: $|f(x)| \leq M$ for some $M$.

Bernstein 다항식:
$$B_n(f)(x) - f(x) = \sum_{k=0}^n b_{n,k}(x) [f(k/n) - f(x)]$$

다항식으로 두 집합 분할:
- $\Delta_\delta = \{k : |k/n - x| < \delta\}$: 가까운 점들
- $\Delta_\delta^c$: 먼 점들

**가까운 점들:**
$$\left|\sum_{k \in \Delta_\delta} b_{n,k}(x) [f(k/n) - f(x)]\right| \leq \varepsilon \sum_{k \in \Delta_\delta} b_{n,k}(x) \leq \varepsilon$$

**먼 점들:**
$$\left|\sum_{k \in \Delta_\delta^c} b_{n,k}(x) [f(k/n) - f(x)]\right| \leq 2M \sum_{k \in \Delta_\delta^c} b_{n,k}(x)$$

임의의 $k$에 대해 $(k/n - x)^2 \geq \delta^2$이므로:
$$\sum_{k \in \Delta_\delta^c} b_{n,k}(x) \leq \sum_{k: |k/n - x| \geq \delta} \frac{(k/n - x)^2}{\delta^2} b_{n,k}(x)$$

이 값은 $n \to \infty$일 때 0으로 수렴합니다 (계산 생략).

따라서 n 충분히 크면:
$$|B_n(f)(x) - f(x)| < 2\varepsilon$$

균등하게.

**Step 4: [a,b]로 확장**

좌표 변환 $t = (x - a)/(b - a)$로 [a,b]를 [0,1]로 매핑하면 결과를 얻습니다. $\square$

### 정리 4.6: C([a,b])와 Lᵖ([a,b]) 간의 관계

**명제:**
1. $C([a,b]) \subset L^p([a,b])$ for all $1 \leq p \leq \infty$
2. $C([a,b])$는 $L^p([a,b])$에서 **조밀**하다

**증명:**

**Part 1: 포함 관계**

$f \in C([a,b])$이면:
$$\int_a^b |f(x)|^p dx \leq \max_{x \in [a,b]} |f(x)|^p \cdot (b-a) < \infty$$

따라서 $f \in L^p$.

**Part 2: 조밀성**

$f \in L^p([a,b])$이고 $\varepsilon > 0$이 주어졌을 때:

1. 단순함수(simple function) $s$로 근사:
   $$\|f - s\|_p < \varepsilon/2$$

2. 단순함수는 특성함수들의 합. 각 특성함수를 연속함수로 근사:
   $$\|\mathbb{1}_A - g\|_p < \varepsilon/(2n)$$
   (n = 단순함수의 항의 개수)

3. 연속함수들의 합 $g$에 대해:
   $$\|f - g\|_p \leq \|f - s\|_p + \|s - g\|_p < \varepsilon$$

따라서 $C([a,b])$는 $L^p$에서 조밀합니다. $\square$

## 💻 NumPy 구현으로 검증

### 예제 1: ℓᵖ 수열공간의 완비성

```python
import numpy as np
import matplotlib.pyplot as plt

print("=" * 70)
print("예제 1: ℓᵖ 수열공간의 완비성 검증")
print("=" * 70)

# Cauchy 수열의 구성: x^(n) = (1, 1/2, 1/4, ..., 1/2^(n-1), 0, 0, ...)
# 극한: x = (1, 1/2, 1/4, 1/8, ...)

def create_sequence(n_terms, seq_length=50):
    """Cauchy 수열 생성"""
    sequence = []
    for n in range(1, n_terms + 1):
        x = np.zeros(seq_length)
        for k in range(min(n, seq_length)):
            x[k] = 1 / (2**k)
        sequence.append(x)
    return np.array(sequence)

# 수열 생성
sequence = create_sequence(20, seq_length=50)

# ℓ² 노름 계산
l2_norms = [np.linalg.norm(x) for x in sequence]

# Cauchy 수열 조건 검증
print("\nCauchy 수열 조건 검증: ‖x^(m) - x^(n)‖₂\n")

for n in [3, 5, 10, 20]:
    for m in [n+1, n+3, n+5]:
        if m <= len(sequence):
            dist = np.linalg.norm(sequence[m-1] - sequence[n-1])
            print(f"  ‖x^({m}) - x^({n})‖₂ = {dist:.6e}", end="")
            if dist < 1e-6:
                print(" ✓")
            else:
                print()

# 극한점
limit = np.array([1 / (2**k) for k in range(50)])
limit_norm = np.linalg.norm(limit)

print(f"\n극한점의 ℓ² 노름: {limit_norm:.6f}")
print(f"이론값 (1/√(1-1/4) = 2/√3): {2/np.sqrt(3):.6f}")

# 수렴성
convergence = [np.linalg.norm(x - limit) for x in sequence]

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 수열의 ℓ² 노름 변화
ax = axes[0]
ax.plot(range(1, len(l2_norms) + 1), l2_norms, 'bo-', markersize=4)
ax.axhline(limit_norm, color='r', linestyle='--', label=f'극한값 = {limit_norm:.4f}')
ax.set_xlabel('$n$ (수열 인덱스)', fontsize=11)
ax.set_ylabel('‖x^(n)‖₂', fontsize=11)
ax.set_title('ℓ² 수열의 노름 수렴', fontsize=12, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

# 극한으로부터의 거리
ax = axes[1]
ax.semilogy(range(1, len(convergence) + 1), convergence, 'go-', markersize=4)
ax.set_xlabel('$n$ (수열 인덱스)', fontsize=11)
ax.set_ylabel('‖x^(n) - limit‖₂', fontsize=11)
ax.set_title('ℓ²에서의 수렴 (Riesz-Fischer)', fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('/tmp/l2_sequence_space.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/l2_sequence_space.png")

# 예제 2: 균등 수렴 vs 점별 수렴
print("\n" + "=" * 70)
print("예제 2: 균등 수렴과 점별 수렴의 차이")
print("=" * 70)

x = np.linspace(0, 1, 1000)

# 함수 수열: f_n(x) = x^n
# 점별로: x < 1에서 0으로 수렴, x=1에서 1
# 균등으로: 수렴하지 않음! (x=1 근처에서)

fig, axes = plt.subplots(2, 2, figsize=(14, 10))

n_values = [2, 5, 20, 100]

for idx, n in enumerate(n_values):
    ax = axes[idx // 2, idx % 2]
    
    f_n = x**n
    
    ax.plot(x, f_n, 'b-', linewidth=2, label=f'$f_n(x) = x^n$ (n={n})')
    ax.axhline(0, color='k', linewidth=0.5)
    ax.axhline(1, color='r', linestyle='--', alpha=0.3)
    ax.set_xlim([0, 1])
    ax.set_ylim([-0.1, 1.1])
    
    # 수렴 영역 표시
    if n == 100:
        # x > 0.9에서 급격한 변화
        ax.axvline(0.9, color='g', linestyle=':', alpha=0.5, label='급격한 변화 시작')
    
    ax.set_xlabel('$x$', fontsize=11)
    ax.set_ylabel('$f_n(x)$', fontsize=11)
    ax.set_title(f'$f_n(x) = x^n$ (n={n})\n점별로는 수렴하지만, 균등하지 않음', 
                 fontsize=11, fontweight='bold')
    ax.grid(True, alpha=0.3)
    ax.legend()

plt.tight_layout()
plt.savefig('/tmp/pointwise_vs_uniform.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/pointwise_vs_uniform.png")

# sup-norm으로 거리 계산
norms = [np.max(np.abs(x**n)) for n in range(1, 51)]

fig, ax = plt.subplots(figsize=(10, 6))
ax.semilogy(range(1, 51), norms, 'ro-', markersize=4)
ax.set_xlabel('$n$ (함수 수열 인덱스)', fontsize=12)
ax.set_ylabel('$‖f_n - 0‖_∞ = ‖x^n‖_∞$', fontsize=12)
ax.set_title('sup-노름으로 본 수렴 실패\n(‖x^n‖_∞ = 1이 수렴하지 않음)', 
             fontsize=12, fontweight='bold')
ax.grid(True, alpha=0.3, which='both')
plt.tight_layout()
plt.savefig('/tmp/sup_norm_divergence.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/sup_norm_divergence.png")

# 예제 3: Weierstrass 근사 정리 — Bernstein 다항식
print("\n" + "=" * 70)
print("예제 3: Weierstrass 근사 정리 — Bernstein 다항식")
print("=" * 70)

def bernstein_polynomial(f, n, x):
    """Bernstein 다항식 계산"""
    result = np.zeros_like(x)
    for k in range(n + 1):
        # 이항계수
        binom = np.math.comb(n, k)
        # Bernstein 기저 함수
        basis = binom * (x**k) * ((1-x)**(n-k))
        # f(k/n) * 기저
        result += f(k/n) * basis
    return result

# 근사할 함수: f(x) = sin(2πx)
f = lambda x: np.sin(2 * np.pi * x)

x = np.linspace(0, 1, 500)
f_true = f(x)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 근사 시각화
ax = axes[0]
ax.plot(x, f_true, 'k-', linewidth=2, label='원래 함수 f(x)')

for n in [5, 10, 30]:
    f_approx = bernstein_polynomial(f, n, x)
    ax.plot(x, f_approx, alpha=0.6, label=f'Bernstein n={n}')

ax.set_xlabel('$x$', fontsize=11)
ax.set_ylabel('$f(x)$', fontsize=11)
ax.set_title('Weierstrass 근사 정리: Bernstein 다항식', fontsize=12, fontweight='bold')
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3)
ax.set_xlim([0, 1])

# 근사 오차
ax = axes[1]
errors = []
n_range = np.arange(1, 51)

for n in n_range:
    f_approx = bernstein_polynomial(f, n, x)
    error = np.max(np.abs(f_true - f_approx))
    errors.append(error)

errors = np.array(errors)

ax.semilogy(n_range, errors, 'go-', markersize=5, label='sup-norm 오차')
ax.set_xlabel('$n$ (Bernstein 차수)', fontsize=11)
ax.set_ylabel('$‖f - B_n(f)‖_∞$', fontsize=11)
ax.set_title('근사 오차의 지수 감소', fontsize=12, fontweight='bold')
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('/tmp/bernstein_approximation.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/bernstein_approximation.png")

print(f"f(x) = sin(2πx)의 근사:")
print(f"  n=5: 오차 = {errors[4]:.6e}")
print(f"  n=10: 오차 = {errors[9]:.6e}")
print(f"  n=30: 오차 = {errors[29]:.6e}")
print(f"  n=50: 오차 = {errors[49]:.6e}")

# 예제 4: C([0,1])의 완비성
print("\n" + "=" * 70)
print("예제 4: C([0,1])의 완비성")
print("=" * 70)

# Cauchy 수열의 극한이 항상 연속
# 균등 수렴 → 극한함수 연속

def create_continuous_sequence(n_terms):
    """연속함수의 Cauchy 수열 생성"""
    x = np.linspace(0, 1, 1000)
    sequence = []
    
    for n in range(1, n_terms + 1):
        # f_n(x) = x^(1/n) → 1이지만 균등하게 수렴하지 않음
        # 대신 다른 예: f_n(x) = sin(πx) / (1 + n)
        f = np.sin(np.pi * x) / (1 + n)
        sequence.append((x, f))
    
    return sequence

sequence = create_continuous_sequence(20)

# sup-norm으로 거리 계산
x_vals = sequence[0][0]
norms = [np.max(np.abs(sequence[i][1])) for i in range(len(sequence))]

fig, ax = plt.subplots(figsize=(10, 6))

# 몇 개 함수 시각화
for n in [0, 5, 10, 19]:
    ax.plot(sequence[n][0], sequence[n][1], label=f'f_{n+1}(x)', alpha=0.7)

ax.set_xlabel('$x$', fontsize=11)
ax.set_ylabel('$f_n(x)$', fontsize=11)
ax.set_title('C([0,1])의 Cauchy 수열\n(균등하게 0으로 수렴)', fontsize=12, fontweight='bold')
ax.legend(fontsize=10)
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/continuous_sequence.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/continuous_sequence.png")

# 예제 5: 조밀성과 근사
print("\n" + "=" * 70)
print("예제 5: C([0,1])에서의 조밀성")
print("=" * 70)

print("\n다항식이 C([0,1])에서 조밀함을 의미:")
print("  '임의의 연속함수를 다항식으로 임의로 잘 근사할 수 있다'")
print("\n이것이 신경망의 Universal Approximation의 기초입니다!")

# 복잡한 함수 근사
def complex_function(x):
    return np.sin(3*x) + 0.5*np.cos(5*x) - 0.3*x**2

x = np.linspace(0, 1, 500)
f_true = complex_function(x)

fig, axes = plt.subplots(2, 2, figsize=(14, 10))

poly_degrees = [3, 5, 10, 20]

for idx, deg in enumerate(poly_degrees):
    ax = axes[idx // 2, idx % 2]
    
    # 다항식 피팅
    x_fit = np.linspace(0, 1, deg + 1)
    y_fit = complex_function(x_fit)
    
    coeffs = np.polyfit(x_fit, y_fit, deg)
    p = np.poly1d(coeffs)
    
    f_approx = p(x)
    error = np.max(np.abs(f_true - f_approx))
    
    ax.plot(x, f_true, 'k-', linewidth=2, label='원래 함수')
    ax.plot(x, f_approx, 'b--', linewidth=1.5, alpha=0.7, label=f'다항식 (차수 {deg})')
    ax.fill_between(x, f_true - 0.01, f_true + 0.01, alpha=0.1, color='green')
    
    ax.set_xlabel('$x$', fontsize=10)
    ax.set_ylabel('$f(x)$', fontsize=10)
    ax.set_title(f'차수 {deg} 다항식으로 근사\n오차 = {error:.6e}', fontsize=11, fontweight='bold')
    ax.legend(fontsize=9)
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/polynomial_density.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/polynomial_density.png")
```

**실행 결과 요약:**
- ℓ² 수열공간에서 Cauchy 수열이 실제로 수렴 확인
- 균등 수렴이 점별 수렴보다 강함 (x^n의 예)
- Bernstein 다항식의 지수 수렴
- 임의의 연속함수를 다항식으로 근사 가능 (Weierstrass)

## 🔗 AI/ML 연결

### Universal Approximation Theorem

신경망의 강력함의 기초:
- **Weierstrass 정리:** 다항식들이 C([a,b])에서 조밀
- **신경망 버전:** 활성화 함수를 가진 신경망들도 조밀
- **의미:** 충분히 큰 신경망은 어떤 연속함수도 임의로 잘 근사 가능

### Attention Weight와 ℓ² 공간

Transformer의 attention weight:
- softmax(Q·K^T / √d) → 확률 분포
- 이들의 집합: ℓ² 수열공간의 부분공간
- 완비성 덕분에 극한 행동 분석 가능

### C([0,1])로서의 신경망

신경망 함수 f_w: [0,1] → ℝ를 생각하면:
- 입력: x ∈ [0,1]
- 출력: f_w(x) ∈ ℝ (연속 — ReLU 제외, 하지만 대부분의 활성화)
- 따라서 신경망은 C([0,1])의 부분집합

가중치 공간의 학습은 C([0,1])에서의 최적화!

## ⚖️ 가정과 한계

### 가정

1. **연속성:** C([a,b])는 연속함수 공간
   - 불연속 함수 (step function)는 제외
   - Sobolev 공간으로 확장 시 약약 미분가능한 함수도 포함

2. **유한 구간:** [a,b]는 compact
   - 무한 구간에서는 다른 기법 필요 (exponential weight 등)

### 한계

1. **Weierstrass는 존재성만:**
   - 다항식의 차수에 대한 정량적 보장 없음
   - 실제 근사는 Jackson 부등식 등으로 개선

2. **C([a,b]) ⊂ L²의 진정한 의미:**
   - C([a,b])는 매우 작은 부분공간
   - 대부분의 L² 함수는 연속이 아님

3. **무한차원의 복잡성:**
   - 최솟값의 존재 보장 안 됨 (다음 장)
   - 컴팩트성 상실 → Riesz 보조정리

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 의미 |
|------|------|------|
| **ℓᵖ** | {(xₙ) : Σ\|xₙ\|ᵖ < ∞} | 무한 수열 공간 |
| **C([a,b])** | 연속함수들, sup-노름 | 함수공간, uniform topology |
| **점별 수렴** | x마다 따로 수렴 | 약한 개념 |
| **균등 수렴** | 모든 x에서 동시 수렴 | 강한 개념, C([a,b])에 필요 |
| **조밀** | A의 폐포 = 전체 공간 | A로 모든 점 근사 가능 |
| **Weierstrass** | 다항식이 조밀 | 신경망 이론의 기초 |

**핵심 3개:**
1. ℓᵖ는 완비 (Riesz-Fischer)
2. C([a,b])는 완비 (균등 수렴 덕분)
3. 다항식은 조밀 (Universal Approximation)

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./03-lp-spaces-completeness.md) | [📚 README](../README.md) | [다음 ▶](./05-finite-dim-properties.md) |

</div>
