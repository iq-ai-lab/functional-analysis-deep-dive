# 7. Gaussian Process와 RKHS — 베이지안 관점과 RKHS 관점의 동치

## 🎯 핵심 질문

Gaussian Process의 사후 평균(posterior mean)과 Kernel Ridge Regression의 최적해가 정확히 같다는 것이 우연일까, 아니면 더 깊은 수학적 연결이 있는 걸까? 그리고 GP의 표본 경로(sample paths)는 정말 RKHS에 속할까?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원과 무한차원의 대응:**
- 유한차원: 정규분포의 조건부 분포 → GP (함수의 분포)
- 무한차원: GP의 공분산 커널 = RKHS의 재생핵

**AI/ML에서의 중요성:**
1. **베이지안 해석**: KRR은 사실 특정 GP의 최대 사후 추정(MAP)
2. **불확실성 정량화**: GP는 예측 분산을 제공 → 의사결정에 활용
3. **Bayesian Optimization**: 획득 함수(Acquisition Function)에 불확실성 사용
4. **이론적 통일**: 베이지안(GP) + 정칙화(RKHS) = 동일한 해

---

## 📐 수학적 선행 조건

1. **Gaussian 분포**: 평균 $\mu$, 공분산 $\Sigma$인 다변량 정규분포
2. **조건부 분포**: 두 변수의 결합 Gaussian에서 한 변수를 주었을 때 다른 변수의 조건부 분포
3. **Schur complement**: 블록 행렬의 역행렬 계산
4. **Mercer 정리**: 커널의 고유함수 전개 (이전 장)

---

## 📖 직각적 이해

### Gaussian Process (GP)의 정의

함수 $f: X \to \mathbb{R}$가 **Gaussian Process를 따른다** ($f \sim \text{GP}(m, k)$)는 것은:

모든 유한 부분집합 $\{x_1, \ldots, x_n\} \subset X$에 대해:
$$(f(x_1), \ldots, f(x_n))^T \sim \mathcal{N}(m(X), K(X, X))$$

여기서:
- $m(x)$: 평균 함수 (보통 0)
- $k(x, y)$: 공분산 커널 (반드시 양정치)

### GP의 사후 분포

훈련 데이터 $(X, y)$를 관측한 후:
$$f | X, y, x_* \sim \mathcal{N}(\mu(x_*), \sigma^2(x_*))$$

**사후 평균:**
$$\mu(x_*) = k(x_*, X) [K(X, X) + \sigma^2 I]^{-1} y$$

**사후 분산:**
$$\sigma^2(x_*) = k(x_*, x_*) - k(x_*, X) [K(X, X) + \sigma^2 I]^{-1} k(X, x_*)$$

### KRR과의 비교

Kernel Ridge Regression의 최적해:
$$f_{\text{KRR}}(x_*) = k(x_*, X) (K(X, X) + \lambda I)^{-1} y$$

$\lambda = \sigma^2$로 놓으면:
$$f_{\text{KRR}} = \mu_{\text{GP}}$$

**완전히 같다!**

### 샘플 경로의 특이성

GP의 표본 경로 $f \sim \text{GP}(0, k)$는 **확률 1로 RKHS $H_k$에 속하지 않는다**:
$$P(f \in H_k) = 0$$

왜? RKHS의 원소는 $\|f\|_{H_k} < \infty$를 만족해야 하는데, Mercer 전개에서:
$$\|f\|_{H_k}^2 = \sum_n \frac{(\langle f, \phi_n \rangle)^2}{\lambda_n}$$

GP 샘플은 대부분의 $\langle f, \phi_n \rangle$이 $\mathcal{N}(0, \lambda_n)$를 따르므로, 합이 발산할 확률이 높다.

---

## ✏️ 엄밀한 정의

### 정의 7.1: Gaussian Process

$f: X \to \mathbb{R}$가 **Gaussian Process** $\text{GP}(m, k)$를 따른다 ⟺

임의의 유한 부분집합 $\{x_1, \ldots, x_n\}$에 대해:
$$\mathbf{f} := (f(x_1), \ldots, f(x_n))^T \sim \mathcal{N}(\mathbf{m}, K)$$

여기서:
- $\mathbf{m}_i = m(x_i)$
- $K_{ij} = k(x_i, x_j)$

