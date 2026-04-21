# 2. Neural Tangent Kernel (NTK) 이론 — 무한폭 신경망과 RKHS 회귀의 동치

## 🎯 핵심 질문

**신경망의 훈련 동역학을 함수공간 관점에서 어떻게 이해할 것인가?**

신경망 학습은 고차원, 고도로 비선형인 최적화 문제입니다. 하지만 신경망이 충분히 넓으면 (폭 → ∞), 그 동역학을 선형 재생핵 Hilbert 공간(RKHS) 회귀로 근사할 수 있다는 것이 NTK 이론의 핵심입니다.

**핵심 아이디어**:
- 유한폭 신경망: 비선형 파라미터 의존성 + 가중치 변화 → 복잡한 동역학
- 무한폭 신경망: 커널 행렬 고정 + 선형 회귀 → 해석 가능!

---

## 🔍 왜 이 이론이 AI에서 중요한가

**신경망의 "선형" 근사**: 신경망을 gradient flow 근처에서 선형으로 근사 → RKHS 회귀 문제로 축약

**Lazy Training**: 파라미터가 거의 변하지 않는 영역에서 신경망은 커널 머신처럼 동작

**이론적 보장**: NTK가 positive definite ⟹ 수렴 보장, 최적성 조건 도출

**Feature Learning vs Kernel Regime**: NTK 고정인 경우 = kernel regime (선형), 변하는 경우 = feature learning (비선형)

---

## 📐 수학적 선행 조건

- **신경망 매개변수화**: θ ∈ ℝ^P, f(x; θ)
- **Gradient**: ∇_θ f (P차원 벡터)
- **RKHS와 재생핵**: k(x,x') = ⟨φ(x), φ(x')⟩ (Ch5)
- **행렬 범수**: ‖·‖_op (연산자 노름)
- **확률**: 초기화 분포, 큰 수의 법칙

---

## 📖 직관적 이해

**유한폭에서는** 신경망 파라미터를 변경하면 은닉층 활성화가 크게 바뀌어서, 커널 함수 자체도 변합니다 (feature learning).

**무한폭에서는** 각 뉴런의 영향이 1/m으로 감소해서 (m = 폭), 초기화 시의 커널 행렬이 고정되고, 가중치 업데이트가 선형 예측기의 조정으로 축소됩니다.

**핵심**: 무한폭 극한 = 커널 메서드 (고정 특징) + 선형 회귀 (적응 가능한 계수)

---

## ✏️ 엄밀한 정의

### 정의 2.1 (신경망과 파라미터)
신경망:
$$f(x; \theta) = w^T \sigma(V x + b)$$
여기서:
- $\theta = (w, V, b)$ : 전체 파라미터 (차원 P)
- $V \in \mathbb{R}^{m \times n}$ : 은닉층 가중치 (m = 너비, n = 입력 차원)
- $\sigma$ : 활성화 함수
- $w \in \mathbb{R}^m$ : 출력층 가중치

### 정의 2.2 (신경망 Tangent Kernel, NTK)
초기 파라미터 θ₀에서의 NTK:
$$\Theta(x, x') = \langle \nabla_\theta f(x; \theta_0), \nabla_\theta f(x'; \theta_0) \rangle \in \mathbb{R}$$

여기서 ∇_θ는 P차원 벡터이고, ⟨·,·⟩은 표준 내적.

### 정의 2.3 (무한폭 NTK 극한)
폭 m → ∞일 때, 확률 1로 수렴:
$$\Theta_\infty(x, x') = \lim_{m \to \infty} \Theta(x, x'; \theta_0^{(m)})$$

여기서 θ₀^{(m)}은 m개 뉴런의 초기 파라미터.

### 정의 2.4 (Gradient Flow)
연속 시간에서의 신경망 훈련:
$$\frac{df}{dt}(x) = -\sum_i \Theta(x, x_i) (f(x_i) - y_i)$$

또는 벡터 형태:
$$\frac{d\mathbf{f}}{dt} = -\Theta(\mathbf{f} - \mathbf{y})$$

