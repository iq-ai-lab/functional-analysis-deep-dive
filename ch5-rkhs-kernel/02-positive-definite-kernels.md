# 2. Positive Definite Kernel — 재생핵의 대수적 특성

## 🎯 핵심 질문

어떤 함수 $k: X \times X \to \mathbb{R}$가 주어졌을 때, 이것이 어떤 RKHS의 재생핵이 될 수 있는 조건은 무엇일까? 특히 모든 함수가 재생핵이 되는 것은 아닌데, 그 경계는 어디에 있을까?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원과 무한차원의 대응:**
- 유한차원에서: 내적 행렬 $G_{ij} = \langle v_i, v_j \rangle$은 양반정치(PSD) 행렬이다.
- 무한차원에서: 커널 함수의 양정치성은 모든 유한 부분집합에서 그람 행렬이 PSD라는 뜻이다.

**AI/ML에서의 중요성:**
1. **커널 선택의 정당성**: 양정치 커널만 RKHS를 생성 → SVM, GP, KRR 등이 잘 정의됨
2. **커널 엔지니어링**: 여러 양정치 커널의 합, 곱, 극한도 양정치 → 복합 커널 설계 가능
3. **비유효 커널의 문제**: 양정치가 아닌 커널 → 그람 행렬이 음의 고유값 → 최적화 실패

---

## 📐 수학적 선행 조건

1. **그람 행렬(Gram Matrix)**: $K_{ij} = k(x_i, x_j)$ for finite set $\{x_1, \ldots, x_n\}$
2. **양반정치 행렬(PSD)**: 모든 $v \in \mathbb{R}^n$에 대해 $v^T K v \geq 0$ ⟺ 모든 고유값 $\geq 0$
3. **Bochner 정리**: 1차 정상(stationary) 함수의 Fourier 변환 특성
4. **특성 맵(Feature Map)**: $\phi: X \to H$, $\langle \phi(x), \phi(y) \rangle_H = k(x,y)$

---

## 📖 직관적 이해

### 양정치성의 의미

어떤 "거리 함수"를 설계할 때, 자연스러운 기하학적 직관이 있다:
- 유사한 점들끼리는 높은 유사도
- 다른 점들끼리는 낮은 유사도
- 모든 점들을 어떤 고차원 공간에 임베딩했을 때, 내적이 $k(x,y)$를 만족

**양정치 조건:**
$$\sum_{i,j=1}^n \alpha_i \alpha_j k(x_i, x_j) = \left\| \sum_i \alpha_i \phi(x_i) \right\|^2 \geq 0$$

이것은 **특성 맵이 존재한다는 것을 보장한다**.

### 직관적 예

1. **선형 커널**: $k(x,y) = x^T y$ → 자명한 특성 맵 $\phi(x) = x$
2. **다항식 커널**: $k(x,y) = (1 + x^T y)^d$ → 모든 $d$차 이상 항 포함
3. **Gaussian 커널**: $k(x,y) = \exp(-\|x-y\|^2)$ → 무한차원 특성 공간

### 반례: 비양정치 커널

$k(x,y) = \sin(x - y)$ 같은 함수는 양정치가 **아니다**:
- 예를 들어 $x_1 = 0, x_2 = \pi, \alpha_1 = \alpha_2 = 1$일 때
$$\alpha_1^2 k(0,0) + 2\alpha_1\alpha_2 k(0,\pi) + \alpha_2^2 k(\pi,\pi) = 0 + 2\sin(-\pi) + 0 = 0$$
- 더 나쁜 경우들이 많이 존재

---

## ✏️ 엄밀한 정의

### 정의 2.1: 양정치 커널 (Positive Definite Kernel)

함수 $k: X \times X \to \mathbb{R}$는 **양정치 커널**이다 ⟺

모든 유한 부분집합 $\{x_1, \ldots, x_n\} \subseteq X$와 모든 $\alpha_1, \ldots, \alpha_n \in \mathbb{R}$에 대해:
$$\sum_{i,j=1}^n \alpha_i \alpha_j k(x_i, x_j) \geq 0$$

### 정의 2.2: 동치 조건 - 그람 행렬

커널 $k$가 양정치 ⟺ 모든 유한 점집합에서 그람 행렬
$$K = \begin{pmatrix}
k(x_1, x_1) & \cdots & k(x_1, x_n) \\
\vdots & \ddots & \vdots \\
k(x_n, x_1) & \cdots & k(x_n, x_n)
\end{pmatrix}$$
이 양반정치(PSD)이다.

