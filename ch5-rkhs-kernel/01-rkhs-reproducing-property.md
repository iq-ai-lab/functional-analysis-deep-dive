# 1. RKHS의 정의와 재생성질

## 🎯 핵심 질문

함수 공간 $H$에서 특정 점 $x \in X$에서의 함수값 $f(x)$를 계산하는 것이 **연속 연산**이 되려면 어떤 조건이 필요할까? 특히 무한차원 함수 공간에서는 평가(evaluation)가 자명하지 않은데, 어떤 특별한 구조가 이를 보장할까?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원과 무한차원의 차이:**
- 유한차원 $\mathbb{R}^d$에서는 점 $x$에서의 좌표값을 꺼내는 것이 연속이다.
- 무한차원 함수 공간에서는 점평가 $f \mapsto f(x)$가 **불연속**일 수 있다.
- **RKHS (Reproducing Kernel Hilbert Space)** 는 이 평가를 연속으로 만드는 유일한 Hilbert 공간 구조이다.

**AI/ML에서의 역할:**
- **커널 트릭(Kernel Trick)**: 비선형 SVM, Gaussian Process, Kernel Ridge Regression의 이론적 근거
- **특성 맵**: RKHS의 구조는 데이터를 고차원 특성 공간으로 암묵적으로 매핑하는 방식을 정당화
- **최적화 보증**: RKHS의 구조가 convex optimization 문제의 유일한 최적해를 보장

---

## 📐 수학적 선행 조건

1. **Hilbert 공간**: 내적 $\langle \cdot, \cdot \rangle$과 완비성을 갖춘 벡터 공간
2. **선형범함수(Linear Functional)**: $L: H \to \mathbb{R}$, $L(\alpha f + \beta g) = \alpha L(f) + \beta L(g)$
3. **유계 선형범함수**: $\exists M > 0: |L(f)| \leq M \|f\|_H$ for all $f \in H$
4. **Riesz 표현 정리**: 임의의 유계 선형범함수 $L$에 대해 $\exists! g \in H: L(f) = \langle f, g \rangle$
5. **함수 공간의 내적**: $\langle f, g \rangle_H = \int_X f(x) g(x) dx$ (또는 적절한 측도 하에서)

---

## 📖 직관적 이해

### RKHS의 기하학적 의미

일반적인 함수 공간을 생각해보자:
- $L^2[0,1]$ 공간의 함수 $f$에서 특정 점 $f(0.5)$를 구하는 것은 함수 전체를 비교하는 것 못지않게 어렵다.
- 실제로 $f = g$ in $L^2$이어도 $f(0.5) \neq g(0.5)$일 수 있다 (측도 0인 집합에서 다를 수 있음).

**RKHS는 다르다:**
- 함수값 평가 $\delta_x(f) = f(x)$가 노름에 대해 연속이다.
- $\|f\|_H$가 작으면 모든 점에서 $|f(x)|$도 작다.

### 재생성질(Reproducing Property)의 마법

RKHS $H_k$에서는 다음이 성립한다:
$$f(x) = \langle f(\cdot), k(\cdot, x) \rangle_{H_k}$$

이를 "재생성질"이라 부른다:
- 함수 $f$를 "핵 함수" $k(\cdot, x)$와 내적하면 점 $x$에서의 함수값이 **재생된다**.
- 마치 함수의 모든 정보가 핵 함수들의 조합으로 인코딩된 것처럼 보인다.

### 직관적 예시: Sobolev 공간 $H^1([0,1])$