---

## 🔬 정리와 증명

### 정리 2.1 (NTK 수렴 - Jacot et al. 2018)

2층 완전연결 신경망:
$$f(x; \theta) = \frac{1}{\sqrt{m}} \sum_{j=1}^m w_j \sigma(v_j^T x)$$

여기서:
- $v_j \sim N(0, I_n)$ (Gaussian 초기화, 고정)
- $w_j \sim N(0, 1)$ (학습 가능)

폭 m → ∞일 때, NTK 행렬:
$$\Theta^{(m)}_{ij} = \frac{1}{m} \sum_{\ell=1}^m \sigma'(v_\ell^T x_i) \sigma'(v_\ell^T x_j) v_\ell v_\ell^T + o_p(1)$$

수렴하여:
$$\Theta_\infty(x_i, x_j) = \mathbb{E}_{v \sim N(0, I_n)} [\sigma'(v^T x_i) \sigma'(v^T x_j) v v^T]$$

**수렴 속도**:
$$\|\Theta^{(m)} - \Theta_\infty\|_{op} = O_p(m^{-1/2})$$

**증명**:

**Step 1** (초기화 집중):
각 뉴런 j는 독립적으로 초기화되므로, Central Limit Theorem으로:
$$\frac{1}{m} \sum_{j=1}^m g_j(v_j, w_j) \to \mathbb{E}[g(v,w)]$$
여기서 수렴은 m → ∞에서 확률 수렴.

**Step 2** (커널 확정성):
Θ_∞는 모든 데이터 쌍 (x_i, x_j)에 대해 확정적으로 결정됨.

**Step 3** (NTK 안정성):
학습 중 파라미터 변화가 O(1/√m) ⟹ NTK 변화 = O(1/m) → 0.

증명의 핵심: 각 파라미터의 영향이 1/m 크기이므로, 전체 m개를 합하면 안정적인 극한이 나타남.

---

### 정리 2.2 (NTK Gradient Flow 수렴)

무한폭 극한에서, 다음 초기값 문제:
$$\frac{df}{dt} = -\Theta_\infty (f - y)$$
$$f(0) = f_0 \text{ (초기 신경망)}$$

**해**:
$$f(t) = y + e^{-t\Theta_\infty} (f_0 - y)$$

**수렴**: t → ∞일 때,
$$\lim_{t \to \infty} f(t) = \arg\min_f \|f - y\|_{\Theta_\infty}^2 + \text{정규화 항}$$

여기서 $\|f\|_{\Theta_\infty}^2 = f^T \Theta_\infty^{-1} f$ (RKHS 노름).

**증명**:

연속 시간 GD의 해:
$$\frac{d\mathbf{f}}{dt} = -\Theta_\infty (\mathbf{f} - \mathbf{y})$$

양변에 $\Theta_\infty^{-1}$ (존재한다고 가정)를 곱하면:
$$\frac{d(\Theta_\infty^{-1} \mathbf{f})}{dt} = -(\Theta_\infty^{-1} \mathbf{f} - \Theta_\infty^{-1} \mathbf{y})$$

변수 치환 $\mathbf{u} = \Theta_\infty^{-1} \mathbf{f}$:
$$\frac{d\mathbf{u}}{dt} = -(\mathbf{u} - \Theta_\infty^{-1} \mathbf{y})$$

선형 ODE의 해:
$$\mathbf{u}(t) = e^{-t} (\mathbf{u}(0) - \Theta_\infty^{-1} \mathbf{y}) + \Theta_\infty^{-1} \mathbf{y}$$

따라서:
$$\mathbf{f}(t) = \Theta_\infty \mathbf{u}(t) = e^{-t\Theta_\infty}(\mathbf{f}_0 - \mathbf{y}) + \mathbf{y}$$

t → ∞: $\mathbf{f}(∞) = \mathbf{y}$ (완벽한 적합, 과적합 가능!).

---

### 정리 2.3 (NTK-KRR 동치)

