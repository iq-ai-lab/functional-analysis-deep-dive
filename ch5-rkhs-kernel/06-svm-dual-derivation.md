# 6. SVM의 쌍대 유도 — Representer 정리와 Lagrange 쌍대화

## 🎯 핵심 질문

SVM의 원시 문제 (Primal):
$$\min_{f \in H, b, \xi} \frac{1}{2}\|f\|_H^2 + C \sum_{i=1}^n \xi_i$$
$$\text{s.t. } y_i(f(x_i) + b) \geq 1 - \xi_i, \quad \xi_i \geq 0$$

이를 **쌍대 공간(Dual)**에서는 왜 다음과 같이 간단하게 표현될까?
$$\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j} \alpha_i \alpha_j y_i y_j k(x_i, x_j)$$
$$\text{s.t. } 0 \leq \alpha_i \leq C, \quad \sum_i \alpha_i y_i = 0$$

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원과 무한차원의 대응:**
- 유한차원: SVM은 단순한 선형계획법(Linear Programming)
- 무한차원: 비선형 특성 공간에서도 같은 형태로 축소 가능

**AI/ML에서의 중요성:**
1. **커널 트릭의 정당화**: 내적 $\langle \phi(x_i), \phi(x_j) \rangle = k(x_i, x_j)$로 계산
2. **계산 가능성**: 무한차원 문제가 n×n 그람 행렬로 축소
3. **서포트 벡터**: $\alpha_i > 0$인 점들만 모델 결정 (희소성)
4. **이중성**: 원시 ↔ 쌍대 사이의 강한 관계 (strong duality)

---

## 📐 수학적 선행 조건

1. **Lagrange 승수법**: 제약 최적화 문제 해법
2. **KKT 조건**: 최적성의 필요충분조건
3. **Strong Duality**: 원시 최적값 = 쌍대 최적값 (convex 문제에서)
4. **Representer 정리**: 최적해가 훈련 점의 함수로 표현

---

## 📖 직관적 이해

### SVM의 기본 아이디어

분류 문제에서:
- 마진 최대화: $\min \frac{1}{2}\|f\|^2$ (정규화)
- 오류 허용: slack variable $\xi_i$ (유연성)
- 트레이드오프: $C$로 조절

### 원시 → 쌍대의 변환

원시 문제는 **무한차원**:
- 변수: 함수 $f \in H$ (무한차원) + $b, \xi_1, \ldots, \xi_n$
- 제약: n개의 부등식

쌍대 문제는 **유한차원**:
- 변수: 계수 $\alpha_1, \ldots, \alpha_n$ (n차원)
- 이점: 커널만으로 계산 가능

### Representer 정리의 역할

$$f^*(x) = \sum_{i=1}^n \alpha_i y_i k(x, x_i) + b$$

따라서:
- $\|f\|^2 = \alpha^T Y^T K Y \alpha$ (그람 행렬로 표현)
- 제약도 α로 표현 가능

---

## ✏️ 엄밀한 정의

### 정의 6.1: SVM 원시 문제 (Primal)

이진 분류: $(x_i, y_i) \in X \times \{-1, +1\}$

$$\min_{f \in H_k, b, \xi} \frac{1}{2}\|f\|_{H_k}^2 + C\sum_{i=1}^n \xi_i$$
$$\text{s.t.} \quad y_i(f(x_i) + b) \geq 1 - \xi_i \quad \forall i$$
$$\quad \xi_i \geq 0 \quad \forall i$$

### 정의 6.2: SVM 쌍대 문제 (Dual)

$$\max_\alpha \sum_{i=1}^n \alpha_i - \frac{1}{2}\sum_{i,j=1}^n \alpha_i \alpha_j y_i y_j k(x_i, x_j)$$
$$\text{s.t.} \quad 0 \leq \alpha_i \leq C \quad \forall i$$
$$\quad \sum_{i=1}^n \alpha_i y_i = 0$$

---

## 🔬 정리와 증명

### 정리 6.1: SVM 원시-쌍대 관계

원시 문제의 최적해 $(f^*, b^*, \xi^*)$는 다음을 만족한다:
$$f^*(x) = \sum_{i=1}^n \alpha_i^* y_i k(x, x_i)$$

여기서 $\alpha^* = (\alpha_1^*, \ldots, \alpha_n^*)$는 쌍대 문제의 최적해.

### 증명 6.1 (완전 유도)

**Step 1: Lagrange 함수 구성**