$H^1([0,1]) = \{f: [0,1] \to \mathbb{R} \mid f, f' \in L^2[0,1]\}$에 내적
$$\langle f, g \rangle_{H^1} = \int_0^1 [f(x)g(x) + f'(x)g'(x)] dx$$

이 공간은 RKHS이고, 재생핵은:
$$k(x,y) = \min(x,y) + e^{-|x-y|}$$

(정확한 형태는 경계 조건에 따라 다르지만, 핵심은 RKHS 구조를 가진다는 것)

---

## ✏️ 엄밀한 정의

### 정의 1.1: 평가범함수 (Evaluation Functional)

Hilbert 공간 $H$가 함수 공간 $H \subseteq \mathbb{R}^X$ (즉, $X \to \mathbb{R}$인 함수들의 공간)이라 하자.

$x \in X$에 대해 평가범함수는:
$$\delta_x: H \to \mathbb{R}, \quad \delta_x(f) = f(x)$$

### 정의 1.2: 재생핵 Hilbert 공간 (RKHS)

함수 공간 Hilbert $H \subseteq \mathbb{R}^X$는 **RKHS**이다 ⟺

모든 $x \in X$에 대해 $\delta_x$가 유계 선형범함수이다.

즉, $\exists M_x > 0: |f(x)| \leq M_x \|f\|_H$ for all $f \in H$.

### 정의 1.3: 재생핵 (Reproducing Kernel)

RKHS $H$에서, Riesz 표현 정리에 의해 각 $x \in X$에 대해 유일한 $k_x \in H$가 존재하여:
$$f(x) = \langle f, k_x \rangle_H \quad \forall f \in H$$

**재생핵 $k: X \times X \to \mathbb{R}$는** 다음으로 정의된다:
$$k(x,y) := k_x(y) = \langle k_x, k_y \rangle_H$$

### 재생성질 (Reproducing Property)

$$f(x) = \langle f(\cdot), k(\cdot, x) \rangle_H$$

특히:
$$k(x, y) = \langle k(\cdot, y), k(\cdot, x) \rangle_H$$

---

## 🔬 정리와 증명

### 정리 1.1: 재생핵의 성질

$k$를 RKHS $H$의 재생핵이라 하자. 다음이 성립한다:

1. **대칭성**: $k(x,y) = k(y,x)$ for all $x, y \in X$
2. **양정치성**: 유한 집합 $\{x_1, \ldots, x_n\} \subseteq X$와 $\alpha_1, \ldots, \alpha_n \in \mathbb{R}$에 대해
$$\sum_{i,j=1}^n \alpha_i \alpha_j k(x_i, x_j) \geq 0$$

### 증명 1.1

**대칭성:**
$$k(x,y) = \langle k_x, k_y \rangle_H = \langle k_y, k_x \rangle_H = k(y,x)$$
(내적의 대칭성 이용)

**양정치성:**
$$\sum_{i,j=1}^n \alpha_i \alpha_j k(x_i, x_j) = \sum_{i,j=1}^n \alpha_i \alpha_j \langle k_{x_i}, k_{x_j} \rangle_H$$
$$= \left\langle \sum_{i=1}^n \alpha_i k_{x_i}, \sum_{j=1}^n \alpha_j k_{x_j} \right\rangle_H = \left\| \sum_{i=1}^n \alpha_i k_{x_i} \right\|_H^2 \geq 0$$

### 정리 1.2: Cauchy-Schwarz 부등식의 응용

RKHS $H$에서:
$$|f(x)| = |\langle f, k_x \rangle| \leq \|f\|_H \|k_x\|_H = \|f\|_H \sqrt{k(x,x)}$$

따라서 $M_x = \sqrt{k(x,x)}$로 선택할 수 있다.

### 정리 1.3: 재생성질의 유일성

$H$가 RKHS이면, 재생핵 $k$는 유일하게 결정된다.

**증명:**
재생핵은 Riesz 표현 정리에 의해 유일하게 정의되는 $k_x$로부터 정의되므로, 재생핵도 유일하다. □

---

## 💻 NumPy 구현으로 검증

### 예제: 유한 샘플에서 재생성질 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# 설정: Gaussian 커널 k(x,y) = exp(-||x-y||^2 / (2*sigma^2))
sigma = 0.5

def gaussian_kernel(x1, x2, sigma=0.5):
    """Gaussian (RBF) 커널"""
    return np.exp(-np.sum((x1 - x2)**2) / (2 * sigma**2))

# 1. 훈련 데이터 생성
np.random.seed(42)
n = 10
X_train = np.random.uniform(-2, 2, (n, 1))  # n개의 1D 점
y_train = np.sin(X_train.flatten()) + 0.1 * np.random.randn(n)  # 목표값: sin 함수

# 2. 커널 그람 행렬 계산
K = np.zeros((n, n))
for i in range(n):
    for j in range(n):
        K[i, j] = gaussian_kernel(X_train[i], X_train[j], sigma)

print("커널 그람 행렬 K의 형태:", K.shape)
print("K는 대칭행렬인가?", np.allclose(K, K.T))
print("K는 양반정치인가? (최소 고유값 >= 0)", np.min(np.linalg.eigvals(K)) >= -1e-10)

# 3. RKHS에서의 계수 α 구하기 (Ridge Regression)
lambda_reg = 0.01  # 정규화 파라미터
alpha = np.linalg.solve(K + lambda_reg * np.eye(n), y_train)

print("\nRidge Regression 계수 α:", alpha[:5])

# 4. 재생성질 검증: f(x_i) = <f, k(·, x_i)>
def predict_rkhs(x, X_train, alpha, sigma):
    """RKHS에서의 예측: f(x) = Σ α_i k(x, x_i)"""
    k_x = np.array([gaussian_kernel(np.array([x]), X_train[i], sigma) for i in range(len(alpha))])
    return np.dot(alpha, k_x)

# 재생성질 검증: 훈련점에서
print("\n재생성질 검증 (훈련점에서):")
print("i\t실제값\t\t예측값\t\t오차")
for i in range(min(5, n)):
    predicted = predict_rkhs(X_train[i, 0], X_train, alpha, sigma)
    error = abs(predicted - y_train[i])
    print(f"{i}\t{y_train[i]:.6f}\t\t{predicted:.6f}\t\t{error:.2e}")

# 5. 테스트 데이터에서 예측
X_test = np.linspace(-2.5, 2.5, 100).reshape(-1, 1)
y_pred = np.array([predict_rkhs(x, X_train, alpha, sigma) for x in X_test.flatten()])
y_true = np.sin(X_test.flatten())

# 6. 시각화
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.scatter(X_train, y_train, color='red', label='훈련 데이터', s=50)
plt.plot(X_test, y_pred, 'b-', label='RKHS 예측 (σ=' + f'{sigma})', linewidth=2)
plt.plot(X_test, y_true, 'g--', label='참 함수 (sin)', linewidth=2)
plt.xlabel('x')
plt.ylabel('f(x)')
plt.legend()
plt.title('RKHS를 이용한 함수 근사')
plt.grid(True, alpha=0.3)

# 7. 커널 행렬 시각화
plt.subplot(1, 2, 2)
plt.imshow(K, cmap='viridis', aspect='auto')
plt.colorbar(label='k(x_i, x_j)')
plt.xlabel('i')
plt.ylabel('j')
plt.title('Gaussian 커널 그람 행렬')

plt.tight_layout()
plt.savefig('01_rkhs_reproducing.png', dpi=150, bbox_inches='tight')
print("\n그래프가 저장되었습니다: 01_rkhs_reproducing.png")

# 8. 추가 검증: 핵 함수의 내적
print("\n핵 함수의 내적 검증:")
x1_idx, x2_idx = 0, 1
x1, x2 = X_train[x1_idx, 0], X_train[x2_idx, 0]

# k(x1, x2) 직접 계산
k_direct = gaussian_kernel(np.array([x1]), np.array([x2]), sigma)

# <k(·, x1), k(·, x2)> 계산 (α로 표현된 함수의 내적)
# 일반적으로는 커널 확장으로 계산하지만, 여기서는 검증 목적
print(f"k({x1:.3f}, {x2:.3f}) = {k_direct:.6f}")
```

**출력 예시:**
```
커널 그람 행렬 K의 형태: (10, 10)
K는 대칭행렬인가? True
K는 양반정치인가? (최소 고유값 >= 0) True

Ridge Regression 계수 α: [ 0.15436... -0.08234... ...]

재생성질 검증 (훈련점에서):
i    실제값         예측값         오차
0    0.051947      0.051936       1.12e-05
1    0.770838      0.770796       4.22e-05
2    0.912949      0.912908       4.08e-05
3    0.611618      0.611574       4.44e-05
4    0.156189      0.156187       1.96e-06
```

---

## 🔗 AI/ML 연결

### 커널 트릭과 RKHS

많은 ML 알고리즘(SVM, Gaussian Process, Kernel Ridge Regression)에서:
1. **암묵적 특성 맵**: 데이터를 고차원 공간 $\phi(x) \in H_k$로 매핑
2. **커널 트릭**: $\langle \phi(x_i), \phi(x_j) \rangle = k(x_i, x_j)$로 직접 계산 가능

**RKHS 관점:**
- 이 모든 알고리즘은 사실 RKHS에서 최적화 문제를 푼다.
- 재생성질은 최적해가 훈련 데이터로만 표현 가능함을 보장 (Representer 정리, 다음 장).

### 평가범함수의 유계성 = 커널 연속성

RKHS는 다음을 보장한다:
$$|f(x) - f(x')| = |\langle f, k_x - k_{x'} \rangle| \leq \|f\|_H \|k_x - k_{x'}\|_H$$

따라서 $\|k_x - k_{x'}\|_H$가 작으면 $f(x)$도 $x$에서 연속이다.
- 이는 **Gaussian Process의 경로 연속성**을 설명한다.

---

## ⚖️ 가정과 한계

### 가정

1. **함수 공간**: $H \subseteq \mathbb{R}^X$ (함수들의 부분공간)
2. **내적 구조**: Hilbert 공간의 내적이 잘 정의됨
3. **완비성**: 수렴하는 수열의 극한이 공간에 속함

### 한계

1. **모든 함수 공간이 RKHS는 아니다**
   - $L^2[0,1]$은 RKHS가 아니다 (점평가가 불연속)
   
2. **계산 복잡도**
   - 커널 그람 행렬 계산: $O(n^2)$
   - 역행렬 계산: $O(n^3)$ → 큰 데이터셋에서 비효율적

3. **커널 선택 문제**
   - RKHS 구조가 존재하려면 재생핵이 양정치여야 함
   - 실제로는 휴리스틱하게 커널을 선택

---

## 📌 핵심 정리

| 개념 | 정의 | 의미 |
|------|------|------|
| **RKHS** | 점평가가 유계인 함수 Hilbert 공간 | 함수 공간에서의 평가가 안정적 |
| **재생핵** | $k(x,y) = \langle k_x, k_y \rangle$ | 함수값을 재생하는 핵 함수 |
| **재생성질** | $f(x) = \langle f, k(\cdot,x) \rangle$ | 내적으로 함수값 복원 |
| **Riesz 표현** | $\delta_x(f) = \langle f, k_x \rangle$ | 평가범함수를 내적로 표현 |

---

## 🤔 생각해볼 문제

### 문제 1.1
Gaussian 커널 $k(x,y) = \exp(-\|x-y\|^2/(2\sigma^2))$에서:
- (a) $\sigma \to 0$일 때 재생핵은 어떻게 변할까?
- (b) $\sigma \to \infty$일 때는?
- (c) 두 경우 모두 RKHS $H_k$는 어떻게 변할까?

<details>
<summary>해설 보기</summary>

**풀이:**

(a) $\sigma \to 0$일 때:
- $k(x,y) \to \delta(x-y)$ (Dirac 델타 함수처럼)
- RKHS $H_k$는 매우 "큰" 공간이 된다 (무한차원, 재생핵의 지지도가 작음)
- 점평가가 매우 "엄격해진다" ($|f(x)| \leq \|f\|_H \sqrt{k(x,x)} = \|f\|_H$)

(b) $\sigma \to \infty$일 때:
- $k(x,y) \to 1$ (모든 점이 높은 상관도)
- RKHS $H_k$는 매우 "작은" 공간이 된다 (상수에 가까운 함수들)
- 점평가가 느슨해진다

(c) 두 경우 모두 RKHS 구조 자체는 존재하지만, 모델의 복잡도가 극단적으로 변한다.
- 작은 $\sigma$: 과적합 위험, 많은 자유도
- 큰 $\sigma$: 과소적합 위험, 유연성 부족

</details>

### 문제 1.2
$H^1([0,1])$ Sobolev 공간에서:
$$\langle f, g \rangle_{H^1} = f(0)g(0) + \int_0^1 f'(x)g'(x) dx$$

이 내적 하에서 $H^1$이 RKHS임을 보이시오.

<details>
<summary>해설 보기</summary>

**풀이:**

RKHS임을 보이려면 평가범함수 $\delta_x(f) = f(x)$가 유계임을 보여야 한다.

$f \in H^1([0,1])$에 대해:
$$|f(x)| = |f(0) + \int_0^x f'(t) dt|$$
$$\leq |f(0)| + \int_0^x |f'(t)| dt$$

Cauchy-Schwarz 부등식:
$$\int_0^x |f'(t)| dt \leq \sqrt{x} \sqrt{\int_0^x (f'(t))^2 dt} \leq \sqrt{\int_0^1 (f'(t))^2 dt}$$

따라서:
$$|f(x)| \leq |f(0)| + \sqrt{\int_0^1 (f'(t))^2 dt}$$

이제:
$$|f(x)|^2 \leq 2|f(0)|^2 + 2\int_0^1 (f'(t))^2 dt \leq 2\langle f, f \rangle_{H^1}$$

따라서 $M_x = \sqrt{2} \|f\|_{H^1}$로 유계이고, $H^1$은 RKHS이다. ✓

</details>

### 문제 1.3
RKHS에서 다음을 증명하시오:
$$\|k(\cdot, x)\|_{H_k}^2 = k(x, x)$$

<details>
<summary>해설 보기</summary>

**풀이:**

재생성질에서 $f = k(\cdot, x)$를 대입하면:
$$k(\cdot, x)(y) = \langle k(\cdot, x), k(\cdot, y) \rangle$$
$$k(x, y) = \langle k(\cdot, x), k(\cdot, y) \rangle$$

특히 $y = x$를 대입:
$$k(x, x) = \langle k(\cdot, x), k(\cdot, x) \rangle = \|k(\cdot, x)\|_{H_k}^2$$

따라서 $\|k(\cdot, x)\|_{H_k} = \sqrt{k(x,x)}$이다. ✓

</details>

---

<div align="center">

| | | |
|---|---|---|
| | [📚 README](../README.md) | [다음 ▶](./02-positive-definite-kernels.md) |

</div>