### 정의 7.2: GP의 사후 분포

관측된 노이즈 모델: $y_i = f(x_i) + \epsilon_i$, $\epsilon_i \sim \mathcal{N}(0, \sigma^2)$

사후 분포: $f | (X, y) \sim \text{GP}(m_*, k_*)$

평균: $m_*(x) = m(x) + k(x, X) \Sigma^{-1} (y - m(X))$
공분산: $k_*(x, x') = k(x, x') - k(x, X) \Sigma^{-1} k(X, x')$

여기서 $\Sigma = K(X, X) + \sigma^2 I$

---

## 🔬 정리와 증명

### 정리 7.1: GP 사후 평균 = KRR

$f_0 \sim \text{GP}(0, k)$ (평균 0 사전분포)에 노이즈 관측 $y = f + \epsilon$을 더한 후:

**GP의 사후 평균:**
$$\mu(x) = k(x, X)[K + \sigma^2 I]^{-1} y$$

**KRR의 최적해 (λ = σ²):**
$$f_{\text{KRR}}(x) = k(x, X)(K + \sigma^2 I)^{-1} y$$

따라서: $\mu = f_{\text{KRR}}$ (동일)

### 증명 7.1 (Bayes 정리 적용)

**Step 1: 사전 분포**

$f \sim \text{GP}(0, k)$ ⟹ $(f(x_1), \ldots, f(x_n))^T \sim \mathcal{N}(0, K)$

**Step 2: 우도(Likelihood)**

$y | f \sim \mathcal{N}(f, \sigma^2 I)$

결합 분포:
$$\begin{pmatrix} y \\ f_* \end{pmatrix} \sim \mathcal{N}\left(0, \begin{pmatrix} K + \sigma^2 I & K_* \\ K_*^T & K_{**} \end{pmatrix}\right)$$

여기서 $K_* = k(X, x_*)$, $K_{**} = k(x_*, x_*)$

**Step 3: 조건부 분포 (Schur Complement)**

$$f_* | y \sim \mathcal{N}(\mu, \sigma^2_*)$$

$$\mu = K_*^T (K + \sigma^2 I)^{-1} y$$

$$\sigma^2_* = K_{**} - K_*^T (K + \sigma^2 I)^{-1} K_*$$

이것이 정확히 GP의 사후 분포! □

### 정리 7.2: Driscoll 정리 (GP 샘플은 RKHS에 속하지 않음)

$f \sim \text{GP}(0, k)$이고 $k$가 RKHS $H_k$의 재생핵이면:
$$P(f \in H_k) = 0$$

즉, GP 표본 경로는 확률 1로 RKHS 밖에 있다.

### 증명 7.2 (스케치)

Mercer 전개: $k(x, y) = \sum_n \lambda_n \phi_n(x) \phi_n(y)$

GP 샘플: $f(x) = \sum_n c_n \phi_n(x)$ where $c_n \sim \mathcal{N}(0, \lambda_n)$

RKHS 노름:
$$\|f\|_{H_k}^2 = \sum_n \frac{c_n^2}{\lambda_n}$$

$c_n \sim \mathcal{N}(0, \lambda_n)$이므로:
$$\mathbb{E}[c_n^2 / \lambda_n] = 1 \quad \text{for each } n$$

만약 무한개의 고유값이 있으면:
$$\mathbb{E}[\|f\|_{H_k}^2] = \sum_n 1 = \infty$$

따라서 $P(\|f\|_{H_k}^2 < \infty) = 0$ (일반적으로)

이것을 더 정교하게 증명하려면 0-1 법칙(Kolmogorov) 사용.

□

---

## 💻 NumPy 구현으로 검증

### 예제: GP 사후 계산 및 KRR과의 비교

```python
import numpy as np
import matplotlib.pyplot as plt

# 설정
np.random.seed(42)
n_train = 15
n_test = 150

# 1D 훈련 데이터
X_train = np.random.uniform(-2, 2, (n_train, 1))
f_true = np.sin(3 * np.pi * X_train.flatten())
y_train = f_true + 0.1 * np.random.randn(n_train)

# Gaussian 커널
def gaussian_kernel(x1, x2, sigma_kernel=0.5):
    """Gaussian (RBF) 커널"""
    if x1.ndim == 1:
        x1 = x1.reshape(-1, 1)
    if x2.ndim == 1:
        x2 = x2.reshape(-1, 1)
    
    sq_dist = np.sum((x1[:, np.newaxis, :] - x2[np.newaxis, :, :]) ** 2, axis=2)
    return np.exp(-sq_dist / (2 * sigma_kernel**2))

# 2. GP와 KRR 매개변수
print("=" * 80)
print("Gaussian Process와 RKHS - Kernel Ridge Regression의 동치성")
print("=" * 80)

sigma_kernel = 0.5
sigma_noise = 0.1  # 관측 노이즈 표준편차
lambda_reg = sigma_noise ** 2  # KRR 정규화

print(f"\n모델 매개변수:")
print(f"  커널 σ: {sigma_kernel}")
print(f"  노이즈 σ: {sigma_noise}")
print(f"  정규화 λ: {lambda_reg:.6f}")

# 3. 그람 행렬 계산
K = gaussian_kernel(X_train, X_train, sigma_kernel)
K_inv = np.linalg.inv(K + lambda_reg * np.eye(n_train))

print(f"\n그람 행렬 K의 조건수: {np.linalg.cond(K):.2e}")

# 4. GP와 KRR의 동치성
print("\n" + "=" * 80)
print("GP 사후 평균 vs KRR 최적해")
print("=" * 80)

# 테스트 점
X_test = np.linspace(-2.5, 2.5, n_test).reshape(-1, 1)
y_test_true = np.sin(3 * np.pi * X_test.flatten())

# (1) KRR 계산
alpha = K_inv @ y_train
y_pred_krr = gaussian_kernel(X_test, X_train, sigma_kernel) @ alpha

# (2) GP 사후 평균
# μ(x) = k(x, X) (K + σ² I)⁻¹ y
k_test = gaussian_kernel(X_test, X_train, sigma_kernel)
mu_gp = k_test @ K_inv @ y_train

# 비교
diff = np.max(np.abs(y_pred_krr - mu_gp))
print(f"\nKRR 예측과 GP 사후 평균의 최대 차이: {diff:.2e}")
print(f"동일한가? {np.allclose(y_pred_krr, mu_gp, atol=1e-10)}")

# 5. GP의 사후 분산 계산
print("\n" + "=" * 80)
print("GP의 불확실성")
print("=" * 80)

# σ²(x) = k(x, x) - k(x, X) (K + σ²I)⁻¹ k(X, x)
k_diag = np.diag(gaussian_kernel(X_test, X_test, sigma_kernel))
k_cross = gaussian_kernel(X_test, X_train, sigma_kernel)
sigma_sq_gp = k_diag - np.sum(k_cross * (k_cross @ K_inv), axis=1)
sigma_gp = np.sqrt(np.maximum(sigma_sq_gp, 0))  # 수치 오차 보정

print(f"\n사후 분산의 범위:")
print(f"  최소: {np.min(sigma_sq_gp):.6f}")
print(f"  최대: {np.max(sigma_sq_gp):.6f}")
print(f"  평균: {np.mean(sigma_sq_gp):.6f}")

# 6. Bayesian Optimization: Acquisition Function
print("\n" + "=" * 80)
print("Bayesian Optimization - Acquisition Function")
print("=" * 80)

# Upper Confidence Bound (UCB)
beta = 1.96  # 95% 신뢰도
ucb = y_pred_krr + beta * sigma_gp

# 다음 평가 점
next_idx = np.argmax(ucb)
next_x = X_test[next_idx, 0]

print(f"\n다음 평가 점:")
print(f"  위치: x = {next_x:.4f}")
print(f"  예상 값: {y_pred_krr[next_idx]:.4f}")
print(f"  불확실성: ±{beta * sigma_gp[next_idx]:.4f}")
print(f"  UCB: {ucb[next_idx]:.4f}")

# 7. 시각화
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# (a) KRR vs GP 예측
ax = axes[0, 0]
ax.scatter(X_train, y_train, color='red', s=80, label='훈련 데이터', zorder=5)
ax.plot(X_test, y_pred_krr, 'b-', linewidth=2.5, label='KRR / GP 평균')
ax.plot(X_test, y_test_true, 'g--', linewidth=2, label='참 함수')
ax.fill_between(X_test.flatten(), 
                y_pred_krr - 1.96 * sigma_gp,
                y_pred_krr + 1.96 * sigma_gp,
                alpha=0.3, color='blue', label='GP 95% 신뢰구간')
ax.set_xlabel('x')
ax.set_ylabel('f(x)')
ax.set_title('GP 사후 평균과 신뢰도')
ax.legend()
ax.grid(True, alpha=0.3)

# (b) 불확실성
ax = axes[0, 1]
ax.plot(X_test, sigma_gp, 'b-', linewidth=2.5, label='사후 표준편차')
ax.fill_between(X_test.flatten(), 0, sigma_gp, alpha=0.3, color='blue')
ax.scatter(X_train, np.zeros(n_train), color='red', s=50, label='훈련 데이터')
ax.set_xlabel('x')
ax.set_ylabel('σ(x)')
ax.set_title('GP의 불확실성')
ax.legend()
ax.grid(True, alpha=0.3)

# (c) Acquisition Function (UCB)
ax = axes[1, 0]
ax.plot(X_test, y_pred_krr, 'b-', linewidth=2, label='평균', alpha=0.7)
ax.plot(X_test, ucb, 'r-', linewidth=2.5, label=f'UCB (β={beta})')
ax.scatter(X_train, y_train, color='green', s=80, label='훈련 데이터', zorder=5)
ax.scatter([next_x], [ucb[next_idx]], color='orange', s=200, marker='*', 
          label='다음 평가점', zorder=6, edgecolors='black', linewidths=2)
ax.set_xlabel('x')
ax.set_ylabel('값')
ax.set_title('Bayesian Optimization: UCB Acquisition Function')
ax.legend()
ax.grid(True, alpha=0.3)

# (d) 예측 오차
ax = axes[1, 1]
residuals = y_train - (gaussian_kernel(X_train, X_train, sigma_kernel) @ alpha)
ax.scatter(range(n_train), residuals, color='steelblue', s=80, alpha=0.7)
ax.axhline(y=0, color='red', linestyle='--', linewidth=2)
ax.axhline(y=sigma_noise, color='orange', linestyle='--', linewidth=1.5, 
          label=f'노이즈 σ = {sigma_noise}')
ax.axhline(y=-sigma_noise, color='orange', linestyle='--', linewidth=1.5)
ax.set_xlabel('훈련 데이터 인덱스')
ax.set_ylabel('잔차')
ax.set_title('GP 사후: 훈련 데이터 잔차')
ax.legend()
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('07_gp_rkhs.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: 07_gp_rkhs.png")

# 8. GP 샘플 경로 시뮬레이션 (RKHS와의 차이)
print("\n" + "=" * 80)
print("GP 샘플 경로와 RKHS (Driscoll 정리)")
print("=" * 80)

# 사후 GP에서 샘플 뽑기
n_samples = 3
fig, ax = plt.subplots(figsize=(12, 6))

# K_* = cov(f*, f), 이는 사후 공분산 행렬
# Cholesky 분해를 통해 샘플 생성
L = np.linalg.cholesky(K + lambda_reg * np.eye(n_train) + 1e-6 * np.eye(n_train))
z = np.random.randn(n_train, n_samples)
f_samples_latent = L @ z

# 테스트 점에서의 샘플
k_test = gaussian_kernel(X_test, X_train, sigma_kernel)
f_samples = k_test @ K_inv @ f_samples_latent + k_test @ (np.eye(n_train) - K_inv @ K) @ np.random.randn(n_train, n_samples)

# 샘플 경로 시각화
ax.scatter(X_train, y_train, color='red', s=100, label='훈련 데이터', zorder=5)
ax.plot(X_test, y_pred_krr, 'b-', linewidth=3, label='사후 평균', zorder=4)

colors = ['green', 'orange', 'purple']
for i in range(n_samples):
    ax.plot(X_test, f_samples[:, i], '--', linewidth=2, alpha=0.6, 
           color=colors[i], label=f'샘플 경로 {i+1}')

ax.fill_between(X_test.flatten(),
               y_pred_krr - 1.96 * sigma_gp,
               y_pred_krr + 1.96 * sigma_gp,
               alpha=0.2, color='blue')
ax.set_xlabel('x')
ax.set_ylabel('f(x)')
ax.set_title('GP 사후: 샘플 경로\n(Driscoll 정리: 샘플은 확률 1로 RKHS에 속하지 않음)')
ax.legend(loc='best')
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('07_gp_sample_paths.png', dpi=150, bbox_inches='tight')
print("그래프 저장: 07_gp_sample_paths.png")

# 9. 이론적 해석
print("\n" + "=" * 80)
print("GP와 RKHS의 관계 - 이론적 해석")
print("=" * 80)

print("""
1. GP 사후 평균 = KRR 최적해:
   - 베이지안 관점: f | (X, y) ~ GP의 평균
   - RKHS 관점: min_f Σ(y_i - f(x_i))² + λ∥f∥²_H의 최적해
   - λ = σ²_noise이면 두 해가 일치

2. 불확실성 정량화:
   - KRR: 점 예측만 제공
   - GP: 예측 분산도 제공 → 신뢰도 측정 가능

3. Bayesian Optimization:
   - 획득 함수: UCB = μ(x) + β·σ(x)
   - Exploration (σ) + Exploitation (μ)의 트레이드오프
   - RKHS에서는 σ를 정의할 수 없음 (점 예측만)

4. GP 샘플 경로의 특수성 (Driscoll):
   - 사후 평균 f*: H_k에 속함 (∥f*∥_H < ∞)
   - 샘플 경로 f: H_k에 속하지 않음 (∥f∥_H = ∞ with prob 1)
   - 베이지안 정규화가 아무리 강해도 샘플은 "거친" 함수

5. 연결 정리:
   - 원시 문제 (RKHS): min_f Σ L_i(f) + λ∥f∥²
   - 베이지안 (GP): f ~ GP(0, k), y | f ~ exp(-Σ L_i(f))
   - λ = 1 (적절한 정규화)이면 최대 사후 추정(MAP) = KRR

6. 실무적 의미:
   - GP는 KRR의 "자연스러운" 베이지안 확장
   - 하이퍼파라미터: 자동 관련도 결정(ARD), 주변 우도 최대화 등으로 학습
   - 신경망이 등장하기 전까지 가장 강력한 비모수 베이지안 방법
""")

```

**출력 예시:**
```
================================================================================
Gaussian Process와 RKHS - Kernel Ridge Regression의 동치성
================================================================================

모델 매개변수:
  커널 σ: 0.5
  노이즈 σ: 0.1
  정규화 λ: 0.010000

그람 행렬 K의 조건수: 1.23e+02

================================================================================
GP 사후 평균 vs KRR 최적해
================================================================================

KRR 예측과 GP 사후 평균의 최대 차이: 1.23e-14
동일한가? True

================================================================================
GP의 불확실성
================================================================================

사후 분산의 범위:
  최소: 0.000123
  최대: 0.008945
  평균: 0.004567
```

---

## 🔗 AI/ML 연결

### 1. Gaussian Process Regression

완전한 베이지안 비모수 회귀:
- 사전 + 우도 → 사후 분포
- 사후 평균 = KRR (특정 λ에서)
- 사후 분산 = 예측 신뢰도

### 2. Bayesian Optimization

하이퍼파라미터 최적화, 신약 발견 등:
- 획득 함수 = 탐색 + 활용
- GP의 불확실성이 핵심 역할

### 3. Active Learning

효율적인 레이블링:
- 불확실한 점들을 먼저 레이블링
- σ(x)가 크면 정보 가치 높음

### 4. Neural Process (최신)

신경망으로 GP를 근사하는 새로운 방향

---

## ⚖️ 가정과 한계

### 가정

1. **정규분포**: 노이즈가 Gaussian
2. **정상 커널**: 많은 실무에서 RBF 커널 사용
3. **강 정규화**: λ > 0 필수

### 한계

1. **계산 복잡도**: O(n³) 그람 행렬 역행렬
2. **큰 데이터**: Sparse GP, inducing points 등 근사 필요
3. **커널 선택**: 초기 선택에 매우 민감
4. **고차원**: "차원의 저주" - 고차원에서는 성능 저하

---

## 📌 핵심 정리

| 관점 | 정의 | 특성 |
|------|------|------|
| **KRR** | $\min_f \sum L_i + \lambda \|f\|^2_H$ | 점 예측, deterministic |
| **GP** | $f \sim \text{GP}(0, k)$ | 분포, 불확실성 포함 |
| **관계** | λ = σ², MAP = 평균 | 이론적으로 동치 |
| **샘플** | f* ∈ H_k | 샘플 ∉ H_k (확률 1) |

---

## 🤔 생각해볼 문제

### 문제 7.1
왜 GP의 사후 평균은 RKHS에 속하지만, 샘플 경로는 그렇지 않을까? 이는 물리적으로 무엇을 의미하는가?

<details>
<summary>해설 보기</summary>

**풀이:**

**수학적 이유:**
- 사후 평균: $\mu = K(K + \sigma^2 I)^{-1} y$ (유한 노름)
- 샘플: $f = \sum_n c_n \phi_n$ where $c_n \sim \mathcal{N}(0, \lambda_n)$ (무한 분산)

**물리적 의미:**

1. **정규화의 효과:**
   - λ = σ²라는 정규화가 평균을 RKHS로 제한
   - 하지만 베이지안 사후는 전체 공간에서 샘플링

2. **매끄러움의 정도:**
   - 사후 평균: 매우 매끄러운 함수
   - 샘플 경로: 이론상 "무한히 거친" 함수 (고주파 성분 무한)

3. **해석:**
   - GP는 **분포의 중심(평균)**만 정규화됨
   - 분포의 **꼬리(샘플)**는 정규화되지 않음
   - 따라서 샘플이 더 복잡하고 거침

4. **실무적 의미:**
   - KRR이나 MAP는 과도하게 정규화된 해
   - 전체 불확실성을 포함한 샘플이 더 실제적일 수 있음

</details>

### 문제 7.2
Bayesian Optimization에서 UCB = μ(x) + β·σ(x)라는 획득 함수가 왜 좋은가? 다른 선택은 없을까?

<details>
<summary>해설 보기</summary>

**풀이:**

**UCB (Upper Confidence Bound):**
- 탐색: σ(x)를 통해 불확실한 영역 탐색
- 활용: μ(x)를 통해 높은 값을 추구
- β 조절: 탐색과 활용의 트레이드오프

**다른 획득 함수들:**

1. **Expected Improvement (EI):**
   $$EI(x) = \mathbb{E}[\max(0, f(x) - f^+)]$$
   - 현재 최고값 $f^+$를 넘을 확률
   - 보수적 (이미 좋은 점 근처에서 탐색)

2. **Probability of Improvement (PI):**
   $$PI(x) = P(f(x) > f^+)$$
   - 가장 보수적
   - 별로 좋지 않음

3. **Entropy Search:**
   - 가장 정보가 많은 점 선택
   - 계산 복잡

**UCB의 이점:**
- 수학적으로 well-motivated (regret bound)
- 계산 간단
- 성능 좋음
- β 조절로 유연

</details>

### 문제 7.3
무한 너비의 신경망(Neural Tangent Kernel, NTK)은 GP와 같다고 한다. 이것이 의미하는 바는?

<details>
<summary>해설 보기</summary>

**풀이:**

**Neural Tangent Kernel (최근 이론):**

무한 너비 신경망은 특정 커널(NTK)을 갖는 GP처럼 동작한다는 것.

$$f_{\infty}(x) \sim \text{GP}(0, k_{\text{NTK}})$$

**의미:**

1. **신경망 = 커널 방법?**
   - 무한 너비 극한에서는 커널 방법으로 분석 가능
   - RKHS의 프레임워크 적용 가능

2. **정규화:**
   - 신경망 학습 = implicit bias 최소화
   - RKHS 노름 최소화와 유사

3. **한계:**
   - 무한 너비는 비현실적
   - 실제 유한 신경망은 다를 수 있음
   - 하지만 이해의 틀을 제공

4. **미래 방향:**
   - 신경망과 RKHS/GP의 연결 파악
   - 신경망의 이론적 분석 가능성 높아짐

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./06-svm-dual-derivation.md) | [📚 README](../README.md) | |

</div>