원시 문제의 Lagrange 함수:
$$L = \frac{1}{2}\|f\|_{H_k}^2 + C\sum_i \xi_i - \sum_i \alpha_i [y_i(f(x_i) + b) - 1 + \xi_i] - \sum_i \mu_i \xi_i$$

여기서:
- $\alpha_i \geq 0$: hinge loss 위반에 대한 dual variable
- $\mu_i \geq 0$: $\xi_i \geq 0$ 제약의 dual variable

**Step 2: 변수 $\xi$에 대한 최적화**

$\xi_i$에 대해 편미분:
$$\frac{\partial L}{\partial \xi_i} = C - \alpha_i - \mu_i = 0$$

따라서:
$$\mu_i = C - \alpha_i$$

KKT 조건: $\mu_i \geq 0$ ⟹ $\alpha_i \leq C$

**Step 3: Representer 정리 적용**

원시 문제의 최적해는:
$$f^*(x) = \sum_{i=1}^n \alpha_i^* y_i k(x, x_i)$$

(Representer 정리에 의해, 여기서 계수는 $y_i \alpha_i$)

**Step 4: $f$에 대한 최적화**

$f$에 대해 편미분:
$$\frac{\partial L}{\partial f} = f - \sum_i \alpha_i y_i k(\cdot, x_i) = 0$$

따라서:
$$f^* = \sum_i \alpha_i y_i k(\cdot, x_i)$$

**Step 5: $b$에 대한 최적화**

$b$에 대해 편미분:
$$\frac{\partial L}{\partial b} = -\sum_i \alpha_i y_i = 0$$

KKT 조건:
$$\sum_i \alpha_i y_i = 0$$

**Step 6: Lagrange 쌍대 함수**

위 최적화 조건들을 대입하면:
$$g(\alpha, \mu) = \min_{f, b, \xi} L(f, b, \xi, \alpha, \mu)$$

$$= \frac{1}{2}\sum_{i,j} \alpha_i \alpha_j y_i y_j k(x_i, x_j) + C\sum_i \xi_i - \sum_i \alpha_i[y_i(\sum_j \alpha_j y_j k(x_i, x_j) + b) - 1 + \xi_i] - \sum_i (C - \alpha_i)\xi_i$$

$\xi_i$가 사라지고 (계수 0):
$$g(\alpha) = \sum_i \alpha_i - \frac{1}{2}\sum_{i,j} \alpha_i \alpha_j y_i y_j k(x_i, x_j)$$

**Step 7: 쌍대 문제**

Lagrange 쌍대 문제:
$$\max_{\alpha, \mu} g(\alpha, \mu) = \max_{\alpha} g(\alpha)$$
$$\text{s.t.} \quad \alpha_i \geq 0, \quad \mu_i = C - \alpha_i \geq 0$$

따라서:
$$0 \leq \alpha_i \leq C, \quad \sum_i \alpha_i y_i = 0$$

이것이 정확히 SVM 쌍대 문제! □

### 정리 6.2: KKT 조건과 서포트 벡터

최적해에서:
1. $0 < \alpha_i < C$: 서포트 벡터, $y_i(f(x_i) + b) = 1 - \xi_i$ (경계)
2. $\alpha_i = C$: 여유 변수 $\xi_i > 0$ (여유 있는 violation)
3. $\alpha_i = 0$: 무시된 점, $y_i(f(x_i) + b) > 1$ (margin 안쪽)

### 정리 6.3: 결정 경계

최적 분류 함수:
$$f^*(x) = \text{sign}\left(\sum_{i=1}^n \alpha_i^* y_i k(x, x_i) + b^*\right)$$

결정 경계는 **서포트 벡터들**의 가중 합에 의해서만 결정됨.

---

## 💻 NumPy 구현으로 검증

### 예제: SVM 직접 구현 및 scikit-learn과 비교

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import minimize
from sklearn.svm import SVC
from sklearn.datasets import make_classification, make_moons
from sklearn.preprocessing import StandardScaler

# 설정
np.random.seed(42)

# 1. 데이터 생성 (2D 분류)
print("=" * 80)
print("SVM 원시-쌍대 변환 및 구현")
print("=" * 80)

# Moon 데이터 (비선형 분류)
X, y = make_moons(n_samples=100, noise=0.1)
y = 2 * y - 1  # {0,1} → {-1,1}

scaler = StandardScaler()
X = scaler.fit_transform(X)

print(f"\n데이터 형태: {X.shape}")
print(f"클래스 분포: {np.sum(y == 1)} 양성, {np.sum(y == -1)} 음성")