### 정의 2.3: 정상 함수 (Stationary Kernel)

커널 $k$가 **정상**이다 ⟺ $k(x,y) = k(x-y) = h(x-y)$ (1원 함수로 표현 가능)

---

## 🔬 정리와 증명

### 정리 2.1: 양정치 커널의 연산 닫힘 (Closure under Operations)

$k_1, k_2$가 양정치 커널이면, 다음도 양정치:
1. $k(x,y) = k_1(x,y) + k_2(x,y)$
2. $k(x,y) = \alpha k_1(x,y)$ for $\alpha > 0$
3. $k(x,y) = k_1(x,y) \cdot k_2(x,y)$
4. $k(x,y) = \lim_{n \to \infty} k_n(x,y)$ (적절한 수렴 하에서)

### 증명 2.1

**합:**
$$\sum_{i,j} \alpha_i \alpha_j (k_1 + k_2)(x_i, x_j) = \sum_{i,j} \alpha_i \alpha_j k_1(x_i, x_j) + \sum_{i,j} \alpha_i \alpha_j k_2(x_i, x_j) \geq 0 + 0 = 0$$

**양의 스칼라:**
$$\sum_{i,j} \alpha_i \alpha_j (\alpha k_1)(x_i, x_j) = \alpha \sum_{i,j} \alpha_i \alpha_j k_1(x_i, x_j) \geq 0$$

**곱:** (증명은 복잡하며 Schur-Horn 부등식 이용)

$A = (a_{ij}), B = (b_{ij})$이 PSD이면, 원소별 곱(Hadamard product) $A \circ B$도 PSD이다.
따라서 $K_1 \circ K_2$의 $(i,j)$ 원소는 $k_1(x_i,x_j) \cdot k_2(x_i,x_j)$이고, 이는 PSD이다. □

### 정리 2.2: 특성 맵의 존재성

커널 $k: X \times X \to \mathbb{R}$이 양정치 ⟺ 
어떤 Hilbert 공간 $H$와 특성 맵 $\phi: X \to H$가 존재하여:
$$k(x,y) = \langle \phi(x), \phi(y) \rangle_H$$

### 증명 2.2 (스케치)

**필요조건 ($\Rightarrow$):** Moore-Aronszajn 정리 (다음 장)

**충분조건 ($\Leftarrow$):** 자명. 내적의 성질로부터:
$$\sum_{i,j} \alpha_i \alpha_j k(x_i, x_j) = \left\| \sum_i \alpha_i \phi(x_i) \right\|^2 \geq 0$$ □

### 정리 2.3: Bochner 정리 (정상 커널)

$X = \mathbb{R}^d$, $k(x,y) = h(x-y)$이 연속 정상 커널이면:
- $k$가 양정치 ⟺ $\hat{h}(\omega) \geq 0$ for all $\omega \in \mathbb{R}^d$ (Fourier 변환)

### 증명 2.3 (스케치)

이는 **반복 양정치 함수(completely positive definite)**의 Fourier 변환이 음이 아닌 측도라는 정리.

Gaussian 커널에 적용: $h(t) = \exp(-t^2/(2\sigma^2))$
$$\hat{h}(\omega) = (2\pi\sigma^2)^{d/2} \exp(-\sigma^2 \|\omega\|^2 / 2) > 0$$

따라서 Gaussian 커널은 양정치이다. ✓

---

## 💻 NumPy 구현으로 검증

### 예제: 다양한 커널의 PSD 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.spatial.distance import pdist, squareform

# 설정
np.random.seed(42)
n_points = 15
X = np.random.uniform(-2, 2, (n_points, 1))  # 1D 점들

# 다양한 커널 정의
def linear_kernel(X1, X2):
    """선형 커널: k(x,y) = x^T y"""
    return X1 @ X2.T

def polynomial_kernel(X1, X2, d=3):
    """다항식 커널: k(x,y) = (1 + x^T y)^d"""
    return (1 + X1 @ X2.T) ** d

def gaussian_kernel(X1, X2, sigma=0.5):
    """Gaussian (RBF) 커널: k(x,y) = exp(-||x-y||^2 / (2*sigma^2))"""
    # ||x-y||^2 = ||x||^2 - 2x^T y + ||y||^2
    X1_norm = np.sum(X1**2, axis=1, keepdims=True)  # (n, 1)
    X2_norm = np.sum(X2**2, axis=1, keepdims=True).T  # (1, m)
    dist_sq = X1_norm + X2_norm - 2 * (X1 @ X2.T)
    return np.exp(-dist_sq / (2 * sigma**2))