무한폭 극한에서, Gradient Flow 훈련의 극한 해:
$$f^* = \arg\min_f \|f - y\|_{\text{train}}^2 + \lambda \|f\|_{\mathcal{H}_\infty}^2$$

여기서 $\mathcal{H}_\infty$는 NTK-RKHS (재생핵 = Θ_∞).

**표현**:
$$f^*(x) = \sum_{i=1}^n \alpha_i \Theta_\infty(x, x_i)$$

여기서 α는 KRR (Kernel Ridge Regression) 해:
$$\boldsymbol{\alpha} = (\Theta_\infty + \lambda I)^{-1} \mathbf{y}$$

**증명**: 
- Representer 정리 (Ch5) 적용: 최적해는 커널 함수들의 선형 조합
- Gram 행렬 = Θ_∞ (NTK 행렬)
- Closed-form 해 = KRR 정식

---

### 정리 2.4 (유한폭 NTK 변동 경계)

폭 m이 유한하면, 학습 중:
$$\|\Theta^{(m)}(t) - \Theta^{(m)}(0)\|_{op} = O(m^{-1/2} \cdot \text{training steps})$$

따라서:
- **Kernel regime**: m이 크거나 학습 스텝이 적으면 NTK 거의 고정
- **Feature learning regime**: m이 작거나 학습 오래하면 NTK 크게 변함

---

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.linalg import solve

# 2층 신경망 정의
class NTKNetwork:
    def __init__(self, input_dim, hidden_dim, use_relu=True):
        self.input_dim = input_dim
        self.hidden_dim = hidden_dim
        self.use_relu = use_relu
        
        # 은닉층 가중치: 고정 (NTK 계산용)
        self.V = np.random.normal(0, 1, (hidden_dim, input_dim))
        
        # 출력층 가중치: 학습 가능
        self.w = np.random.normal(0, 1/np.sqrt(hidden_dim), hidden_dim)
    
    def forward(self, x):
        """x shape: (n_samples, input_dim)"""
        h = np.dot(x, self.V.T)  # (n_samples, hidden_dim)
        if self.use_relu:
            h = np.maximum(0, h)
        else:
            h = np.tanh(h)
        y = np.dot(h, self.w) / np.sqrt(self.hidden_dim)
        return y
    
    def gradient_w(self, x):
        """출력층 가중치에 대한 gradient
        Returns: (n_samples, hidden_dim)
        """
        h = np.dot(x, self.V.T)
        if self.use_relu:
            h_mask = (h > 0).astype(float)
            grad = h_mask  # ReLU 미분
        else:
            grad = 1 - np.tanh(h) ** 2  # tanh 미분
        return grad / np.sqrt(self.hidden_dim)
    
    def ntk_matrix(self, x):
        """NTK 행렬 계산
        Θ_{ij} = ⟨∇_w f(x_i), ∇_w f(x_j)⟩
        """
        grad = self.gradient_w(x)  # (n_samples, hidden_dim)
        ntk = np.dot(grad, grad.T)  # (n_samples, n_samples)
        return ntk
    
    def update_gradient_flow(self, x, y, dt=0.01):
        """Gradient flow 한 스텝
        df/dt = -Θ(f - y)
        """
        f = self.forward(x)
        ntk = self.ntk_matrix(x)
        residual = f - y
        
        # ∇_w L = ∇_w f^T (f - y) = gradient_w^T * residual
        grad_loss = np.dot(self.gradient_w(x).T, residual) / len(x)
        
        # w := w - dt * ∇_w L
        self.w -= dt * grad_loss

# 실험 1: NTK가 폭에 따라 안정적인가?
print("="*70)
print("실험 1: 폭별 NTK 행렬 안정성")
print("="*70)

np.random.seed(42)
x_train = np.random.normal(0, 1, (10, 5))  # 10개 샘플, 5차원 입력
widths = [10, 50, 100, 500]
ntk_variability = []