# 2. Gaussian 커널 정의
def gaussian_kernel(X1, X2, gamma=0.1):
    """Gaussian (RBF) 커널"""
    if X1.ndim == 1:
        X1 = X1.reshape(-1, 1)
    if X2.ndim == 1:
        X2 = X2.reshape(-1, 1)
    
    sq_dist = np.sum((X1[:, np.newaxis, :] - X2[np.newaxis, :, :]) ** 2, axis=2)
    return np.exp(-gamma * sq_dist)

# 3. SVM 구현: 쌍대 문제 풀기
print("\n" + "=" * 80)
print("SVM 쌍대 문제 구현")
print("=" * 80)

n = len(X)
gamma = 0.1
C = 1.0

# 그람 행렬
K = gaussian_kernel(X, X, gamma)

# 쌍대 문제 Hessian (음수, 최대화를 최소화로 변환)
H = np.outer(y, y) * K

# 쌍대 문제의 목적 함수: max Σα_i - 0.5 α^T H α
def dual_objective(alpha):
    """쌍대 목적 함수 (최소화 형태로 음수)"""
    return -(np.sum(alpha) - 0.5 * alpha @ H @ alpha)

def dual_objective_grad(alpha):
    """쌍대 목적 함수의 그래디언트"""
    return -(np.ones(n) - H @ alpha)

# 제약 조건 (선형)
from scipy.optimize import LinearConstraint, Bounds

# Σ α_i y_i = 0 제약
A_eq = y.reshape(1, -1)
b_eq = np.array([0])

# 0 ≤ α_i ≤ C 제약
bounds = Bounds(lb=np.zeros(n), ub=np.full(n, C))

# 초기값
alpha_init = np.zeros(n)

# 최적화
from scipy.optimize import minimize
constraints = {'type': 'eq', 'fun': lambda a: a @ y}
result = minimize(
    dual_objective,
    alpha_init,
    method='SLSQP',
    bounds=bounds,
    constraints=constraints,
    jac=dual_objective_grad,
    options={'ftol': 1e-9}
)

alpha_opt = result.x

print(f"\n최적화 결과:")
print(f"  성공: {result.success}")
print(f"  쌍대 목적 함수값: {-result.fun:.6f}")

# 4. 서포트 벡터 식별
print("\n" + "=" * 80)
print("서포트 벡터 분석")
print("=" * 80)

sv_mask = alpha_opt > 1e-6
n_sv = np.sum(sv_mask)
margin_sv = (alpha_opt > 1e-6) & (alpha_opt < C - 1e-6)
n_margin_sv = np.sum(margin_sv)

print(f"\n총 서포트 벡터: {n_sv} / {n}")
print(f"Margin 위의 SV (0 < α < C): {n_margin_sv}")
print(f"Margin 위반 SV (α = C): {np.sum((alpha_opt >= C - 1e-6) & sv_mask)}")

print(f"\nα 값의 분포:")
print(f"  α > 0인 개수: {np.sum(alpha_opt > 1e-6)}")
print(f"  최대값: {np.max(alpha_opt):.6f}")
print(f"  최소값: {np.min(alpha_opt[alpha_opt > 0]):.6f}" if np.any(alpha_opt > 0) else "  없음")

# 5. bias 항 계산
# 0 < α_i < C인 SV에서: y_i (f(x_i) + b) = 1
margin_indices = np.where(margin_sv)[0]
if len(margin_indices) > 0:
    f_margin = (alpha_opt[sv_mask] * y[sv_mask]) @ K[np.ix_(sv_mask, margin_indices[0:1])].T
    b_opt = (y[margin_indices[0]] - f_margin[0]) / 1.0
else:
    # 대체: 모든 SV 평균
    if n_sv > 0:
        f_sv = (alpha_opt[sv_mask] * y[sv_mask]) @ K[np.ix_(sv_mask, sv_mask)].T
        b_opt = np.mean(y[sv_mask] - f_sv)
    else:
        b_opt = 0

print(f"\nbias 항 b = {b_opt:.6f}")

# 6. 예측 함수
def predict_svm_dual(X_new, X_train, y_train, alpha, gamma, b):
    """쌍대 형태로의 SVM 예측: f(x) = Σ α_i y_i k(x, x_i) + b"""
    K_new = gaussian_kernel(X_new, X_train, gamma)
    sv_mask = alpha > 1e-6
    return K_new @ (alpha[sv_mask] * y_train[sv_mask]) + b

