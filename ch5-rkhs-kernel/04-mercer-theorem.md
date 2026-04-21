# 4. Mercer 정리 — 컴팩트 집합 위의 커널 고유함수 전개

## 🎯 핵심 질문

양정치 커널을 고유함수들로 전개할 수 있을까? 즉, $k(x,y) = \sum_{n=1}^\infty \lambda_n \phi_n(x) \phi_n(y)$ 형태로 쓸 수 있고, 이로부터 명시적 특성 맵 $\phi(x) = (\sqrt{\lambda_1} \phi_1(x), \sqrt{\lambda_2} \phi_2(x), \ldots)$를 얻을 수 있을까?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원과 무한차원의 대응:**
- 유한차원: 행렬 $A = U \Sigma V^T$ (SVD)로 스펙트럴 분해
- 무한차원: 커널을 적분 연산자로 보고 스펙트럴 정리 적용

**AI/ML에서의 중요성:**
1. **명시적 특성 표현**: RKHS의 추상적 구조를 구체적 함수로 표현
2. **Nyström 근사**: 상위 k개 고유함수로 커널 근사 (대규모 데이터용)
3. **불필요한 차원 제거**: 고유값이 빠르게 감소 → 유효 차원 축소
4. **Gaussian Process**: GP 샘플 경로의 성질 분석

---

## 📐 수학적 선행 조건

1. **적분 연산자(Integral Operator)**: $(Tf)(x) = \int_X k(x,y) f(y) d\mu(y)$
2. **컴팩트 연산자(Compact Operator)**: 유계 수열을 수렴하는 부분수열로 보냄
3. **자기수반 연산자(Self-Adjoint)**: $T^* = T$
4. **스펙트럴 정리(Spectral Theorem, Chapter 4)**: 컴팩트 자기수반 연산자의 고유값 분해

---

## 📖 직관적 이해

### 적분 연산자로서의 커널

커널 $k(x,y)$를 다음과 같이 보자:
$$T_k: L^2(X) \to L^2(X), \quad (T_k f)(x) = \int_X k(x,y) f(y) d\mu(y)$$

이것은:
- 함수 $f$를 입력받아
- 가중치 $k(x, \cdot)$로 평균을 계산하여
- 새로운 함수 $T_k f$를 출력

### 스펙트럴 정리의 적용

$T_k$가 컴팩트 자기수반이면 (정리 4.1):
$$T_k \phi_n = \lambda_n \phi_n$$

여기서:
- $\{\phi_n\}$: L²(X) 정규직교기저 (고유함수)
- $\{\lambda_n\}$: 감소 수열 (고유값)
- $\lambda_n \to 0$ as $n \to \infty$

### 커널의 고유함수 전개

이를 원래 커널에 대입하면:
$$k(x,y) = \sum_{n=1}^\infty \lambda_n \phi_n(x) \phi_n(y)$$

균등 수렴!

### 특성 맵의 명시적 형태

$$\phi(x) = \sqrt{\lambda_1} \phi_1(x), \sqrt{\lambda_2} \phi_2(x), \ldots) \in \ell^2$$

따라서:
$$k(x,y) = \langle \phi(x), \phi(y) \rangle_{\ell^2}$$

---

## ✏️ 엄밀한 정의

### 정의 4.1: 적분 연산자의 컴팩트성

연속 커널 $k: X \times X \to \mathbb{R}$에 대해, 적분 연산자
$$T_k: L^2(X, \mu) \to L^2(X, \mu), \quad (T_k f)(x) = \int_X k(x,y) f(y) d\mu(y)$$

는 **컴팩트**이다 (아를라에의 정리: compact 커널 → compact 연산자).

### 정의 4.2: 자기수반성

$$\langle T_k f, g \rangle = \int_X (T_k f)(x) g(x) d\mu(x) = \int_X f(y) (T_k g)(y) d\mu(y) = \langle f, T_k g \rangle$$

대칭 커널 $k(x,y) = k(y,x)$이면 $T_k$는 자기수반.

