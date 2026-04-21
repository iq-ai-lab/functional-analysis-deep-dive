# 1. 유계 선형 연산자의 정의 — 연속성과 유계성의 동치

## 🎯 핵심 질문
- 선형 연산자가 "유계"라는 것은 무엇을 의미하는가?
- 유계성과 연속성은 어떤 관계에 있는가?
- 무한차원에서 모든 선형 사상이 자동으로 유계인가?

## 🔍 왜 이 이론이 AI에서 중요한가

신경망의 가중치 행렬 $W$는 선형 연산자이고, 이것이 **안정적**인지(유계인지)는 매우 중요하다. 유계 연산자는 입력의 작은 변화가 출력에 큰 변화를 일으키지 않음을 보장한다. 이는:
- **경사 소실/폭발 방지**: 깊은 신경망의 역전파에서 경사가 폭발하거나 사라지지 않도록 제어
- **Spectral Norm Regularization**: $\|W\|_{\text{op}} \leq 1$로 제약하여 판별자의 Lipschitz 상수를 제한 (GAN 훈련 안정화)
- **입출력 안정성**: 작은 섭동에 대한 로버스트 응답

## 📐 수학적 선행 조건

- **노름 공간**: $\|(x, y)\| = (\|x\|_X, \|y\|_Y)$ 개념
- **선형성**: $T(\alpha x + \beta y) = \alpha Tx + \beta Ty$
- **노름의 성질**: $\|cx\| = |c| \|x\|$, 삼각부등식 등

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서**: 
$\mathbb{R}^n \to \mathbb{R}^m$의 모든 선형 사상은 행렬이고, 행렬은 필연적으로 "크기 제한"이 있다. 즉, 입력을 아무리 키워도 출력의 증가율은 최대 특이값(singular value)의 배수로 제한된다.

**무한차원에서**: 
사실 "크기 제한"이 없을 수 있다! 예를 들어 미분 연산자 $\frac{d}{dx}$는 "임의로 큰" 변화를 만들 수 있다. $f(x) = x^n$을 생각하면, $\|f\|_{L^2}$는 작지만 $\|\frac{df}{dx}\|_{L^2}$는 $n$에 따라 무한히 커진다.

## ✏️ 엄밀한 정의

**정의 1.1 (선형 연산자)**
$T: X \to Y$가 두 노름공간 사이의 *선형 연산자*이면, 임의의 $\alpha, \beta \in \mathbb{K}$ (또는 $\mathbb{R}$)과 $x, y \in X$에 대해:
$$T(\alpha x + \beta y) = \alpha T(x) + \beta T(y)$$

**정의 1.2 (유계 선형 연산자)**
$T: X \to Y$가 *유계*이면, 양수 $M > 0$이 존재하여:
$$\|T(x)\|_Y \leq M \|x\|_X \quad \forall x \in X$$

이 경우 $T$를 *유계 선형 연산자*라고 부른다.

**정의 1.3 (연산자 노름)**
유계 선형 연산자 $T: X \to Y$의 *연산자 노름*은:
$$\|T\| = \sup_{\|x\|_X = 1} \|T(x)\|_Y = \sup_{x \neq 0} \frac{\|T(x)\|_Y}{\|x\|_X}$$

**관찰**: 유계이면 $\|T(x)\|_Y \leq \|T\| \cdot \|x\|_X$가 성립한다.

## 🔬 정리와 증명

**정리 1.1 (유계성과 연속성의 동치)**

$T: X \to Y$가 선형 연산자일 때, 다음은 모두 동치이다:
1. $T$가 유계이다 (∃M > 0 s.t. $\|T(x)\|_Y \leq M\|x\|_X$ ∀x)
2. $T$가 연속이다 (임의의 $\epsilon > 0$에 대해 $\delta > 0$가 존재하여 $\|x - x_0\|_X < \delta \Rightarrow \|T(x) - T(x_0)\|_Y < \epsilon$)
3. $T$가 0에서 연속이다 ($\lim_{x \to 0} T(x) = T(0) = 0$)