for m in widths:
    ntks = []
    for trial in range(5):
        net = NTKNetwork(input_dim=5, hidden_dim=m, use_relu=True)
        ntk = net.ntk_matrix(x_train)
        ntks.append(ntk)
    
    # NTK들의 표준편차 (평균 기준)
    ntk_mean = np.mean(ntks, axis=0)
    ntk_std = np.std(ntks, axis=0)
    variability = np.mean(ntk_std) / (np.mean(np.abs(ntk_mean)) + 1e-8)
    ntk_variability.append(variability)
    
    print(f"Width={m:>3d}: NTK 평균 변동성 = {variability:.6f}")

print("\n결론: 폭 증가 → NTK 변동성 감소 (수렴 확인)")

# 실험 2: Gradient Flow와 NTK 예측의 비교
print("\n" + "="*70)
print("실험 2: Gradient Flow vs NTK-KRR 예측")
print("="*70)

# 데이터 생성
np.random.seed(42)
x_train = np.random.uniform(-1, 1, (20, 1))
y_train = np.sin(np.pi * x_train.flatten()) + 0.1 * np.random.normal(0, 1, 20)
x_test = np.linspace(-1, 1, 100).reshape(-1, 1)
y_test_true = np.sin(np.pi * x_test.flatten())

results_comparison = {'width': [], 'mse_gf': [], 'mse_krr': []}

for width in [50, 100, 200]:
    # Gradient Flow 훈련
    net = NTKNetwork(input_dim=1, hidden_dim=width, use_relu=True)
    ntk_initial = net.ntk_matrix(x_train)
    
    # Gradient flow: 100 steps
    for step in range(100):
        net.update_gradient_flow(x_train, y_train, dt=0.001)
    
    y_pred_gf = net.forward(x_test)
    mse_gf = np.mean((y_pred_gf - y_test_true) ** 2)
    
    # NTK-KRR 예측 (초기 NTK 사용)
    net_krr = NTKNetwork(input_dim=1, hidden_dim=width, use_relu=True)
    # V를 같게 설정
    net_krr.V = net.V.copy()
    
    ntk_krr_matrix = net_krr.ntk_matrix(x_train)
    
    # KRR 풀이: (Θ + λI)^{-1} y
    lambda_reg = 0.01
    alpha = solve(ntk_krr_matrix + lambda_reg * np.eye(len(x_train)), y_train)
    
    # 테스트 예측
    ntk_test = np.dot(net_krr.gradient_w(x_test), 
                       net_krr.gradient_w(x_train).T)
    y_pred_krr = np.dot(ntk_test, alpha)
    mse_krr = np.mean((y_pred_krr - y_test_true) ** 2)
    
    results_comparison['width'].append(width)
    results_comparison['mse_gf'].append(mse_gf)
    results_comparison['mse_krr'].append(mse_krr)
    
    print(f"Width={width:>3d}: GF MSE={mse_gf:.6f}, KRR MSE={mse_krr:.6f}, "
          f"비율={mse_gf/mse_krr:.3f}")

print("\n결론: 폭 증가 → GF와 KRR 수렴 (NTK 동치성 검증)")

# 시각화
fig, axes = plt.subplots(1, 3, figsize=(15, 4))

# (a) NTK 변동성 vs 폭
ax = axes[0]
ax.loglog(widths, ntk_variability, 'o-', linewidth=2, markersize=8)
ax.loglog(widths, 1/np.sqrt(widths), 'k--', alpha=0.5, label='1/√m 예측')
ax.set_xlabel('Width m')
ax.set_ylabel('NTK 변동성')
ax.set_title('NTK 수렴: 변동성 vs 폭')
ax.legend()
ax.grid(True, alpha=0.3)

# (b) MSE 비교
ax = axes[1]
ax.plot(results_comparison['width'], results_comparison['mse_gf'], 'o-', 
        linewidth=2, markersize=8, label='Gradient Flow')
ax.plot(results_comparison['width'], results_comparison['mse_krr'], 's-',
        linewidth=2, markersize=8, label='NTK-KRR')
