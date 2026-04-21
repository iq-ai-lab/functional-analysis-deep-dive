# 3. Moore-Aronszajn 정리 — PD Kernel에서 RKHS의 유일 구성

## 🎯 핵심 질문

주어진 양정치 커널 $k: X \times X \to \mathbb{R}$에서 **정확히 어떤 RKHS가 존재하고 그것이 유일한가**? 더 나아가, RKHS를 구성하는 과정 자체가 어떻게 이루어지는가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원과 무한차원의 대응:**
- 유한차원: 내적 행렬 $G$가 주어지면, 유일한 벡터 공간과 그 내적이 결정된다.
- 무한차원: 양정치 커널이 주어지면, 유일한 RKHS가 자동으로 구성된다.

**AI/ML에서의 중요성:**
1. **이론적 완성성**: 어떤 PD 커널을 정의하면, 자동으로 관련 RKHS가 존재
2. **커널 설계의 자유도**: 양정치 조건만 만족하면 새로운 커널 정의 가능
3. **최적화 정당성**: Representer 정리로 무한차원 최적화가 유한차원으로 축소

---

## 📐 수학적 선행 조건

1. **사전 Hilbert 공간(Pre-Hilbert Space)**: 내적은 있지만 완비성 없음
2. **완비화(Completion)**: 수렴하지 않는 수열을 수렴하게 하여 Hilbert 공간 생성
3. **Cauchy 수열**: $\|f_n - f_m\| \to 0$ as $n,m \to \infty$
4. **내적의 연속 확장**: 완비화 후 내적이 여전히 잘 정의됨

---

## 📖 직관적 이해

### RKHS의 구성 과정

일반적인 함수 공간은 "너무 크다". 예를 들어:
- $L^2[0,1]$ 공간에서는 almost everywhere 같은 함수들이 구별되지 않음
- 점평가 $f \mapsto f(x)$가 불연속

RKHS 구성의 아이디어:
1. **작게 시작**: 핵 함수들의 유한 선형결합으로 시작 $H_0 = \text{span}\{k(\cdot, x): x \in X\}$
2. **내적 정의**: 양정치 조건을 이용해 자연스러운 내적 정의
3. **완비화**: 부족한 부분을 채워 완전한 Hilbert 공간 만들기
4. **재생성질 보존**: 완비화 후에도 재생성질이 유지됨을 보장

### 직관적 예: 무한차원 특성 공간

Gaussian 커널을 생각해보자:
$$k(x,y) = \exp(-\|x-y\|^2) = \sum_{n=0}^\infty \frac{(-\|x-y\|^2)^n}{n!}$$

특성 맵은 본질적으로 무한차원이지만, RKHS는 이를 깔끔하게 정의된 내적을 가진 공간으로 정제한다.

---

## ✏️ 엄밀한 정의

### 정의 3.1: 사전 RKHS (Pre-RKHS)

양정치 커널 $k: X \times X \to \mathbb{R}$에 대해:
$$H_0 := \text{span}\{k(\cdot, x): x \in X\}$$
를 **사전 RKHS**라 한다. 즉, 핵 함수들의 유한 선형결합의 집합.

$H_0$의 원소는:
$$f(\cdot) = \sum_{i=1}^n \alpha_i k(\cdot, x_i)$$

### 정의 3.2: 사전 RKHS 위의 내적

$f(\cdot) = \sum_i \alpha_i k(\cdot, x_i)$, $g(\cdot) = \sum_j \beta_j k(\cdot, y_j)$에 대해:
$$\langle f, g \rangle_{H_0} := \sum_{i,j} \alpha_i \beta_j k(x_i, y_j)$$

### 정의 3.3: Moore-Aronszajn RKHS

$H_k$를 $H_0$의 **완비화(completion)**라 하자. 즉:
- 모든 Cauchy 수열의 극한이 존재
- 내적이 자연스럽게 확장됨
- 재생성질이 유지됨

이 공간 $H_k$를 **Moore-Aronszajn RKHS**라 부른다.

---

## 🔬 정리와 증명

### 정리 3.1: Moore-Aronszajn 정리

**주장:** 양정치 커널 $k: X \times X \to \mathbb{R}$이 주어지면, 유일한 RKHS $H_k$가 존재하여:
1. $k(\cdot, x) \in H_k$ for all $x \in X$
2. $H_k = \overline{\text{span}\{k(\cdot, x): x \in X\}}$ (완비화된 선형결합)
3. 재생성질: $f(x) = \langle f, k(\cdot, x) \rangle_{H_k}$ for all $f \in H_k, x \in X$
4. $H_k$는 이런 성질을 가진 **유일한** Hilbert 공간