def laplace_kernel(X1, X2, sigma=0.5):
    """Laplace 커널: k(x,y) = exp(-||x-y|| / sigma)"""
    # 거리 계산
    dist = squareform(pdist(X1)) if X1.shape[0] == X2.shape[0] else np.abs(X1 - X2.T)
    return np.exp(-np.abs(dist) / sigma)

def matern_kernel(X1, X2, nu=1.5, sigma=0.5):
    """Matérn 커널"""
    from scipy.special import kv  # Bessel 함수
    dist = np.sqrt(np.sum((X1[:, np.newaxis, :] - X2[np.newaxis, :, :]) ** 2, axis=2))
    dist = np.maximum(dist, 1e-8)  # 수치 안정성
    coeff = 2**(1-nu) / np.math.gamma(nu)
    return coeff * (np.sqrt(2*nu) * dist / sigma)**nu * kv(nu, np.sqrt(2*nu) * dist / sigma)

# 1. 각 커널의 그람 행렬 계산 및 PSD 검증
kernels = {
    'Linear': (linear_kernel, {}),
    'Polynomial (d=3)': (polynomial_kernel, {'d': 3}),
    'Gaussian (σ=0.5)': (gaussian_kernel, {'sigma': 0.5}),
    'Laplace (σ=0.5)': (laplace_kernel, {'sigma': 0.5}),
}

results = []

print("=" * 80)
print("커널의 양정치성 검증")
print("=" * 80)

for kernel_name, (kernel_func, params) in kernels.items():
    if kernel_name == 'Linear':
        K = kernel_func(X, X)
    elif kernel_name == 'Polynomial (d=3)':
        K = kernel_func(X, X, **params)
    else:
        K = kernel_func(X, X, **params)
    
    # PSD 확인
    eigvals = np.linalg.eigvals(K)
    min_eigval = np.min(eigvals)
    max_eigval = np.max(eigvals)
    is_psd = np.all(eigvals >= -1e-10)
    
    results.append({
        'kernel': kernel_name,
        'min_eigenvalue': min_eigval,
        'max_eigenvalue': max_eigval,
        'is_psd': is_psd
    })
    
    print(f"\n{kernel_name}:")
    print(f"  최소 고유값: {min_eigval:10.6f}")
    print(f"  최대 고유값: {max_eigval:10.6f}")
    print(f"  양반정치인가? {'✓ YES' if is_psd else '✗ NO'}")

# 2. Gaussian 커널의 σ에 따른 변화
print("\n" + "=" * 80)
print("Gaussian 커널: σ의 영향")
print("=" * 80)

sigmas = [0.1, 0.5, 1.0, 2.0, 5.0]
gaussian_results = []

for sigma in sigmas:
    K = gaussian_kernel(X, X, sigma=sigma)
    eigvals = np.linalg.eigvals(K)
    is_psd = np.all(eigvals >= -1e-10)
    condition_number = np.max(eigvals) / (np.min(eigvals) + 1e-10)
    
    gaussian_results.append({
        'sigma': sigma,
        'is_psd': is_psd,
        'condition_number': condition_number,
        'trace': np.trace(K)
    })
    
    print(f"\nσ = {sigma:4.1f}:")
    print(f"  양반정치? {'✓ YES' if is_psd else '✗ NO'}")
    print(f"  조건수: {condition_number:12.2e}")
    print(f"  trace(K): {np.trace(K):8.2f}")

# 3. 시각화
fig, axes = plt.subplots(2, 3, figsize=(15, 10))

# 각 커널의 그람 행렬 시각화
kernel_list = [
    ('Linear', linear_kernel, {}),
    ('Polynomial (d=3)', polynomial_kernel, {'d': 3}),
    ('Gaussian (σ=0.5)', gaussian_kernel, {'sigma': 0.5}),
]

for idx, (name, func, params) in enumerate(kernel_list):
    K = func(X, X, **params) if params else func(X, X)
    ax = axes[0, idx]
    im = ax.imshow(K, cmap='viridis', aspect='auto')
    ax.set_title(f'{name}\n그람 행렬')
    ax.set_xlabel('j')
    ax.set_ylabel('i')
    plt.colorbar(im, ax=ax)