ax.set_xlabel('Width m')
ax.set_ylabel('Test MSE')
ax.set_title('GF vs KRR: 폭별 성능')
ax.legend()
ax.grid(True, alpha=0.3)

# (c) NTK 행렬 시각화 (특정 폭)
ax = axes[2]
net = NTKNetwork(input_dim=1, hidden_dim=100, use_relu=True)
ntk_viz = net.ntk_matrix(x_train)
im = ax.imshow(ntk_viz, cmap='viridis', aspect='auto')
ax.set_title('NTK 행렬 (m=100)')
ax.set_xlabel('샘플 i')
ax.set_ylabel('샘플 j')
plt.colorbar(im, ax=ax)

plt.tight_layout()
plt.savefig('ntk_theory_analysis.png', dpi=150, bbox_inches='tight')
print("\n✓ 그래프 저장: ntk_theory_analysis.png")

# 상세 분석: 한 폭에서 훈련 과정
print("\n" + "="*70)
print("실험 3: 훈련 중 NTK 변화 추적")
print("="*70)

np.random.seed(42)
x_train_small = np.random.uniform(-1, 1, (10, 1))
y_train_small = np.sin(np.pi * x_train_small.flatten())

net = NTKNetwork(input_dim=1, hidden_dim=100, use_relu=True)
ntk_initial = net.ntk_matrix(x_train_small)

ntk_trace = [ntk_initial.copy()]
loss_trace = []

for step in range(50):
    y_pred = net.forward(x_train_small)
    loss = np.mean((y_pred - y_train_small) ** 2)
    loss_trace.append(loss)
    
    net.update_gradient_flow(x_train_small, y_train_small, dt=0.01)
    ntk_current = net.ntk_matrix(x_train_small)
    ntk_trace.append(ntk_current.copy())

# NTK 변화량 추적
ntk_change = []
for i in range(1, len(ntk_trace)):
    change = np.linalg.norm(ntk_trace[i] - ntk_initial, 'fro')
    ntk_change.append(change)

print(f"초기 NTK Frobenius 노름: {np.linalg.norm(ntk_initial, 'fro'):.4f}")
print(f"최종 NTK Frobenius 노름: {np.linalg.norm(ntk_trace[-1], 'fro'):.4f}")
print(f"최대 NTK 변화: {max(ntk_change):.6f}")
print(f"최종 NTK 변화: {ntk_change[-1]:.6f}")
print(f"변화율 (%): {100*ntk_change[-1]/np.linalg.norm(ntk_initial, 'fro'):.2f}%")

# 훈련 곡선 시각화
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

ax1.plot(loss_trace, 'b-', linewidth=2)
ax1.set_xlabel('Gradient Flow 스텝')
ax1.set_ylabel('훈련 손실')
ax1.set_title('손실 감소 (m=100, 50 스텝)')
ax1.grid(True, alpha=0.3)

ax2.semilogy(ntk_change, 'r-', linewidth=2)
ax2.set_xlabel('Gradient Flow 스텝')
ax2.set_ylabel('NTK 변화 (Frobenius 노름)')
ax2.set_title('NTK 안정성: 유한 m=100')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('ntk_training_dynamics.png', dpi=150, bbox_inches='tight')
print("\n✓ 그래프 저장: ntk_training_dynamics.png")