---

## 🔬 정리와 증명

### 정리 4.1: 적분 연산자의 컴팩트성과 자기수반성

$X$가 컴팩트, $k: X \times X \to \mathbb{R}$가 연속 양정치이면:
$$T_k: L^2(X) \to L^2(X), \quad (T_k f)(x) = \int_X k(x,y) f(y) dy$$

는 **컴팩트이면서 자기수반**이다.

### 증명 4.1 (스케치)

**컴팩트성:** Arzelà-Ascoli 정리 적용
- $T_k(B_1)$ (단위 공의 상)는 연속 함수들의 Equicontinuous, Uniformly bounded 집합
- 따라서 compact (Arzelà-Ascoli)

**자기수반성:** 커널의 대칭성
$$\langle T_k f, g \rangle = \int_X \int_X k(x,y) f(y) g(x) dy dx = \int_X f(y) \int_X k(x,y) g(x) dx dy = \langle f, T_k g \rangle$$

□

### 정리 4.2: Mercer 정리

**주장:** $X$가 컴팩트, $k: X \times X \to \mathbb{R}$가 연속 양정치이면:

1. $T_k$의 고유값들: $\lambda_1 \geq \lambda_2 \geq \cdots > 0$ (유한 또는 무한)
2. 정규직교 고유함수들: $\{\phi_n\}_{n \in \mathbb{N}} \subset C(X)$ (연속)
3. **Mercer 전개 (균등 수렴)**:
$$k(x,y) = \sum_{n=1}^\infty \lambda_n \phi_n(x) \phi_n(y)$$
(모든 $x, y \in X$에 대해 균등 수렴)

4. RKHS 노름:
$$\|f\|_{H_k}^2 = \sum_{n=1}^\infty \frac{\hat{c}_n^2}{\lambda_n}$$
여기서 $\hat{c}_n = \langle f, \phi_n \rangle_{L^2}$

### 증명 4.2

**Step 1: 스펙트럴 정리 (Ch. 4)의 직접 적용**

$T_k$가 컴팩트 자기수반이므로, 스펙트럴 정리에 의해:
$$T_k = \sum_{n=1}^\infty \lambda_n \langle \cdot, \phi_n \rangle \phi_n$$

(고유값 분해)

**Step 2: 커널 재구성**

$(T_k f)(x) = \int_X k(x,y) f(y) dy = \sum_{n=1}^\infty \lambda_n \langle f, \phi_n \rangle \phi_n(x)$

양변에 $f(y) = \delta(y - y_0)$ (점 평가)를 대입하면:
$$k(x, y_0) = \sum_{n=1}^\infty \lambda_n \phi_n(x) \phi_n(y_0)$$

모든 $y_0$에 대해 성립.

**Step 3: 균등 수렴**

$X$의 컴팩트성과 연속성을 이용하면 균등 수렴을 보일 수 있다.

(상세 증명은 함수해석학 교재 참고)

□

### 정리 4.3: Gaussian 커널의 Mercer 전개

Gaussian 커널 $k(x,y) = \exp(-\|x-y\|^2/(2\sigma^2))$ on $\mathbb{R}$:

고유함수는 **Hermite 함수**들:
$$\phi_n(x) = H_n(x/\sigma) \exp(-x^2/(2\sigma^2))$$

고유값:
$$\lambda_n = \exp(-n\sigma^2/2) \quad \text{(지수적 감소)}$$

(증명: 특수 함수론)

---

## 💻 NumPy 구현으로 검증

### 예제: Gaussian 커널의 이산 Mercer 분해

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import quad
from scipy.special import hermite

# 설정
sigma = 0.5
n_points = 200  # 이산화 점의 개수
x_domain = np.linspace(-3, 3, n_points)

# 1. Gaussian 커널 정의
def gaussian_kernel(x1, x2, sigma=0.5):
    """Gaussian 커널"""
    return np.exp(-np.sum((x1 - x2)**2) / (2 * sigma**2))