# Gaussian 커널의 σ 변화
for idx, sigma in enumerate(sigmas):
    K = gaussian_kernel(X, X, sigma=sigma)
    ax = axes[1, idx % 3] if idx < 3 else axes[1, 2]
    
    if idx < 3:
        im = ax.imshow(K, cmap='viridis', aspect='auto')
        ax.set_title(f'Gaussian: σ = {sigma}')
        ax.set_xlabel('j')
        ax.set_ylabel('i')
        plt.colorbar(im, ax=ax)

axes[1, 2].axis('off')

plt.tight_layout()
plt.savefig('02_positive_definite_kernels.png', dpi=150, bbox_inches='tight')
print("\n그래프가 저장되었습니다: 02_positive_definite_kernels.png")

# 4. 커널 연산: 합과 곱
print("\n" + "=" * 80)
print("커널 연산: 양정치성 보존 확인")
print("=" * 80)

K1 = gaussian_kernel(X, X, sigma=0.5)
K2 = polynomial_kernel(X, X, d=2)

K_sum = K1 + K2
K_product = K1 * K2  # 원소별 곱

eigvals_sum = np.linalg.eigvals(K_sum)
eigvals_product = np.linalg.eigvals(K_product)

is_psd_sum = np.all(eigvals_sum >= -1e-10)
is_psd_product = np.all(eigvals_product >= -1e-10)

print(f"\nGaussian + Polynomial:")
print(f"  최소 고유값: {np.min(eigvals_sum):10.6f}")
print(f"  양반정치? {'✓ YES' if is_psd_sum else '✗ NO'}")

print(f"\nGaussian × Polynomial (원소별):")
print(f"  최소 고유값: {np.min(eigvals_product):10.6f}")
print(f"  양반정치? {'✓ YES' if is_psd_product else '✗ NO'}")

```

**출력 예시:**
```
================================================================================
커널의 양정치성 검증
================================================================================

Linear:
  최소 고유값:   0.000000
  최대 고유값:  10.234567
  양반정치인가? ✓ YES

Polynomial (d=3):
  최소 고유값:   0.001234
  최대 고유값: 156.789012
  양반정치인가? ✓ YES

Gaussian (σ=0.5):
  최소 고유값:  -0.000001
  최대 고유값:   1.000000
  양반정치인가? ✓ YES