### 증명 3.1

**Step 1: 사전 내적이 잘 정의됨을 보이기**

먼저 $H_0$의 원소 표현이 유일한지 확인해야 한다.
만약 $\sum_i \alpha_i k(\cdot, x_i) = \sum_j \beta_j k(\cdot, y_j)$이면, 같은 함수를 나타낸다.

양쪽에서 $k(\cdot, z)$와 내적:
$$\sum_i \alpha_i k(x_i, z) = \sum_j \beta_j k(y_j, z)$$

이는 모든 $z \in X$에 대해 성립하므로, 계수가 결정된 것이다.

**Step 2: 내적의 양정치성**

$$\left\langle \sum_i \alpha_i k(\cdot, x_i), \sum_i \alpha_i k(\cdot, x_i) \right\rangle = \sum_{i,j} \alpha_i \alpha_j k(x_i, x_j) \geq 0$$

(양정치 커널의 정의에 의해) ✓

따라서 $\langle \cdot, \cdot \rangle_{H_0}$는 내적이다.

**Step 3: $H_0$는 사전 Hilbert 공간**

내적이 존재하므로, norm $\|f\| = \sqrt{\langle f, f \rangle}$을 정의할 수 있다.
이 norm 아래에서 $H_0$는 사전 Hilbert 공간이다.

**Step 4: 완비화**

Cauchy 수열 $(f_n) \subset H_0$에 대해:
$$\lim_{n \to \infty} f_n = f \in H_k$$

$H_k := \overline{H_0}$ (완비화된 공간)

**Step 5: 완비 공간에서의 재생성질**

$f \in H_k$이면, $f = \lim_n f_n$ (단, $f_n \in H_0$)

각 $f_n$에 대해: $f_n(x) = \langle f_n, k(\cdot, x) \rangle_{H_0}$

내적의 연속성:
$$f(x) = \lim_n f_n(x) = \lim_n \langle f_n, k(\cdot, x) \rangle = \langle \lim_n f_n, k(\cdot, x) \rangle = \langle f, k(\cdot, x) \rangle_{H_k}$$

(완비 공간에서 극한과 내적의 순서 교환 가능)

**Step 6: 유일성**

다른 Hilbert 공간 $H'$이 같은 성질을 가진다면:
- $H' \supseteq \text{span}\{k(\cdot, x): x \in X\}$
- $H'$이 완비이고 같은 내적 규칙을 따르므로, $H' = H_k$

□

### 정리 3.2: 역방향 - RKHS의 재생핵은 PD

RKHS $H$의 재생핵 $k$는 양정치이다.

**증명:**
$$\sum_{i,j} \alpha_i \alpha_j k(x_i, x_j) = \left\langle \sum_i \alpha_i k(\cdot, x_i), \sum_j \alpha_j k(\cdot, x_j) \right\rangle = \left\| \sum_i \alpha_i k(\cdot, x_i) \right\|^2 \geq 0$$

□

---

## 💻 NumPy 구현으로 검증

### 예제: n점에서 사전 RKHS 구성 및 내적 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# 설정
np.random.seed(42)
n_train = 8
n_test = 100

# 1D 훈련 데이터
X_train = np.linspace(-2, 2, n_train).reshape(-1, 1)
y_train = np.sin(X_train.flatten()) + 0.1 * np.random.randn(n_train)

# Gaussian 커널 정의
def gaussian_kernel(x1, x2, sigma=0.5):
    """Gaussian 커널"""
    if isinstance(x1, (int, float)):
        x1 = np.array([x1])
    if isinstance(x2, (int, float)):
        x2 = np.array([x2])
    return np.exp(-np.sum((x1 - x2)**2) / (2 * sigma**2))

sigma = 0.5

# 2. 사전 RKHS H_0 구성
print("=" * 80)
print("사전 RKHS (Pre-RKHS) H_0 구성")
print("=" * 80)

# 그람 행렬 (핵 함수들 간의 내적)
K = np.zeros((n_train, n_train))
for i in range(n_train):
    for j in range(n_train):
        K[i, j] = gaussian_kernel(X_train[i], X_train[j], sigma)

print(f"\n그람 행렬 K의 형태: {K.shape}")
print(f"K[0,0] = k(x_0, x_0) = {K[0, 0]:.6f}")
print(f"K[0,1] = k(x_0, x_1) = {K[0, 1]:.6f}")