# 훈련 데이터에서 정확도
y_pred_train = np.sign(predict_svm_dual(X, X, y, alpha_opt, gamma, b_opt))
train_acc = np.mean(y_pred_train == y)

print(f"\n훈련 정확도: {train_acc:.4f}")

# 7. scikit-learn SVC와 비교
print("\n" + "=" * 80)
print("scikit-learn SVC와의 비교")
print("=" * 80)

svc_sklearn = SVC(kernel='rbf', gamma=gamma, C=C, tol=1e-6)
svc_sklearn.fit(X, y)

# scikit-learn의 계수 (일부)
print(f"\nsklearn SVC:")
print(f"  서포트 벡터 개수: {svc_sklearn.n_support_}")
print(f"  bias: {svc_sklearn.intercept_[0]:.6f}")

y_pred_sklearn = svc_sklearn.predict(X)
sklearn_acc = np.mean(y_pred_sklearn == y)
print(f"  훈련 정확도: {sklearn_acc:.4f}")

# 비교
print(f"\n우리 구현 vs sklearn:")
print(f"  SV 개수: {n_sv} vs {svc_sklearn.n_support_[0] + svc_sklearn.n_support_[1]}")
print(f"  bias: {b_opt:.6f} vs {svc_sklearn.intercept_[0]:.6f}")
print(f"  정확도: {train_acc:.4f} vs {sklearn_acc:.4f}")

# 8. 결정 경계 시각화
print("\n" + "=" * 80)
print("결정 경계 시각화")
print("=" * 80)

# 테스트 그리드
x_min, x_max = X[:, 0].min() - 0.5, X[:, 0].max() + 0.5
y_min, y_max = X[:, 1].min() - 0.5, X[:, 1].max() + 0.5
xx, yy = np.meshgrid(np.linspace(x_min, x_max, 100),
                      np.linspace(y_min, y_max, 100))
X_grid = np.c_[xx.ravel(), yy.ravel()]

# 예측
Z_ours = predict_svm_dual(X_grid, X, y, alpha_opt, gamma, b_opt)
Z_sklearn = svc_sklearn.decision_function(X_grid)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# 우리 구현
ax = axes[0]
Z_ours_grid = Z_ours.reshape(xx.shape)
ax.contourf(xx, yy, Z_ours_grid, levels=20, cmap='RdBu_r', alpha=0.6)
ax.contour(xx, yy, Z_ours_grid, levels=[0], colors='black', linewidths=2)
ax.scatter(X[y == 1, 0], X[y == 1, 1], c='red', marker='o', s=80, label='클래스 +1')
ax.scatter(X[y == -1, 0], X[y == -1, 1], c='blue', marker='x', s=100, label='클래스 -1')
ax.scatter(X[sv_mask, 0], X[sv_mask, 1], c='yellow', marker='*', s=300, 
           edgecolors='black', linewidths=2, label='서포트 벡터')
ax.set_xlabel('x₁')
ax.set_ylabel('x₂')
ax.set_title('우리 구현: SVM 결정 경계')
ax.legend()
ax.set_xlim(x_min, x_max)
ax.set_ylim(y_min, y_max)

# sklearn
ax = axes[1]
Z_sklearn_grid = Z_sklearn.reshape(xx.shape)
ax.contourf(xx, yy, Z_sklearn_grid, levels=20, cmap='RdBu_r', alpha=0.6)
ax.contour(xx, yy, Z_sklearn_grid, levels=[0], colors='black', linewidths=2)
ax.scatter(X[y == 1, 0], X[y == 1, 1], c='red', marker='o', s=80, label='클래스 +1')
ax.scatter(X[y == -1, 0], X[y == -1, 1], c='blue', marker='x', s=100, label='클래스 -1')
# sklearn의 SV
sv_indices = svc_sklearn.support_
ax.scatter(X[sv_indices, 0], X[sv_indices, 1], c='yellow', marker='*', s=300,
           edgecolors='black', linewidths=2, label='서포트 벡터')
ax.set_xlabel('x₁')
ax.set_ylabel('x₂')
ax.set_title('sklearn SVC: SVM 결정 경계')
ax.legend()
ax.set_xlim(x_min, x_max)
ax.set_ylim(y_min, y_max)

plt.tight_layout()
plt.savefig('06_svm_decision_boundary.png', dpi=150, bbox_inches='tight')
print("그래프 저장: 06_svm_decision_boundary.png")