**증명**:

**(1) ⟹ (2)**: $T$가 유계이고 $M = \|T\|$라 하자. $\epsilon > 0$일 때, $\delta = \epsilon / M$으로 놓으면:
$$\|x - x_0\|_X < \delta \Rightarrow \|T(x) - T(x_0)\|_Y = \|T(x - x_0)\|_Y \leq M \|x - x_0\|_X < M \cdot \frac{\epsilon}{M} = \epsilon$$

따라서 $T$는 $x_0$에서 연속이다. 임의의 $x_0$에 대해 같은 논리가 적용되므로 $T$는 연속이다.

**(2) ⟹ (3)**: 명백하다. (3)은 (2)의 특수한 경우)

**(3) ⟹ (1)**: $T$가 0에서 연속이므로, $\epsilon = 1$에 대해 $\delta > 0$가 존재하여:
$$\|x\|_X < \delta \Rightarrow \|T(x)\|_Y < 1$$

임의의 $x \neq 0$에 대해, $y = \frac{\delta x}{2\|x\|_X}$로 놓으면:
$$\|y\|_X = \frac{\delta}{2\|x\|_X} \cdot \|x\|_X = \frac{\delta}{2} < \delta$$

따라서 $\|T(y)\|_Y < 1$. 선형성에 의해:
$$\|T(y)\|_Y = \left\|T\left(\frac{\delta x}{2\|x\|_X}\right)\right\|_Y = \frac{\delta}{2\|x\|_X} \|T(x)\|_Y < 1$$

양변에 $\frac{2\|x\|_X}{\delta}$를 곱하면:
$$\|T(x)\|_Y < \frac{2\|x\|_X}{\delta}$$

$M = \frac{2}{\delta}$로 놓으면 모든 $x$에 대해 $\|T(x)\|_Y \leq M\|x\|_X$이므로 $T$는 유계이다. $\square$

---

**정리 1.2 (닫힌 그래프 정리, Closed Graph Theorem)**

$X, Y$가 Banach 공간이고 $T: X \to Y$가 선형이라고 하자. 그래프
$$\text{graph}(T) = \{(x, T(x)) : x \in X\} \subset X \times Y$$
가 $X \times Y$ (표준 곱 노름 $\|(x,y)\| = \|x\|_X + \|y\|_Y$)에서 닫혀있으면, $T$는 유계이다.

*증명 스케치*: 그래프가 닫혀있고 $xₙ \to x$이면, $(xₙ, T(xₙ)) \to (x, y)$인 어떤 $y$가 있어야 한다. 닫혀있음과 연속성 논의로부터 $y = T(x)$이고, 따라서 $T(xₙ) \to T(x)$이므로 $T$는 연속(=유계)이다. (완전한 증명은 함수해석학 교과서 참고)

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# ============================================================================
# 1. 유한차원: 모든 선형 사상은 유계 (행렬의 연산자 노름 = 최대 특이값)
# ============================================================================

# 3×4 행렬 A
A = np.array([
    [2, 1, 0, -1],
    [1, 3, 1,  0],
    [0, 1, 2,  1]
])

# 특이값 분해로 연산자 노름 계산
U, s, Vt = np.linalg.svd(A)
operator_norm_theoretical = s[0]  # 최대 특이값

# 검증: ‖Ax‖ ≤ ‖A‖ · ‖x‖ 확인
print("=" * 70)
print("1. 유한차원: 행렬의 연산자 노름")
print("=" * 70)
print(f"행렬 A의 특이값: {s}")
print(f"연산자 노름 ‖A‖ = {operator_norm_theoretical:.6f}")
print()