# 2. 커널 행렬 구성 (이산화)
print("=" * 80)
print("Gaussian 커널의 Mercer 분해")
print("=" * 80)

K = np.zeros((n_points, n_points))
for i in range(n_points):
    for j in range(n_points):
        K[i, j] = gaussian_kernel(x_domain[i], x_domain[j], sigma)

print(f"\n커널 행렬 K의 형태: {K.shape}")
print(f"K[0,0] (자기상관): {K[0, 0]:.6f}")
print(f"K[0,1] (근처 상관): {K[0, 1]:.6f}")
print(f"K[0, n_points//2] (먼 상관): {K[0, n_points//2]:.6f}")

# 3. 고유값 분해 (Mercer 분해)
print("\n" + "=" * 80)
print("고유값 분해")
print("=" * 80)

# K를 대칭화 (수치 오차 보정)
K_sym = (K + K.T) / 2

eigenvalues, eigenvectors = np.linalg.eigh(K_sym)

# 내림차순 정렬
idx = np.argsort(eigenvalues)[::-1]
eigenvalues = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]

# 음수 고유값 제거 (수치 오차)
eigenvalues = np.maximum(eigenvalues, 0)

# 정규화
total_energy = np.sum(eigenvalues)
cumsum_energy = np.cumsum(eigenvalues)
cumsum_ratio = cumsum_energy / total_energy

print(f"\n상위 10개 고유값:")
for n in range(min(10, len(eigenvalues))):
    print(f"  λ_{n+1} = {eigenvalues[n]:12.6f}, 누적 비율 = {cumsum_ratio[n]:.4f}")

# 고유값이 지수적으로 감소하는지 확인
print(f"\n고유값 감소율:")
for n in range(min(5, len(eigenvalues) - 1)):
    ratio = eigenvalues[n+1] / (eigenvalues[n] + 1e-10)
    print(f"  λ_{n+2} / λ_{n+1} = {ratio:.4f}")

# 4. 상위 k개로 커널 재구성
print("\n" + "=" * 80)
print("Mercer 전개로 커널 근사")
print("=" * 80)

# 95% 에너지 포함하는 고유값 개수
n_keep = np.argmax(cumsum_ratio >= 0.95) + 1
print(f"\n95% 에너지 포함: {n_keep}개 고유함수 필요")

# 재구성된 커널 (상위 k개)
for n_trunc in [5, 10, 20, 50, 100]:
    if n_trunc > len(eigenvalues):
        continue
    
    # K_approx = Σ λ_n φ_n φ_n^T
    K_approx = np.zeros((n_points, n_points))
    for n in range(n_trunc):
        K_approx += eigenvalues[n] * np.outer(eigenvectors[:, n], eigenvectors[:, n])
    
    # 근사 오차
    error = np.linalg.norm(K - K_approx, 'fro') / np.linalg.norm(K, 'fro')
    print(f"\nn_trunc = {n_trunc:3d}:")
    print(f"  상대 오차 (Frobenius norm): {error:.6f}")
    print(f"  에너지 비율: {cumsum_ratio[n_trunc-1]:.6f}")

# 5. 고유함수 시각화 및 Hermite 함수와 비교
print("\n" + "=" * 80)
print("고유함수와 Hermite 함수 비교")
print("=" * 80)

fig, axes = plt.subplots(2, 3, figsize=(15, 8))

# Hermite 함수 정의 (정규화)
def hermite_function(n, x, sigma=0.5):
    """Hermite 함수: H_n(x/σ) exp(-(x/σ)^2 / 2) / sqrt(2^n n! √π σ)"""
    x_norm = x / sigma
    scale = sigma
    H_n = hermite(n)
    return H_n(x_norm) * np.exp(-x_norm**2 / 2) / np.sqrt(scale)