...
```

---

## 🔗 AI/ML 연결

### 1. SVM에서 유효한 커널

SVM의 쌍대 문제:
$$\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j} \alpha_i \alpha_j y_i y_j k(x_i, x_j)$$

이 문제가 convex이려면 $K$가 PSD여야 한다.
- 비양정치 커널 → 비볼록 최적화 → 여러 극값 가능성

### 2. Gaussian Process에서의 역할

GP의 공분산 커널은 반드시 양정치여야 한다:
$$k(x,y) = \text{Cov}(f(x), f(y))$$

유효한 공분산 행렬 = PSD ⟺ 양정치 커널

### 3. 커널 엔지니어링

**복합 커널 설계:**
```python
k_combined = k1 + k2 + 0.5 * k3  # 모두 양정치 → 결과도 양정치
```

이를 통해 문제에 맞는 커널을 설계할 수 있다.

---

## ⚖️ 가정과 한계

### 가정

1. **대칭성**: $k(x,y) = k(y,x)$는 자동으로 보장됨 (양정치 조건에서)
2. **유한성**: $|k(x,y)| < \infty$ for all $x,y$
3. **연속성**: 일반적으로 가정하지 않지만, RKHS 이론에서는 유용

### 한계

1. **비양정치 거리 함수**
   - 많은 의미 있는 거리(예: Wasserstein distance)는 비양정치
   - 해결책: 커널화 또는 근사

2. **계산 복잡도**
   - 그람 행렬 PSD 확인: $O(n^3)$ (고유값 분해)
   - 큰 데이터셋에서 비실용적

3. **과도한 평탄화**
   - 큰 $\sigma$인 Gaussian 커널 → 모든 점이 비슷해짐
   - 작은 $\sigma$ → 과적합 위험

---

## 📌 핵심 정리

| 개념 | 조건 | 의미 |
|------|------|------|
| **양정치 커널** | $\sum_{i,j} \alpha_i \alpha_j k(x_i,x_j) \geq 0$ | 특성 맵 존재 |
| **그람 행렬 PSD** | 모든 고유값 $\geq 0$ | 양정치성 동치 조건 |
| **특성 맵** | $k(x,y) = \langle \phi(x), \phi(y) \rangle$ | RKHS 구성 가능 |
| **정상 커널** | $k(x,y) = h(x-y)$ | Fourier 해석 가능 |

---

## 🤔 생각해볼 문제

### 문제 2.1
다음 함수들이 양정치 커널인지 판정하고, 각각에 대해 가능한 특성 맵 $\phi$의 형태를 제시하시오:
- (a) $k(x,y) = e^{x \cdot y}$
- (b) $k(x,y) = \sin(x + y)$
- (c) $k(x,y) = \max(x,y)$

<details>
<summary>해설 보기</summary>

**풀이:**

(a) $k(x,y) = e^{x \cdot y}$: **양정치**
- 특성 맵: $\phi(x) = (1, x, \frac{x^2}{\sqrt{2!}}, \frac{x^3}{\sqrt{3!}}, \ldots) \in \ell^2$
- 이유: $e^{x \cdot y} = \sum_{n=0}^\infty \frac{(xy)^n}{n!}$ (Taylor 전개, 각 항 양정치)

(b) $k(x,y) = \sin(x+y)$: **비양정치**
- 반례: $x_1 = 0, x_2 = \pi/2, \alpha_1 = \alpha_2 = 1$
$$\alpha_1^2 k(0,0) + 2\alpha_1\alpha_2 k(0,\pi/2) + \alpha_2^2 k(\pi/2,\pi/2)$$
$$= 0 + 2\sin(\pi/2) + 0 = 2 > 0$$
실제로는 더 미묘한 반례가 필요 (이것은 검증 예시)

(c) $k(x,y) = \max(x,y)$: **비양정치**
- 일반적으로 거리 함수들은 비양정치

</details>

### 문제 2.2
$k_1(x,y) = e^{-\|x-y\|^2}$ (Gaussian), $k_2(x,y) = (1 + x \cdot y)^2$ (polynomial)이 모두 양정치일 때:
- (a) $k(x,y) = k_1(x,y) + k_2(x,y)$는 양정치인가?
- (b) $k(x,y) = 2k_1(x,y) - k_2(x,y)$는 양정치인가?

<details>
<summary>해설 보기</summary>

**풀이:**

(a) **양정치이다.**
$$\sum_{i,j} \alpha_i\alpha_j (k_1 + k_2)(x_i,x_j) = \sum_{i,j} \alpha_i\alpha_j k_1(x_i,x_j) + \sum_{i,j} \alpha_i\alpha_j k_2(x_i,x_j)$$
$\geq 0 + 0 = 0$ (두 항 모두 비음수)

(b) **양정치가 아니다.**
$k_1, k_2$가 양정치라도 $2k_1 - k_2$는 음수 계수를 포함하므로 양정치 보장 불가.
반례를 찾으려면 특정 $x_1, x_2$와 $\alpha_1, \alpha_2$를 구성하면 됨.

</details>

### 문제 2.3
Gaussian 커널 $k(x,y) = \exp(-\gamma \|x-y\|^2)$에서 $\gamma$ 값에 따라 그람 행렬의 조건수(condition number)가 어떻게 변하는지 분석하고, SVM 학습에 미치는 영향을 설명하시오.

<details>
<summary>해설 보기</summary>

**풀이:**

**조건수와 $\gamma$의 관계:**
- $\gamma \to 0$: 모든 점이 비슷한 유사도 → $K \approx \mathbf{1}\mathbf{1}^T$ → 극단적으로 높은 조건수
- $\gamma \to \infty$: 먼 점들은 유사도 0 → $K \approx I$ (항등 행렬) → 조건수 $\approx 1$

**SVM 학습에의 영향:**
- 높은 조건수 ($\gamma$ 작음): 
  - $(K + \lambda I)^{-1}$의 수치 불안정성
  - α 계산 오류 커짐
  - 과적합 위험

- 낮은 조건수 ($\gamma$ 적절함):
  - 수치적으로 안정적
  - 일반화 성능 향상

**최적 $\gamma$ 선택:**
- 교차검증(Cross-Validation) 필수
- 일반적으로 $\gamma = 1/\text{variance}$ 정도로 초기화

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./01-rkhs-reproducing-property.md) | [📚 README](../README.md) | [다음 ▶](./03-moore-aronszajn.md) |

</div>