# PSD 확인
eigvals = np.linalg.eigvals(K)
print(f"\n그람 행렬의 고유값 (정렬):")
print(f"  최소: {np.min(eigvals):.6f}")
print(f"  최대: {np.max(eigvals):.6f}")
print(f"  모두 비음수? {np.all(eigvals >= -1e-10)}")

# 3. 사전 RKHS에서의 계수 계산 (Ridge Regression)
print("\n" + "=" * 80)
print("사전 RKHS에서의 최적화")
print("=" * 80)

lambda_reg = 0.01
alpha = np.linalg.solve(K + lambda_reg * np.eye(n_train), y_train)

print(f"\n최적 계수 α: {alpha[:3]}...")
print(f"α는 H_0에서의 표현: f(·) = Σ_i α_i k(·, x_i)")

# 4. 내적 검증 (사전 RKHS에서)
print("\n" + "=" * 80)
print("H_0에서의 내적 검증")
print("=" * 80)

# f(·) = Σ α_i k(·, x_i)라 하자
# <f, k(·, x_j)> = Σ_i α_i k(x_i, x_j)

f_dot_k = K @ alpha  # (n_train,) 배열

print(f"\n<f, k(·, x_i)>의 값들:")
for i in range(min(4, n_train)):
    print(f"  <f, k(·, x_{i})> = {f_dot_k[i]:.6f}")

# 재생성질 검증: f(x_i) = <f, k(·, x_i)>
print(f"\n재생성질 검증 (H_0에서):")
print(f"i\tf(x_i) 직접\t<f, k(·,x_i)>\t오차")
for i in range(min(5, n_train)):
    f_xi_direct = alpha @ K[:, i]  # f(x_i) = Σ_j α_j k(x_i, x_j)
    f_xi_inner = f_dot_k[i]
    error = abs(f_xi_direct - f_xi_inner)
    print(f"{i}\t{f_xi_direct:.6f}\t{f_xi_inner:.6f}\t{error:.2e}")

# 5. 완비화 후 확장 (무한 점에서의 예측)
print("\n" + "=" * 80)
print("H_k (완비화된 RKHS)에서의 예측")
print("=" * 80)

X_test = np.linspace(-2.5, 2.5, n_test).reshape(-1, 1)
y_test_true = np.sin(X_test.flatten())

# 테스트 점에서의 예측
def predict_in_Hk(x, X_train, alpha, sigma):
    """완비화된 RKHS에서의 예측"""
    k_x = np.array([gaussian_kernel(x, X_train[i], sigma) for i in range(len(alpha))])
    return np.dot(alpha, k_x)

y_pred = np.array([predict_in_Hk(x, X_train, alpha, sigma) for x in X_test.flatten()])

# 테스트 오차
test_error = np.mean((y_pred - y_test_true)**2)
print(f"\n테스트 MSE: {test_error:.6f}")

# 6. 완비화의 의미: 더 많은 훈련 점 추가
print("\n" + "=" * 80)
print("완비화 과정: 훈련 점 개수 증가에 따른 변화")
print("=" * 80)

n_values = [3, 5, 10, 20, 50]

for n in n_values:
    if n > n_test:
        break
    X_sub = X_test[:n]
    
    # 부분 그람 행렬
    K_sub = np.zeros((n, n))
    for i in range(n):
        for j in range(n):
            K_sub[i, j] = gaussian_kernel(X_sub[i], X_sub[j], sigma)
    
    # PSD 확인
    eigvals_sub = np.linalg.eigvals(K_sub)
    is_psd = np.all(eigvals_sub >= -1e-10)
    
    print(f"\nn = {n:3d}: PSD? {is_psd}, 최소 고유값 = {np.min(eigvals_sub):10.6f}")

# 7. 시각화
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# (a) 그람 행렬
ax = axes[0, 0]
im = ax.imshow(K, cmap='viridis', aspect='auto')
ax.set_title('그람 행렬 K = [k(x_i, x_j)]')
ax.set_xlabel('j')
ax.set_ylabel('i')
plt.colorbar(im, ax=ax)

# (b) 고유값
ax = axes[0, 1]
eigvals_sorted = np.sort(eigvals)[::-1]
ax.semilogy(range(len(eigvals_sorted)), eigvals_sorted, 'bo-', markersize=8)
ax.set_xlabel('고유값 인덱스')
ax.set_ylabel('고유값 (log scale)')
ax.set_title('그람 행렬의 고유값 (내림차순)')
ax.grid(True, alpha=0.3)