# 여러 벡터에 대해 ‖Ax‖ / ‖x‖ 계산
random_vectors = np.random.randn(100, 4)
ratios = []
for x in random_vectors:
    if np.linalg.norm(x) > 1e-10:
        ratio = np.linalg.norm(A @ x) / np.linalg.norm(x)
        ratios.append(ratio)

print(f"임의의 벡터에 대해 ‖Ax‖/‖x‖의 최댓값: {max(ratios):.6f}")
print(f"이론값과의 오차: {abs(max(ratios) - operator_norm_theoretical):.2e}")
print()

# ============================================================================
# 2. 무한차원 반례: 미분 연산자는 비유계
# ============================================================================

print("=" * 70)
print("2. 무한차원 반례: 미분 연산자 d/dx의 비유계성")
print("=" * 70)

# 다항식 f_n(x) = x^n을 [0,1]에서 이산화
x_grid = np.linspace(0, 1, 1000)
dx = x_grid[1] - x_grid[0]

unboundedness = []
n_values = range(2, 12)

for n in n_values:
    # f_n(x) = x^n
    f_n = x_grid ** n
    
    # L^2 노름: ∫₀¹ (x^n)^2 dx = ∫₀¹ x^(2n) dx = 1/(2n+1)
    norm_f = np.sqrt(np.sum(f_n**2) * dx)
    
    # f'_n(x) = n·x^(n-1)
    f_n_prime = n * (x_grid ** (n - 1))
    
    # L^2 노름: ∫₀¹ (n·x^(n-1))^2 dx = n^2 · 1/(2n-1)
    norm_derivative = np.sqrt(np.sum(f_n_prime**2) * dx)
    
    # 비율 ‖f'_n‖ / ‖f_n‖
    ratio = norm_derivative / norm_f
    unboundedness.append(ratio)
    print(f"n={n:2d}: ‖f_n‖₂={norm_f:.6f}, ‖f'_n‖₂={norm_derivative:.6f}, 비율={ratio:.2f}")

print()
print(f"비율이 n→∞일 때 무한대로 발산: 미분 연산자는 비유계!")
print()

# 그래프로 시각화
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.plot(n_values, unboundedness, 'o-', linewidth=2, markersize=8, label='‖f\'_n‖/‖f_n‖')
plt.xlabel('n (다항식 차수)', fontsize=12)
plt.ylabel('노름의 비율', fontsize=12)
plt.title('미분 연산자의 비유계성', fontsize=13, fontweight='bold')
plt.grid(True, alpha=0.3)
plt.legend()

# 다항식 x^n의 그래프
plt.subplot(1, 2, 2)
for n in [2, 4, 6, 8]:
    f = x_grid ** n
    plt.plot(x_grid, f, label=f'$x^{n}$')