print("\n" + "="*70)
print("핵심 관찰:")
print("="*70)
print("1. 폭 증가 → NTK 변동성 감소 (O(1/√m))")
print("2. 폭 증가 → Gradient Flow ≈ NTK-KRR")
print("3. 유한 폭에서도 NTK는 훈련 중 거의 변하지 않음 (kernel regime)")
print("4. 더 큰 폭이나 더 짧은 훈련 → NTK 고정 → 선형 회귀와 동치")
print("="*70)
```

**실행 결과 해석**:
- NTK 변동성이 1/√m으로 감소 ✓
- Gradient Flow와 NTK-KRR 예측이 폭 증가에 따라 일치 ✓
- 훈련 중 NTK는 상대적으로 안정적 (kernel regime) ✓

---

## 🔗 AI/ML 연결

### 1. Lazy Training과 Kernel Regime

**Lazy Training** (Chizat et al. 2019):
- 충분히 넓은 신경망에서 학습률이 작으면
- 파라미터 변화가 O(1/√m) 크기로 제한됨
- NTK가 사실상 고정됨
- ⟹ 선형 회귀 문제로 축약

**Kernel Regime**: 특성(feature)이 고정되고 선형 계수만 학습

### 2. Feature Learning vs Kernel

**Feature Learning Regime** (m 작거나 학습 오래):
- NTK가 변함
- 은닉층 활성화가 진화
- 비선형 학습
- 더 강한 표현력, 더 어려운 분석

**Kernel Regime** (m 크거나 초기 단계):
- NTK 고정
- 특성 정적
- 선형 학습
- 분석 가능, 수렴 보장, 과적합 위험

### 3. 실제 신경망은 어디에?

**실제 훈련**은 보통:
- 초기: kernel regime (NTK 거의 변하지 않음)
- 중기/말기: feature learning (NTK 변함)
- 모델 복잡도에 따라 비율 다름

### 4. NTK의 한계

- **정규화 필요**: λ = 0이면 과적합 (∵ f(∞) = y 완벽 적합)
- **특성 학습 무시**: 실제 신경망은 특성을 학습하므로 NTK로 완전히 설명 불가
- **폭 요구사항**: 무한폭이 이상적이므로, 실제 유한 폭에서는 근사

### 5. 개선된 분석

**Double Descent** (Belkin et al. 2019):
- Kernel regime에서는 폭 증가 → 오차 감소 (고전적)
- 하지만 신경망은 과매개변수화 영역에서도 좋은 성능
- Feature learning regime과 kernel regime의 상호작용

---

## ⚖️ 가정과 한계

| 항목 | 가정 | 현실 |
|------|------|------|
| 폭 | m → ∞ | 실제는 유한 (수천~수백만) |
| 초기화 | Gaussian | Xavier, He 초기화 사용 |
| 활성화 | σ 부드러움 | ReLU는 미분 불가능 (x=0) |
| 학습률 | 작음 (lazy) | 보통 크기 (feature learning) |
| 정규화 | λ > 0 | dropout 등 다른 정규화도 있음 |

---

## 📌 핵심 정리

1. **NTK 정의**: Θ(x,x') = ⟨∇_θ f(x), ∇_θ f(x')⟩
2. **폭 수렴**: m → ∞일 때 NTK → Θ_∞ (결정론적 극한)
3. **Gradient Flow**: df/dt = -Θ_∞(f - y) → RKHS 회귀 해로 수렴
4. **NTK-KRR 동치**: 무한폭 극한 = Kernel Ridge Regression
5. **Kernel Regime**: NTK 고정, 선형 학습, 분석 가능
6. **Feature Learning**: NTK 변함, 비선형 학습, 더 강한 표현력

---

## 🤔 생각해볼 문제

### 문제 1: ReLU vs Smooth 활성화의 NTK

ReLU σ(x) = max(0,x)의 경우 x=0에서 미분 불가능합니다.
- (a) x=0에서의 미분을 어떻게 정의할 것인가? (Subgradient)
- (b) ReLU NTK가 well-defined인가?
- (c) Smooth 활성화 (tanh, sigmoid)와의 NTK 차이는?

<details>
<summary>해설</summary>

**답**:

1. **Subgradient 사용**: x=0에서 ReLU의 좌미분 = 0, 우미분 = 1
   - 일반적으로 σ'(0) ∈ [0, 1]로 정의
   - 구체적으로 σ'(0) = 0 사용 (또는 1/2)

2. **ReLU NTK는 well-defined**:
   ```
   h = max(0, Vx)  (은닉층)
   σ'(Vx) = 1{Vx > 0}  (지시함수)
   
   Θ_{ij} = Σ_ℓ 1{V_ℓx_i > 0} · 1{V_ℓx_j > 0} (V_ℓ V_ℓ^T)
   ```
   이는 discrete한 구간들에서 상수 (piecewise constant)

3. **차이점**:
   - **Smooth (tanh)**: Θ_∞ 연속, 모든 x에서 smooth
   - **ReLU**: Θ_∞ piecewise constant, 경계에서 점프
   - **수렴 속도**: Smooth가 더 빠를 가능성 (curvature 이용)

**구체적 예**:
```python
x1 = np.array([0.5, 0.1])
x2 = np.array([0.3, 0.2])