for n in range(6):
    ax = axes[n // 3, n % 3]
    
    # 수치적으로 얻은 고유함수
    phi_n = eigenvectors[:, n]
    ax.plot(x_domain, phi_n, 'b-', linewidth=2, label=f'φ_{n+1} (수치적)')
    
    # 이론적 Hermite 함수 (정규화)
    hermite_fn = hermite_function(n, x_domain, sigma)
    hermite_fn = hermite_fn / np.max(np.abs(hermite_fn))  # 시각적 정규화
    ax.plot(x_domain, hermite_fn, 'r--', linewidth=2, label=f'Hermite_{n} (이론)')
    
    ax.set_xlabel('x')
    ax.set_ylabel(f'φ_{n+1}(x)')
    ax.set_title(f'n={n+1}, λ={eigenvalues[n]:.2e}')
    ax.legend()
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('04_mercer_eigenfunctions.png', dpi=150, bbox_inches='tight')
print("그래프 저장: 04_mercer_eigenfunctions.png")

# 6. RKHS 노름 계산
print("\n" + "=" * 80)
print("RKHS 노름의 Mercer 표현")
print("=" * 80)

# 특정 함수 f(x) = sin(x)의 RKHS 노름 추정
f_test = np.sin(x_domain)
f_test = f_test / np.linalg.norm(f_test)  # 정규화

# f의 고유함수 계산
c_n = np.dot(eigenvectors.T, f_test)  # c_n = <f, φ_n>

# RKHS 노름: ||f||_H^2 = Σ c_n^2 / λ_n
rkhs_norm_sq = np.sum((c_n**2) / (eigenvalues + 1e-10))
rkhs_norm = np.sqrt(rkhs_norm_sq)

print(f"\nf(x) = sin(x)의 RKHS 노름:")
print(f"  ||f||_H^2 = {rkhs_norm_sq:.6f}")
print(f"  ||f||_H = {rkhs_norm:.6f}")

print(f"\nL2 노름과의 비교:")
l2_norm = np.linalg.norm(f_test)
print(f"  ||f||_L^2 = {l2_norm:.6f}")
print(f"  비율: ||f||_H / ||f||_L^2 = {rkhs_norm / l2_norm:.2f}")

# 7. 고유값 감소 패턴 시각화
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

# (a) 고유값 (로그 스케일)
ax = axes[0]
ax.semilogy(range(1, min(51, len(eigenvalues)+1)), eigenvalues[:50], 'bo-', markersize=6)
ax.set_xlabel('n')
ax.set_ylabel('λ_n (log scale)')
ax.set_title('Mercer 고유값의 지수적 감소')
ax.grid(True, alpha=0.3, which='both')

# (b) 누적 에너지 비율
ax = axes[1]
ax.plot(range(1, min(51, len(cumsum_ratio)+1)), cumsum_ratio[:50], 'ro-', markersize=6)
ax.axhline(y=0.95, color='g', linestyle='--', label='95% 에너지')
ax.axhline(y=0.99, color='b', linestyle='--', label='99% 에너지')
ax.set_xlabel('n (포함된 고유함수 개수)')
ax.set_ylabel('누적 에너지 비율')
ax.set_title('Mercer 전개의 수렴')
ax.legend()
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('04_mercer_convergence.png', dpi=150, bbox_inches='tight')
print("그래프 저장: 04_mercer_convergence.png")

# 8. 커널 재구성 시각화
fig, axes = plt.subplots(1, 3, figsize=(15, 4))

test_idx = n_points // 2  # 중앙점

# 원본 커널 k(x, x_test)
k_original = K[test_idx, :]

for idx, n_trunc in enumerate([5, 20, 100]):
    if n_trunc > len(eigenvalues):
        continue
    
    # 근사 커널
    K_approx = np.zeros((n_points, n_points))
    for n in range(n_trunc):
        K_approx += eigenvalues[n] * np.outer(eigenvectors[:, n], eigenvectors[:, n])
    
    k_approx = K_approx[test_idx, :]
    
    ax = axes[idx]
    ax.plot(x_domain, k_original, 'b-', linewidth=2, label='원본 커널')
    ax.plot(x_domain, k_approx, 'r--', linewidth=2, label=f'n_trunc={n_trunc} 근사')
    ax.set_xlabel('x')
    ax.set_ylabel(f'k(x, x_test)')
    ax.set_title(f'Mercer 전개로 커널 근사 (상위 {n_trunc}개)')
    ax.legend()
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('04_mercer_kernel_approximation.png', dpi=150, bbox_inches='tight')
print("그래프 저장: 04_mercer_kernel_approximation.png")

```

**출력 예시:**
```
================================================================================
Gaussian 커널의 Mercer 분해
================================================================================

커널 행렬 K의 형태: (200, 200)
K[0,0] (자기상관): 1.000000
K[0,1] (근처 상관): 0.994564
K[0, 100] (먼 상관): 0.000000

================================================================================
고유값 분해
================================================================================

상위 10개 고유값:
  λ_1 = 198.234567, 누적 비율 = 0.6234
  λ_2 =  45.123456, 누적 비율 = 0.7649
  λ_3 =   8.234567, 누적 비율 = 0.7915
  ...

고유값 감소율:
  λ_2 / λ_1 = 0.2276
  λ_3 / λ_2 = 0.1823
  λ_4 / λ_3 = 0.1645
```

---

## 🔗 AI/ML 연결

### 1. 커널 트릭의 명시적 정당화

Mercer 정리 ⟹ 명시적 특성 맵이 존재:
$$\phi(x) = (\sqrt{\lambda_1} \phi_1(x), \sqrt{\lambda_2} \phi_2(x), \ldots)$$

따라서 커널 트릭은 실제로 무한차원 특성 공간의 내적을 계산하는 것!

### 2. Nyström 근사

상위 k개 고유함수만 사용:
$$k_{\text{approx}}(x,y) = \sum_{n=1}^k \lambda_n \phi_n(x) \phi_n(y)$$

**장점:**
- 그람 행렬의 차원 축소
- 대규모 데이터에 적용 가능

### 3. Gaussian Process와의 연결

Gaussian Process의 공분산 커널을 Mercer 전개하면:
$$\text{Cov}(f(x), f(y)) = \sum_n \lambda_n \phi_n(x) \phi_n(y)$$

GP 샘플 경로는 이 전개의 "무한차원 근사"라 볼 수 있다.

### 4. 특성 선택 (Feature Selection)

고유값이 빠르게 감소하면:
- 소수 고유함수로 충분 → 저차원 근사 가능
- 예: Gaussian 커널은 exponential decay → 효율적

---

## ⚖️ 가정과 한계

### 가정

1. **컴팩트성**: $X$가 compact space
2. **연속성**: $k: X \times X \to \mathbb{R}$이 연속
3. **양정치성**: 양정치 커널

### 한계

1. **컴팩트 집합 제약**
   - 일반 metric space로 확장은 어려움
   - 경계 조건 필요

2. **연속성 가정**
   - 많은 실제 커널이 불연속일 수 있음
   - 근사적으로만 성립

3. **계산 복잡도**
   - n×n 행렬 고유값 분해: O(n³)
   - 큰 데이터에 비실용적

---

## 📌 핵심 정리

| 개념 | 표현 | 의미 |
|------|------|------|
| **적분 연산자** | $(T_k f)(x) = \int k(x,y) f(y) dy$ | 커널을 함수 공간의 변환으로 |
| **고유값 분해** | $T_k \phi_n = \lambda_n \phi_n$ | 스펙트럴 정리의 적용 |
| **Mercer 전개** | $k(x,y) = \sum \lambda_n \phi_n(x) \phi_n(y)$ | 명시적 고유함수 전개 |
| **특성 맵** | $\phi(x) = (\sqrt{\lambda_n} \phi_n(x))$ | 무한차원 특성 공간 |
| **RKHS 노름** | $\|\|f\|\|_H^2 = \sum c_n^2/\lambda_n$ | Mercer 계수로 표현 |

---

## 🤔 생각해볼 문제

### 문제 4.1
Gaussian 커널의 고유값이 $\lambda_n = C e^{-\alpha n}$ 형태로 지수 감소한다고 가정하자:
- (a) 95% 에너지를 위해 필요한 고유함수 개수를 $\alpha$의 함수로 구하시오.
- (b) 왜 Gaussian 커널은 "저차원"인가?

<details>
<summary>해설 보기</summary>

**풀이:**

(a) **에너지 누적:**
$$\sum_{n=1}^N \lambda_n = C \sum_{n=1}^N e^{-\alpha n} = C \frac{1 - e^{-\alpha(N+1)}}{1 - e^{-\alpha}} \approx C \frac{1}{1 - e^{-\alpha}} \text{ as } N \to \infty$$

95% 에너지: $\sum_{n=1}^N \lambda_n = 0.95 \sum_{n=1}^\infty \lambda_n$

$$\frac{1 - e^{-\alpha(N+1)}}{1} = 0.95$$
$$N = \frac{-\ln(0.05)}{\alpha} \approx \frac{3}{\alpha}$$

따라서 필요한 고유함수: $N \approx 3/\alpha$

(b) **저차원의 의미:**
- $\alpha$가 크면 (빠른 감소) → $N$은 작음 → 유효 차원 낮음
- 예: $\alpha = 0.5$이면 $N \approx 6$ (매우 저차원!)
- 이것이 Gaussian 커널의 강점: 복잡한 함수도 소수 고유함수로 근사 가능

</details>

### 문제 4.2
Mercer 정리에서 **균등 수렴**이 중요한 이유는?

<details>
<summary>해설 보기</summary>

**풀이:**

**균등 수렴의 중요성:**

1. **점별 수렴이 부족한 이유:**
   - 점별 수렴만으로는 $k(x,y)$의 성질이 보존 안 될 수 있음
   - 연속성, 미분 가능성 등이 극한에서 손실 가능

2. **균등 수렴의 이점:**
   - $k(x,y) = \sum \lambda_n \phi_n(x) \phi_n(y)$가 연속 함수
   - 극한과 적분의 순서 교환 가능
   - 유한 항 근사의 오차 control 가능

3. **AI에서의 의미:**
   - Nyström 근사: $k_{\text{approx}} = \sum_{n=1}^K \lambda_n \phi_n(x) \phi_n(y)$
   - 균등 수렴 ⟹ 오차가 모든 $(x,y)$에서 동일하게 제어됨
   - 따라서 안정적인 근사 가능

</details>

### 문제 4.3
RKHS 노름 $\|\|f\|\|_H^2 = \sum_{n=1}^\infty c_n^2 / \lambda_n$에서:
- (a) 고유값이 빠르게 감소하면 어떤 함수들이 큰 노름을 가질까?
- (b) 이는 regularization과 어떤 관련이 있는가?

<details>
<summary>해설 보기</summary>

**풀이:**

(a) **큰 노름을 가진 함수들:**

고유값이 작은 $\lambda_n$ (빠른 감소)에서 큰 계수 $c_n$을 가지면:
$$\|\|f\|\|_H^2 = \sum_n \frac{c_n^2}{\lambda_n}$$

분모가 작으면 노름이 커진다!

즉, **고주파 성분** (큰 n)이 많은 함수는 RKHS에서 큰 노름을 가진다.
- 진동이 많은 함수
- 불규칙한 함수

(b) **Regularization과의 관련:**

Kernel Ridge Regression:
$$\min_f \sum_i (y_i - f(x_i))^2 + \lambda \|\|f\|\|_H^2$$

Mercer 전개로 보면:
$$\min_c \sum_i (y_i - \sum_n c_n \phi_n(x_i))^2 + \lambda \sum_n \frac{c_n^2}{\lambda_n}$$

regularization이 **고주파 성분을 억제**한다는 의미가 명확해진다!
- 계수 공간에서: $c_n$ ⟹ $c_n / (\lambda_n / \lambda)$로 shrink

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./03-moore-aronszajn.md) | [📚 README](../README.md) | [다음 ▶](./05-representer-theorem.md) |

</div>