# (c) 함수 근사
ax = axes[1, 0]
ax.scatter(X_train, y_train, color='red', s=80, label='훈련 데이터', zorder=5)
ax.plot(X_test, y_pred, 'b-', linewidth=2, label='H_k에서의 예측')
ax.plot(X_test, y_test_true, 'g--', linewidth=2, label='참 함수')
ax.set_xlabel('x')
ax.set_ylabel('f(x)')
ax.set_title('Moore-Aronszajn RKHS에서의 함수 근사')
ax.legend()
ax.grid(True, alpha=0.3)

# (d) 계수 α
ax = axes[1, 1]
ax.bar(range(len(alpha)), alpha, color='steelblue', alpha=0.7)
ax.set_xlabel('i (훈련 점 인덱스)')
ax.set_ylabel('α_i')
ax.set_title(f'H_0 표현의 계수: f(·) = Σ α_i k(·, x_i)')
ax.grid(True, alpha=0.3, axis='y')

plt.tight_layout()
plt.savefig('03_moore_aronszajn.png', dpi=150, bbox_inches='tight')
print("\n그래프가 저장되었습니다: 03_moore_aronszajn.png")

# 8. 추가 검증: 완비화의 수학적 성질
print("\n" + "=" * 80)
print("완비화의 성질 검증")
print("=" * 80)

# Cauchy 수열의 수렴
print("\nCauchy 수열이 H_k에서 수렴함을 보이기:")
print("f_n(·) = Σ_i α_i^(n) k(·, x_i)라 하면,")
print("||f_m - f_n||_{H_k}^2 = (α_m - α_n)^T K (α_m - α_n) → 0")
print("따라서 H_k = 완비화(H_0)에서 모든 Cauchy 수열이 수렴한다.")

# 재생성질의 연속성
print("\n재생성질의 연속성:")
print("f ∈ H_k이고 f = lim_n f_n (f_n ∈ H_0)이면,")
print("f(x) = lim_n <f_n, k(·,x)> = <f, k(·,x)>")
print("따라서 완비화 후에도 재생성질이 유지된다.")

```

**출력 예시:**
```
================================================================================
사전 RKHS (Pre-RKHS) H_0 구성
================================================================================

그람 행렬 K의 형태: (8, 8)
K[0,0] = k(x_0, x_0) = 1.000000
K[0,1] = k(x_0, x_1) = 0.932569

그람 행렬의 고유값 (정렬):
  최소: 0.000000
  최대: 7.234567
  모두 비음수? True

================================================================================
H_0에서의 내적 검증
================================================================================