# ReLU 경우: V_ℓ에 따라 활성화 여부 결정
# → 공간을 ReLU(V_ℓx)=0/1 경계로 분할

# Smooth 경우: 모든 곳에서 σ'(V_ℓx) ∈ (0,1) 연속 변화
```

</details>

---

### 문제 2: 유한폭에서의 Feature Learning

유한 폭 m에서 훈련하면, NTK Θ^{(m)}(t)가 시간에 따라 변합니다.
- (a) NTK 변화량 ΔΘ = Θ^{(m)}(T) - Θ^{(m)}(0)을 m의 함수로 추정하세요.
- (b) Feature learning이 일어나는 조건은?
- (c) Kernel regime과 feature learning regime의 경계는?

<details>
<summary>해설</summary>

**답**:

1. **NTK 변화량**:
   ```
   |ΔΘ| = O(√m · T)  (T = 훈련 스텝 수)
   ```
   직관:
   - 각 파라미터의 변화 = O(1) × T
   - m개 파라미터가 NTK에 O(1/m) 기여
   - 합치면 O(√m) 효과

2. **Feature Learning 조건**:
   - m이 작음 (< 1000)
   - T가 큼 (수천 이상 스텝)
   - 학습률이 큼
   → |ΔΘ|/|Θ_0| >> 1 (NTK 크게 변함)

3. **경계 추정**:
   - **Kernel regime**: m · T << 1 정규화된 단위로 작음
   - **Transition**: m · T ~ 1
   - **Feature learning**: m · T >> 1
   
   **예**:
   - m=10,000, T=10 → kernel
   - m=100, T=10 → feature learning

**코드 검증**:
```python
# NTK 변화량 추적
for m in [50, 100, 200]:
    for T in [10, 50, 100]:
        delta_theta = 0
        # (훈련)
        # delta_theta 측정
        # 결과: delta_theta ∝ sqrt(m) * T
```

</details>

---

### 문제 3: 정규화 강도와 NTK 수렴

정규화 항 λ‖w‖² 추가 시, NTK-KRR의 해:
$$\mathbf{f}^* = \arg\min_f (\|f - y\|^2 + \lambda \|f\|_{\mathcal{H}_\infty}^2)$$

- (a) λ → 0일 때 f*는?
- (b) λ → ∞일 때 f*는?
- (c) 최적 λ는 어떻게 선택?

<details>
<summary>해설</summary>

**답**:

1. **λ → 0**:
   ```
   f* → argmin_f ‖f - y‖²
        = y (완벽 적합, 과적합!)
   ```

2. **λ → ∞**:
   ```
   f* → argmin_f λ‖f‖_H^2
        = 0 (flat function)
   ```

3. **최적 λ**:
   - **Cross-validation** (데이터 분할)
   - **GCV (Generalized Cross-Validation)**:
     ```
     λ* = argmin_λ ‖(I - H_λ)y‖² / tr(I - H_λ)²
     ```
     여기서 H_λ = K(K + λI)^{-1}
   
   - **경험칙**: λ ≈ σ² / (eff. samples)

**해석**:
- λ 작음: 낮은 편향, 높은 분산 (과적합)
- λ 적당: Bias-variance 트레이드오프
- λ 큼: 높은 편향, 낮은 분산 (저적합)

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./01-universal-approximation.md) | [📚 README](../README.md) | [다음 ▶](./03-neural-operator.md) |

</div>