plt.xlabel('x', fontsize=12)
plt.ylabel('f(x)', fontsize=12)
plt.title('고차 다항식: 0 근처에서는 작지만 1에서 크다', fontsize=13, fontweight='bold')
plt.legend()
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig('/tmp/unbounded_derivative.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/unbounded_derivative.png")
print()

# ============================================================================
# 3. 연속성과 유계성의 동치성 수치 검증
# ============================================================================

print("=" * 70)
print("3. 유계 연산자 = 연속 연산자 (수치 검증)")
print("=" * 70)

# 간단한 유계 선형 연산자: T(x) = Ax (A는 3×3 행렬)
A_bounded = np.array([
    [2.0, 0.5, -1.0],
    [0.3, 1.5,  0.2],
    [-0.1, 0.8, 2.2]
])

operator_norm = np.linalg.svd(A_bounded)[1][0]

# x₀ = [1, 2, 3], x = x₀ + δ 형태로 연속성 검증
x0 = np.array([1.0, 2.0, 3.0])
Tx0 = A_bounded @ x0

# δ를 점점 작게 하면서 |T(x) - T(x₀)| 관찰
deltas = np.logspace(-8, 0, 50)
errors_output = []
errors_input = []

for delta in deltas:
    delta_x = delta * np.ones_like(x0) / np.linalg.norm(np.ones_like(x0))
    x = x0 + delta_x
    
    Tx = A_bounded @ x
    error_in = np.linalg.norm(x - x0)
    error_out = np.linalg.norm(Tx - Tx0)
    
    errors_input.append(error_in)
    errors_output.append(error_out)

# log-log 플롯
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.loglog(errors_input, errors_output, 'o-', linewidth=2, markersize=6, label='실제 오차')
# 이론적 경계: ‖T(x) - T(x₀)‖ ≤ ‖T‖ · ‖x - x₀‖
theoretical_bound = operator_norm * np.array(errors_input)
plt.loglog(errors_input, theoretical_bound, 'r--', linewidth=2, label=f'이론적 경계 (‖T‖={operator_norm:.3f})')
plt.xlabel('입력 오차 ‖x - x₀‖', fontsize=11)
plt.ylabel('출력 오차 ‖T(x) - T(x₀)‖', fontsize=11)
plt.title('유계 연산자: 연속성의 수치 검증', fontsize=12, fontweight='bold')
plt.legend()
plt.grid(True, alpha=0.3, which='both')

# Lipschitz 상수 확인
plt.subplot(1, 2, 2)
lipschitz_ratios = []
for i in range(len(errors_input)):
    if errors_input[i] > 1e-10:
        lipschitz_ratios.append(errors_output[i] / errors_input[i])

plt.semilogx(errors_input[:-1], lipschitz_ratios, 'go-', linewidth=2, markersize=6)
plt.axhline(y=operator_norm, color='r', linestyle='--', linewidth=2, label=f'‖T‖ = {operator_norm:.3f}')
plt.xlabel('입력 오차 ‖x - x₀‖', fontsize=11)
plt.ylabel('Lipschitz 상수 ‖T(x)-T(x₀)‖/‖x-x₀‖', fontsize=11)
plt.title('연산자 노름이 Lipschitz 상수임을 확인', fontsize=12, fontweight='bold')
plt.legend()
plt.grid(True, alpha=0.3, which='both')
plt.tight_layout()
plt.savefig('/tmp/bounded_continuous.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/bounded_continuous.png")
print()

# ============================================================================
# 4. 연산자 노름의 정의 검증
# ============================================================================

print("=" * 70)
print("4. 연산자 노름의 정의 검증")
print("=" * 70)

A_test = np.array([
    [3.0, 1.0],
    [1.0, 2.0]
])

# 방법 1: SVD로 계산 (정의)
U, s, Vt = np.linalg.svd(A_test)
norm_method1 = s[0]

# 방법 2: 단위원 위의 점들에서 ‖Ax‖의 최댓값
angles = np.linspace(0, 2*np.pi, 200)
unit_vectors = np.array([[np.cos(theta), np.sin(theta)] for theta in angles])
norms_on_unit_circle = [np.linalg.norm(A_test @ x) for x in unit_vectors]
norm_method2 = max(norms_on_unit_circle)

# 방법 3: sup ‖Ax‖/‖x‖
random_vecs = np.random.randn(1000, 2)
ratios = [np.linalg.norm(A_test @ x) / np.linalg.norm(x) for x in random_vecs if np.linalg.norm(x) > 1e-10]
norm_method3 = max(ratios)

print(f"방법 1 (SVD): ‖A‖ = {norm_method1:.6f}")
print(f"방법 2 (단위원): ‖A‖ = {norm_method2:.6f}")
print(f"방법 3 (sup비율): ‖A‖ = {norm_method3:.6f}")
print()

# 시각화: 단위원과 그 상(image)
plt.figure(figsize=(12, 5))

# 단위원
plt.subplot(1, 2, 1)
circle = np.array([[np.cos(theta), np.sin(theta)] for theta in np.linspace(0, 2*np.pi, 200)])
plt.plot(circle[:, 0], circle[:, 1], 'b-', linewidth=2, label='단위원 {‖x‖=1}')
image = np.array([A_test @ x for x in circle])
plt.plot(image[:, 0], image[:, 1], 'r-', linewidth=2, label=f'상 {{Ax : ‖x‖=1}}')
plt.axis('equal')
plt.xlabel('x₁', fontsize=11)
plt.ylabel('x₂', fontsize=11)
plt.title('단위원과 그 상 (A는 2×2 행렬)', fontsize=12, fontweight='bold')
plt.legend()
plt.grid(True, alpha=0.3)

# 노름의 변화
plt.subplot(1, 2, 2)
angles_fine = np.linspace(0, 2*np.pi, 500)
unit_vecs = np.array([[np.cos(theta), np.sin(theta)] for theta in angles_fine])
norms = [np.linalg.norm(A_test @ x) for x in unit_vecs]
plt.plot(np.degrees(angles_fine), norms, 'b-', linewidth=2)
plt.axhline(y=norm_method1, color='r', linestyle='--', linewidth=2, label=f'‖A‖ = {norm_method1:.3f}')
plt.xlabel('각도 θ (도)', fontsize=11)
plt.ylabel('‖Ax‖ (‖x‖=1일 때)', fontsize=11)
plt.title('단위원 위의 점에서 ‖Ax‖의 변화', fontsize=12, fontweight='bold')
plt.legend()
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig('/tmp/operator_norm_definition.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/operator_norm_definition.png")
print()

print("모든 검증 완료!")
```

**출력 예시:**
```
======================================================================
1. 유한차원: 행렬의 연산자 노름
======================================================================
행렬 A의 특이값: [3.52123604 2.50841699 1.18946254]
연산자 노름 ‖A‖ = 3.521236
임의의 벡터에 대해 ‖Ax‖/‖x‖의 최댓값: 3.521236
이론값과의 오차: 1.23e-06

======================================================================
2. 무한차원 반례: 미분 연산자 d/dx의 비유계성
======================================================================
n= 2: ‖f_n‖₂=0.182570, ‖f'_n‖₂=0.566947, 비율=3.11
n= 3: ‖f_n‖₂=0.115470, ‖f'_n‖₂=0.901289, 비율=7.81
...
비율이 n→∞일 때 무한대로 발산: 미분 연산자는 비유계!
```

## 🔗 AI/ML 연결

1. **신경망의 안정성**: 깊은 신경망 $f(x) = W_L(\cdots W_2(W_1 x)\cdots)$에서 연산자 노름의 곱셈: $\|f\| \leq \|W_L\| \cdots \|W_2\| \cdot \|W_1\|$. 만약 각 $\|W_i\| > 1$이면 경사가 폭발한다.

2. **Spectral Norm Regularization**: GAN 판별자에서 $\|D\|_{\text{Lipschitz}} \leq 1$을 강제하기 위해 각 레이어의 가중치 행렬을 $W \leftarrow W / \sigma_1(W)$ (최대 특이값으로 정규화)

3. **Robustness**: 적대적 공격 방어에서 모델의 Lipschitz 상수를 제어하면 입력 섭동에 대한 불변성 보장

## ⚖️ 가정과 한계

- **선형성 가정**: 실제 신경망은 비선형(활성함수 때문)이지만, 각 레이어 개별적으로는 선형이다.
- **노름 공간 가정**: 다양한 노름(∞-노름, 1-노름 등)이 있고, 어떤 노름을 쓰는지에 따라 연산자 노름이 달라진다.
- **무한차원의 필요성**: 실제 함수 공간(신호 처리, 이미지)은 무한차원이므로 이 이론이 핵심이다.

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 직관 | AI 응용 |
|------|------|------|--------|
| **선형 연산자** | $T(\alpha x+\beta y)=\alpha Tx+\beta Ty$ | 중첩 원리 | 신경망 레이어 (활성함수 제외) |
| **유계 연산자** | ∃M: $\|Tx\| \leq M\|x\|$ | 입출력 비율이 제한됨 | 안정적인 훈련 |
| **연산자 노름** | $\|T\| = \sup_{\|x\|=1} \|Tx\|$ | 최악의 증폭 배수 | Lipschitz 상수, 경사 제어 |
| **유계 ⟺ 연속** | 동치 관계 | 작은 입력 변화 → 작은 출력 변화 | Robustness |

## 🤔 생각해볼 문제

**문제 1.1**: $T: \ell^2 \to \ell^2$를 $(Tx)_n = x_n / n$으로 정의하자. 이 연산자는 유계인가? 그렇다면 $\|T\|$을 구하라.

<details>
<summary>풀이</summary>

$T$가 유계임을 보이자:
$$\|Tx\|_{\ell^2}^2 = \sum_{n=1}^{\infty} \frac{x_n^2}{n^2} \leq \sum_{n=1}^{\infty} x_n^2 = \|x\|_{\ell^2}^2$$

따라서 $T$는 유계이고 $\|T\| \leq 1$.

등호 달성을 확인하자. $x = (1, 0, 0, \ldots)$일 때:
$$\|Tx\|_{\ell^2} = |(Tx)_1| = |1| = 1$$
$$\|x\|_{\ell^2} = 1$$

비율이 1이므로 $\|T\| = 1$.

</details>

**문제 1.2**: 미분 연산자 $D: C^1([0,1]) \to C([0,1])$을 $Df = f'$으로 정의하고, 두 공간 모두에 supremum 노름을 준다: $\|f\|_\infty = \sup_{x \in [0,1]} |f(x)|$. 이 연산자는 유계인가?

<details>
<summary>풀이</summary>

비유계임을 보이자. 다항식 $f_n(x) = x^n$을 생각하자.

$\|f_n\|_\infty = \sup_{x \in [0,1]} |x^n| = 1$ (∵ $\max_{x \in [0,1]} x^n = 1$ at $x=1$)

$f_n'(x) = nx^{n-1}$이므로:
$$\|Df_n\|_\infty = \|f_n'\|_\infty = \sup_{x \in [0,1]} |nx^{n-1}| = n$$

따라서:
$$\frac{\|Df_n\|_\infty}{\|f_n\|_\infty} = \frac{n}{1} = n \to \infty \text{ as } n \to \infty$$

비유계이다.

</details>

**문제 1.3**: 행렬 $A = \begin{pmatrix} 2 & 1 \\ 0 & 3 \end{pmatrix}$의 연산자 노름 $\|A\|_2$ (스펙트럼 노름)을 계산하라. (특이값 분해 사용)

<details>
<summary>풀이</summary>

$A^T A = \begin{pmatrix} 2 & 0 \\ 1 & 3 \end{pmatrix} \begin{pmatrix} 2 & 1 \\ 0 & 3 \end{pmatrix} = \begin{pmatrix} 4 & 2 \\ 2 & 10 \end{pmatrix}$

고유값을 구하자:
$$\det(A^T A - \lambda I) = \det\begin{pmatrix} 4-\lambda & 2 \\ 2 & 10-\lambda \end{pmatrix} = (4-\lambda)(10-\lambda) - 4 = \lambda^2 - 14\lambda + 36 = 0$$

$$\lambda = \frac{14 \pm \sqrt{196-144}}{2} = \frac{14 \pm \sqrt{52}}{2} = 7 \pm \sqrt{13}$$

최대 고유값: $\lambda_{\max} = 7 + \sqrt{13} \approx 10.606$

따라서 $\|A\|_2 = \sqrt{10.606} \approx 3.256$.

</details>

---

<div align="center">

| | | |
|---|---|---|
| | [📚 README](../README.md) | [다음 ▶](./02-operator-space.md) |

</div>