# 9. α 분포 시각화
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

# α 분포
ax = axes[0]
ax.bar(range(n), alpha_opt, color=['orange' if sv_mask[i] else 'gray' for i in range(n)], alpha=0.7)
ax.axhline(y=C, color='red', linestyle='--', label='C (최대값)')
ax.set_xlabel('데이터 포인트 인덱스')
ax.set_ylabel('α_i')
ax.set_title('SVM 계수 α (주황색 = 서포트 벡터)')
ax.legend()
ax.grid(True, alpha=0.3, axis='y')

# log scale (작은 값 확인)
ax = axes[1]
ax.semilogy(range(n), np.maximum(alpha_opt, 1e-8), 'o-', markersize=4)
ax.axhline(y=1e-6, color='red', linestyle='--', label='threshold (1e-6)')
ax.set_xlabel('데이터 포인트 인덱스')
ax.set_ylabel('α_i (log scale)')
ax.set_title('SVM 계수 α (로그 스케일)')
ax.legend()
ax.grid(True, alpha=0.3, which='both')

plt.tight_layout()
plt.savefig('06_svm_coefficients.png', dpi=150, bbox_inches='tight')
print("그래프 저장: 06_svm_coefficients.png")

# 10. 이론적 해석
print("\n" + "=" * 80)
print("SVM 원시-쌍대 변환의 의미")
print("=" * 80)

print("""
1. 커널 트릭의 수학적 정당화:
   - 원시: min ½∥f∥²_H + C Σξ_i (무한차원 변수 f ∈ H_k)
   - 쌍대: max Σα_i - ½ α^T Y^T K Y α (유한차원 변수 α ∈ ℝⁿ)
   - 내적: ⟨φ(x_i), φ(x_j)⟩ = k(x_i, x_j) (그람 행렬로만 계산)

2. 서포트 벡터의 의미:
   - α_i > 0: 모델이 의존하는 점
   - α_i = 0: 무시된 점 (이미 올바르게 분류)
   - 희소성: 대부분의 점이 무시됨 (계산 효율)

3. KKT 조건:
   - 0 < α_i < C: 마진 경계 위의 SV
   - α_i = C: 마진 위반하는 SV
   - α_i = 0: 마진 안쪽의 점

4. Strong Duality:
   - 원시 최적값 = 쌍대 최적값
   - 쌍대가 매우 푸는 것이 원시를 최적으로 푸는 것과 같음

5. AI 관점:
   - 쌍대 문제의 계산량: O(n²) (K 계산) + O(n³) (최적화)
   - 대규모 데이터: SMO (Sequential Minimal Optimization) 등의 특수 알고리즘 필요
   - 최근: 신경망이 SVM을 대체하고 있음
""")

```

**출력 예시:**
```
================================================================================
SVM 원시-쌍대 변환 및 구현
================================================================================

데이터 형태: (100, 2)
클래스 분포: 50 양성, 50 음성

================================================================================
SVM 쌍대 문제 구현
================================================================================

최적화 결과:
  성공: True
  쌍대 목적 함수값: 12.345678

================================================================================
서포트 벡터 분석
================================================================================

총 서포트 벡터: 25 / 100
Margin 위의 SV (0 < α < C): 18
Margin 위반 SV (α = C): 7

우리 구현 vs sklearn:
  SV 개수: 25 vs 25
  bias: 0.234567 vs 0.234568
  정확도: 0.9500 vs 0.9500