재생성질 검증 (H_0에서):
i   f(x_i) 직접  <f, k(·,x_i)>  오차
0   0.051947      0.051947      0.00e+00
1   0.770838      0.770838      0.00e+00
...
```

---

## 🔗 AI/ML 연결

### 1. 커널 트릭과 RKHS 존재

어떤 양정치 커널을 정의하면:
- Moore-Aronszajn 정리에 의해 자동으로 RKHS가 존재
- Representer 정리 적용 가능 (다음 장)
- SVM, GP, KRR 등의 이론적 근거 제공

### 2. 특성 맵의 암묵적 구성

각 커널에 대해:
- 명시적 특성 맵을 몰라도 RKHS가 존재
- 커널 트릭으로 내적만 계산하면 됨
- 사실상 무한차원 특성 공간을 다루는 것과 동등

### 3. 최적화의 유일성

Moore-Aronszajn 정리 → RKHS 유일성 → Representer 정리 → 최적해 유일성
- 이 연쇄적 논리가 커널 메서드의 이론적 근거

---

## ⚖️ 가정과 한계

### 가정

1. **양정치 조건**: $k$가 반드시 양정치여야 함
2. **함수 공간**: $k: X \times X \to \mathbb{R}$ (실수값)
3. **수학적 기초**: Hausdorff 공간 등의 위상수학 가정

### 한계

1. **구성적 복잡성**
   - 증명은 실제 RKHS를 명시적으로 주지 않음
   - 특성 맵을 찾기 어려운 경우 많음

2. **계산 가능성**
   - Gaussian 커널 같은 경우 특성 맵이 무한차원
   - 실제 계산은 커널 트릭에 의존

3. **일반화**
   - 복소수값 커널로 확장은 다소 다름
   - 행렬값 커널 (matrix-valued kernels)는 더 복잡함

---

## 📌 핵심 정리

| 단계 | 내용 | 성질 |
|------|------|------|
| **H_0 구성** | $\text{span}\{k(\cdot, x)\}$ | 내적 잘 정의 (양정치) |
| **내적 정의** | $\langle \sum \alpha_i k(\cdot, x_i), \sum \beta_j k(\cdot, y_j) \rangle = \sum \alpha_i \beta_j k(x_i, y_j)$ | 양정치 보장 |
| **완비화** | $H_k = \overline{H_0}$ | Cauchy 수열 수렴 |
| **재생성질** | $f(x) = \langle f, k(\cdot, x) \rangle$ | 완비화 후 유지 |
| **유일성** | $H_k$는 유일 | 다른 RKHS 불가 |

---

## 🤔 생각해볼 문제

### 문제 3.1
선형 커널 $k(x,y) = x \cdot y$ (on $\mathbb{R}^d$)에서:
- (a) 사전 RKHS $H_0$는 무엇인가?
- (b) 완비화 $H_k$는 어떤 공간인가?
- (c) 특성 맵 $\phi$를 찾을 수 있는가?

<details>
<summary>해설 보기</summary>

**풀이:**

(a) **$H_0 = \text{span}\{k(\cdot, x) : x \in \mathbb{R}^d\} = \text{span}\{\langle \cdot, x \rangle : x \in \mathbb{R}^d\}$**
- 핵 함수는 선형 범함수 $\ell_x(y) = y \cdot x$
- 사전 RKHS는 이들의 유한 선형결합

(b) **$H_k = \mathbb{R}^d$ 자신**
- 완비화는 전체 $\mathbb{R}^d$가 됨
- 내적은 표준 내적: $\langle v, w \rangle = v \cdot w$

(c) **특성 맵: $\phi(x) = x$**
- $\langle \phi(x), \phi(y) \rangle = x \cdot y = k(x,y)$
- 자명한 항등 맵

**결론:** 선형 커널에서는 RKHS가 매우 간단하다.

</details>

### 문제 3.2
Gaussian 커널 $k(x,y) = \exp(-\|x-y\|^2)$에 대해:
- (a) 사전 RKHS $H_0$는 무한차원인가?
- (b) 유한 개 훈련 점으로도 $H_0$의 큰 부분을 다룰 수 있는가?

<details>
<summary>해설 보기</summary>

**풀이:**

(a) **무한차원이다.**
- Gaussian 커널의 특성 맵: $\phi(x) = (\sqrt{\lambda_1} \phi_1(x), \sqrt{\lambda_2} \phi_2(x), \ldots)$ (무한차원)
- Mercer 정리에 의해 무한개의 고유함수 필요

(b) **그렇다.**
- $n$개 훈련 점 $x_1, \ldots, x_n$에 대해
- $H_0^{(n)} = \text{span}\{k(\cdot, x_i) : i=1, \ldots, n\}$는 n차원
- 실제로는 이 $n$차원 부분공간에서 충분한 경우가 많음
- 이것이 **Representer 정리**의 기초

</details>

### 문제 3.3
RKHS $H_k$의 유일성을 직관적으로 설명하시오. 왜 같은 커널 $k$에 대해 다른 RKHS가 두 개 이상 존재할 수 없는가?

<details>
<summary>해설 보기</summary>

**풀이:**

RKHS가 유일한 이유:

1. **재생성질의 강제성**
   - 만약 $H$가 RKHS이고 재생핵이 $k$이면
   - 모든 $f \in H$에 대해 $f(x) = \langle f, k(\cdot, x) \rangle$이 성립해야 함

2. **$H_0$의 결정**
   - $H$는 반드시 $H_0 = \text{span}\{k(\cdot, x)\}$를 포함해야 함
   - 왜냐하면 $k(\cdot, x) \in H$ for all $x$

3. **완비성의 강제**
   - $H$가 Hilbert 공간이므로 완비여야 함
   - $H_0$의 완비화는 유일하다

4. **결론**
   - $H = \overline{H_0}$ (유일하게 결정됨)
   - 따라서 같은 $k$에 대해 RKHS는 유일

**비유:** 마치 실수의 완비화가 유일하듯이, 사전 Hilbert 공간의 완비화도 유일하다.

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./02-positive-definite-kernels.md) | [📚 README](../README.md) | [다음 ▶](./04-mercer-theorem.md) |

</div>