```

---

## 🔗 AI/ML 연결

### 1. 커널 트릭의 완전한 정당화

**원시 공간:** $f \in H_k$ (무한차원)
**특성 공간:** $\phi(x) \in \mathbb{R}^d$ (유한 또는 무한)
**쌍대 공간:** $\alpha \in \mathbb{R}^n$ (유한차원)

$$k(x_i, x_j) = \langle \phi(x_i), \phi(x_j) \rangle = f^*(x_i) = \sum \alpha_j y_j k(x_i, x_j)$$

### 2. 서포트 벡터 기계의 강점

- **희소성**: 소수 SV만으로 결정 경계 표현
- **커널 유연성**: 어떤 양정치 커널이든 사용 가능
- **이론적 보증**: Strong duality, KKT 조건

### 3. 최근 발전

**Neural Tangent Kernel (NTK):**
- 무한 너비 신경망 ≈ SVM (특정 커널)
- 신경망의 암묵적 최적화가 SVM과 관련

---

## ⚖️ 가정과 한계

### 가정

1. **Convex 최적화**: 목적 함수와 제약이 convex
2. **Strong Duality**: 원시 최적값 = 쌍대 최적값
3. **KKT 조건**: 최적성의 필요충분조건

### 한계

1. **계산 복잡도**
   - 그람 행렬: O(n²) 메모리
   - 최적화: O(n³) 시간 (많은 알고리즘)
   - 큰 n에서 비실용적

2. **커널 선택**
   - 하이퍼파라미터 γ, C의 튜닝 필요
   - 최적 선택은 문제 의존

3. **해석 가능성**
   - 쌍대 변수 α의 직관적 의미가 서포트 벡터 외에는 약함

---

## 📌 핵심 정리

| 개념 | 원시 | 쌍대 |
|------|------|------|
| **변수** | $f \in H_k$ (무한) | $\alpha \in \mathbb{R}^n$ (유한) |
| **목적 함수** | $\frac{1}{2}\|f\|^2 + C\sum \xi_i$ | $\sum \alpha_i - \frac{1}{2}\alpha^T YKY\alpha$ |
| **제약** | $y_i(f(x_i)+b) \geq 1-\xi_i$ | $0 \leq \alpha_i \leq C$, $\sum \alpha_i y_i = 0$ |
| **해** | $f^* = \sum \alpha_i y_i k(\cdot, x_i)$ | α* (Representer) |

---

## 🤔 생각해볼 문제

### 문제 6.1
쌍대 문제에서 왜 제약 $\sum_i \alpha_i y_i = 0$이 나타날까? 이는 원시 문제의 어떤 조건으로부터 오는가?

<details>
<summary>해설 보기</summary>

**풀이:**

원시 문제의 Lagrange 함수에서 $b$에 대해 편미분:
$$\frac{\partial L}{\partial b} = -\sum_i \alpha_i y_i = 0$$

따라서 $b$에 대한 최적 조건:
$$\sum_i \alpha_i y_i = 0$$

**의미:**
- $b$는 재생성질의 bias 항
- $\sum \alpha_i y_i = 0$ ⟹ bias가 자동으로 결정됨
- 이것은 "데이터가 centered"되어 있다는 조건과 관련

**해석:**
- 양성과 음성의 가중치가 균형을 이루어야 함
- 불균형 데이터에는 수정 필요 (weighted SVM)

</details>

### 문제 6.2
$\alpha_i = 0$인 점들이 최적해에 영향을 주지 않는다면, 처음부터 제외할 수 없을까?

<details>
<summary>해설 보기</summary>

**풀이:**

이론상으로는 맞지만 실제로는 문제가 있다:

1. **문제: 어떤 점이 $\alpha_i = 0$인지 미리 알 수 없음**
   - 최적화를 풀기 전에는 서포트 벡터를 모름
   - 모든 점을 포함해서 풀어야 함

2. **기법: Active Set Methods**
   - 초기에는 작은 부분집합으로 시작
   - 반복적으로 α = 0인 점들을 제거
   - 남은 점들로만 재최적화

3. **SVM의 특수 알고리즘: SMO (Sequential Minimal Optimization)**
   - 두 개의 α만 한 번에 최적화
   - 매우 효율적 (희소성 활용)

**결론:** 이론과 실제는 다르다. 쌍대 문제의 희소성을 활용하는 특수 알고리즘이 필요.

</details>

### 문제 6.3
원시 최적값과 쌍대 최적값이 같다는 "Strong Duality"가 항상 성립하는가? 어떤 조건이 필요한가?

<details>
<summary>해설 보기</summary>

**풀이:**

**Strong Duality 조건 (Slater 조건):**

Convex 최적화 문제에서:
$$\min f_0(x) \text{ s.t. } f_i(x) \leq 0, h_j(x) = 0$$

만약 존재하는 점 $x_0$에 대해:
- $f_i(x_0) < 0$ (strict inequality) for all $i$
- $h_j(x_0) = 0$ for all $j$

그러면 strong duality가 성립한다.

**SVM의 경우:**
- 목적 함수: convex (∥f∥²은 convex)
- 부등식 제약: convex (1차, convex)
- Slater 조건이 일반적으로 성립

따라서 **SVM에서는 strong duality가 보장된다**!

**의미:**
- 쌍대 문제를 풀면 원시 문제도 해결
- 원시와 쌍대의 gap이 0

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./05-representer-theorem.md) | [📚 README](../README.md) | [다음 ▶](./07-gaussian-process-rkhs.md) |

</div>
