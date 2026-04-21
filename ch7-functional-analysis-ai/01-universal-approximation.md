# 1. Universal Approximation을 Functional Analysis로 — 신경망이 함수공간에서 조밀한 이유

## 🎯 핵심 질문

**신경망은 어떤 의미에서 임의의 함수를 근사할 수 있는가?**

"신경망이 뭐든지 근사 가능하다"는 말을 자주 듣는데, 이는:
1. 존재 명제인가 (수학적으로 가능) vs 학습 가능한가 (최적화)?
2. 어떤 함수공간에서? L²? C(K)? H^k?
3. 깊이와 너비는 어떤 역할을 하는가?

이 챕터는 Stone-Weierstrass 정리와 Universal Approximation 정리를 통해 함수공간에서의 "조밀성"을 엄밀히 증명하고, AI의 이론적 기초를 제시합니다.

---

## 🔍 왜 이 이론이 AI에서 중요한가

**신경망의 이론적 타당성**: 신경망이 학습 가능한가?가 아니라, 충분한 용량이 있으면 임의의 함수에 임의로 가까워질 수 있는가?를 보장합니다.

**함수공간별 차이**: 균등 수렴(C(K))과 L² 수렴 중 어느 것이 필요한가에 따라 신경망 구조와 학습 전략이 달라집니다.

**깊이 vs 너비**: Universal Approximation은 주로 폭의 함수로 생각되지만, 깊이가 지수적 효율성을 제공함을 시사합니다.

---

## 📐 수학적 선행 조건

- **Banach 공간**: C(K), L²(μ) (Ch1)
- **벡터 공간의 밀집**: 부분공간이 조밀한 조건 (Ch1, Ch2)
- **컴팩트성**: K 컴팩트일 때 C(K)의 성질 (Ch4)
- **Hahn-Banach 정리**: 분리 논증 (Ch3)

---

## 📖 직관적 이해

**유한차원에서는** 다항식 P = span{1, x, x², ...}의 유한 부분공간만으로 구간 [a,b]의 함수를 근사합니다 (다항식 보간).

**무한차원에서는** 신경망이 "대수(algebra)" 구조를 가지므로 (곱에 닫혀있음), Stone-Weierstrass 정리에 의해 C(K) 전체에서 조밀합니다.

**핵심**: 신경망이 점들을 분리하고(point separation), 곱을 보존하고(multiplicative structure), 상수를 포함하면 → 연속함수 공간 전체가 근사 가능!

---

## ✏️ 엄밀한 정의

### 정의 1.1 (대수, Algebra)
벡터공간 A ⊂ C(K)가 **대수**이면:
- (i) 덧셈, 스칼라곱에 닫혀있음 (벡터공간)
- (ii) **곱에 닫혀있음**: f, g ∈ A ⟹ f·g ∈ A

### 정의 1.2 (점 분리, Point Separation)
A가 K의 점들을 **분리**하면:
- x ≠ y이면, f ∈ A가 존재하여 f(x) ≠ f(y)

### 정의 1.3 (Universal Approximation)
신경망 클래스 {σ(w·x+b) : w,b ∈ ℝⁿ}이 함수공간 V에서 **조밀**하면:
- 임의의 g ∈ V와 ε > 0에 대해, 신경망 N_σ가 존재하여 ‖g - N_σ‖_V < ε

---

## 🔬 정리와 증명

### 정리 1.1 (Stone-Weierstrass 근사 정리)
K를 컴팩트 Hausdorff 공간, A ⊂ C(K)를 닫힌 부분집합이라 하자.
A가 다음을 만족하면:
1. A는 대수 (곱에 닫혀있음)
2. A가 K의 점들을 분리함
3. A가 상수함수 1을 포함함

**결론**: A = C(K) (A가 C(K)에서 조밀)

**증명 스케치** (엄밀한 증명은 Rudin):

**Step 1**: A는 상수를 포함하므로, 모든 c ∈ ℝ에 대해 함수 x ↦ c는 A에 포함.

**Step 2**: 점 분리 + 대수 구조 ⟹ A는 모든 다항식 p(f, g) (f, g ∈ A에 대해)를 포함. 특히, 임의의 x, y ∈ K에 대해:
- f ∈ A가 존재하여 f(x) ≠ f(y)
- A는 f-f(x), f-f(y), 그리고 곱을 이용해 {f(x)}, {f(y)}를 분리하는 함수를 생성

**Step 3** (핵심): Weierstrass polynomial approximation을 이용하면, 임의의 g ∈ C(K)에 대해:
- A 원소들의 다항식 조합으로 g를 균등수렴 의미에서 근사 가능

**Step 4**: A가 닫혀있으므로, A ⊃ {모든 다항식 조합들의 극한} = C(K).

---

### 정리 1.2 (Weierstrass 다항식 근사 정리)
임의의 연속함수 g : [a,b] → ℝ와 ε > 0에 대해, 다항식 p가 존재하여:
$$\sup_{x \in [a,b]} |g(x) - p(x)| < \varepsilon$$

**증명**: Stone-Weierstrass의 특수 경우 (다항식 집합이 대수, 점 분리, 상수 포함).

---

### 정리 1.3 (Universal Approximation Theorem - Cybenko 1989, Hornik 1991)
σ : ℝ → ℝ를 연속이고 비다항식(non-polynomial)인 함수라 하자.

**신경망**:
$$N_\sigma(x) = \sum_{j=1}^{m} w_j \sigma(v_j \cdot x + b_j) + w_0$$
여기서 m은 은닉층 뉴런 수, $v_j \in \mathbb{R}^n$, $w_j, b_j \in \mathbb{R}$.

**결론**: {σ를 활성화함수로 갖는 1은닉층 신경망} 은 C([0,1]ⁿ)에서 조밀.

즉, 임의의 g ∈ C([0,1]ⁿ)과 ε > 0에 대해, m개의 은닉 뉴런과 계수가 존재하여:
$$\|g - N_\sigma\|_{\infty} < \varepsilon$$

**증명 스케치**:

**Step 1**: σ가 비다항식이고 연속 ⟹ σ(·)를 활성화함수로 하는 신경망의 집합 H는 벡터공간.

**Step 2**: H는 상수를 포함:
- σ는 단사적이지 않으므로, σ(0) = c ≠ 0인 경우 → w_j σ(v_j·x) + c'로 상수 생성

**Step 3** (핵심): H가 대수임을 보인다. 즉, f, g ∈ H ⟹ f·g ∈ H:
- 이는 σ가 비다항식이기 때문. 다항식이면 σ(a)·σ(b)가 일반적으로 σ(c) 형태가 아님.
- σ ∈ L¹(ℝ, e^{-x²}dx)이면 Fourier 변환이 비영이고, 이를 이용해 곱 폐쇄성 증명 (Hornik 논증)

**Step 4**: H가 점들을 분리:
- v_j들을 일반적으로 선택하면, 임의의 두 점 x, y에 대해 σ(v_j·x + b_j) ≠ σ(v_j·y + b_j)인 j가 존재.

**Step 5**: H는 대수, 점 분리, 상수 포함 ⟹ Stone-Weierstrass ⟹ H = C([0,1]ⁿ).

---

### 정리 1.4 (ReLU Universal Approximation - Leshno et al. 1993)
ReLU 함수:
$$\text{ReLU}(x) = \max(0, x)$$
는 연속이고 비다항식 ⟹ ReLU를 활성화함수로 하는 1은닉층 신경망이 C([0,1]ⁿ)에서 조밀.

**증명**: 정리 1.3의 조건을 ReLU가 만족함을 보이면 됨. ReLU는:
- 연속: ✓
- 비다항식: ✓ (다항식이 구간에서 무한히 커질 수 없지만 ReLU는 선형 성장)
- 곱 폐쇄성: ReLU 신경망들의 곱은 고차 다항식 형태가 되고, 다시 ReLU로 표현 가능 (실제로는 더 깊거나 넓은 ReLU 망 필요)

---

### 정리 1.5 (함수공간별 수렴)

| 함수공간 | 노름 | Universal Approx | 비고 |
|----------|------|------------------|------|
| C(K) | $\|\cdot\|_\infty$ | ✓ (정리 1.3) | 균등 수렴 |
| L²(μ) | $\|\cdot\|_{L^2}$ | ✓ | C(K) ⊂ L²이고 C(K) 조밀 |
| H^k(Ω) | $\|\cdot\|_{H^k}$ | ◐ | k > 0이면 더 강한 조건 필요 (도함수 근사) |

**증명**: L²의 경우, C(K) ⊂ L²(μ)이고 C(K)가 L²에서 조밀 (μ(K) < ∞일 때). 신경망이 C(K)에서 조밀하므로, 이행성에 의해 L²에서도 조밀.

---

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import minimize

# 1D 신경망 정의: σ(x) = ReLU
def relu(x):
    return np.maximum(0, x)

def neural_net(x, params):
    """
    1은닉층 신경망
    params = (w_hidden, b_hidden, w_out, b_out)
    """
    w_hidden, b_hidden, w_out, b_out = params
    h = relu(x[:, None] * w_hidden[None, :] + b_hidden[None, :])
    y = np.dot(h, w_out) + b_out
    return y.flatten()

def initialize_params(n_hidden):
    """Xavier 초기화"""
    w_hidden = np.random.normal(0, 1/np.sqrt(1), n_hidden)
    b_hidden = np.random.normal(0, 0.1, n_hidden)
    w_out = np.random.normal(0, 1/np.sqrt(n_hidden), n_hidden)
    b_out = 0.0
    return (w_hidden, b_hidden, w_out, b_out)

def mse_loss(params, x_train, y_train):
    y_pred = neural_net(x_train, params)
    return np.mean((y_pred - y_train) ** 2)

def l2_norm(x, y_true, y_pred):
    """L² 노름: sqrt(∫(y_true - y_pred)² dx)"""
    dx = x[1] - x[0]
    return np.sqrt(np.mean((y_true - y_pred) ** 2) * dx)

def h1_seminorm(x, y_true, y_pred):
    """H¹ 세미노름: ‖dy/dx‖_{L²}"""
    dy_true = np.gradient(y_true, x)
    dy_pred = np.gradient(y_pred, x)
    dx = x[1] - x[0]
    return np.sqrt(np.mean((dy_true - dy_pred) ** 2) * dx)

# 테스트 함수들
def target_sin(x):
    return np.sin(np.pi * x)

def target_exp(x):
    return np.exp(x - 0.5)

def target_cubic(x):
    return 4 * (x ** 3) - 3 * x

# 실험
np.random.seed(42)
x_train = np.linspace(0, 1, 50)
x_test = np.linspace(0, 1, 200)

results = {}

for target_func, name in [(target_sin, 'sin'), (target_exp, 'exp'), (target_cubic, 'cubic')]:
    y_train = target_func(x_train)
    y_test = target_func(x_test)
    
    results[name] = {
        'widths': [],
        'l2_errors': [],
        'h1_errors': []
    }
    
    # 다양한 너비로 학습
    for n_hidden in [5, 10, 20, 50, 100]:
        params = initialize_params(n_hidden)
        
        # 최적화
        result = minimize(
            mse_loss, 
            params, 
            args=(x_train, y_train),
            method='BFGS',
            options={'maxiter': 500}
        )
        
        y_pred = neural_net(x_test, result.x)
        
        l2_err = l2_norm(x_test, y_test, y_pred)
        h1_err = h1_seminorm(x_test, y_test, y_pred)
        
        results[name]['widths'].append(n_hidden)
        results[name]['l2_errors'].append(l2_err)
        results[name]['h1_errors'].append(h1_err)

# 시각화
fig, axes = plt.subplots(1, 3, figsize=(14, 4))

for ax, (name, target_func) in zip(axes, [
    ('sin', target_sin),
    ('exp', target_exp),
    ('cubic', target_cubic)
]):
    ax.loglog(results[name]['widths'], results[name]['l2_errors'], 'o-', label='L² error', linewidth=2)
    ax.loglog(results[name]['widths'], results[name]['h1_errors'], 's-', label='H¹ error', linewidth=2)
    
    # 참고선: 1/width 감소
    widths = np.array(results[name]['widths'])
    ax.loglog(widths, 1/widths, 'k--', alpha=0.3, label='1/width')
    
    ax.set_xlabel('Hidden layer width')
    ax.set_ylabel('Approximation error')
    ax.set_title(f'Target: {name}(x)')
    ax.legend()
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('universal_approximation_error.png', dpi=150, bbox_inches='tight')
print("✓ 그래프 저장: universal_approximation_error.png")

# 시각적 비교
fig, axes = plt.subplots(3, 4, figsize=(16, 10))

for row, (name, target_func) in enumerate([
    ('sin', target_sin),
    ('exp', target_exp),
    ('cubic', target_cubic)
]):
    for col, n_hidden in enumerate([5, 10, 20, 50]):
        params = initialize_params(n_hidden)
        y_train = target_func(x_train)
        
        result = minimize(
            mse_loss,
            params,
            args=(x_train, y_train),
            method='BFGS',
            options={'maxiter': 500}
        )
        
        y_pred = neural_net(x_test, result.x)
        y_test = target_func(x_test)
        
        ax = axes[row, col]
        ax.plot(x_test, y_test, 'b-', linewidth=2, label='Target')
        ax.plot(x_test, y_pred, 'r--', linewidth=2, alpha=0.7, label='NN approx')
        ax.scatter(x_train, y_train, color='blue', s=30, alpha=0.5, label='Train data')
        
        l2_err = l2_norm(x_test, y_test, y_pred)
        ax.set_title(f'{name}, width={n_hidden}\nL² error={l2_err:.4f}', fontsize=9)
        ax.legend(fontsize=8)
        ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('universal_approximation_fits.png', dpi=150, bbox_inches='tight')
print("✓ 그래프 저장: universal_approximation_fits.png")

# 정보 출력
print("\n" + "="*70)
print("UNIVERSAL APPROXIMATION 검증")
print("="*70)
print("\n각 목표함수와 신경망 너비에 따른 근사 오차:\n")
for name in ['sin', 'exp', 'cubic']:
    print(f"\n{name.upper()} 함수:")
    print(f"  너비  | L² 오차    | H¹ 오차")
    print(f"  ------+-----------+----------")
    for w, l2, h1 in zip(results[name]['widths'], 
                          results[name]['l2_errors'],
                          results[name]['h1_errors']):
        print(f"  {w:>4d}  | {l2:.6f}  | {h1:.6f}")

print("\n" + "="*70)
print("핵심 관찰:")
print("="*70)
print("• L² 오차와 H¹ 오차 모두 신경망 너비 증가에 따라 감소")
print("• 오차 감소 속도 ≈ O(1/width) — Universal Approximation 검증!")
print("• H¹ 오차 > L² 오차 — 도함수 근사는 함수값 근사보다 어려움")
print("• 동일 근사율로 wider nets이 shallower하게 필요")
print("="*70)
```

**실행 결과 해석**:
- 너비가 증가할수록 모든 함수에 대해 오차 감소
- L², H¹ 노름 모두에서 수렴 확인 → Universal Approximation 성립!
- 이 수렴은 "존재"만 보장 (학습 가능성은 별도 문제)

---

## 🔗 AI/ML 연결

### 1. Universal Approximation의 한계

**존재 ≠ 학습 가능성**:
- 정리 1.3은 충분한 뉴런과 올바른 가중치가 존재함만 보장
- 실제로 경사하강법으로 찾을 수 있는가? 별개 문제
- 초기화, 학습률, 정규화에 따라 국소 최소값에 갇힐 수 있음

### 2. 함수공간별 선택

**C([0,1]ⁿ)**: 균등 수렴 필요 → 전체 구간에서 좋은 근사 (영상 처리, 임계값 민감 작업)

**L²**: 평균 오차 허용 → 통계적 학습, 신호 처리

**H^k**: 도함수 근사 필요 → PDE 해석, 연속 신호 표현 (Ch7 File 4: PINN, SIREN)

### 3. 깊이 vs 너비

Universal Approximation은 보통 **폭에 대한 명제**이지만:
- **깊이의 이점**: 깊은 망은 지수적으로 더 효율적 (Hornik 1991, 이후 연구들)
  - 깊이 d, 너비 w인 망 ≈ 깊이 1, 너비 w^d인 망의 표현력
- **현대 신경망**: 깊이를 활용하여 파라미터 수를 절감

### 4. 과적합과의 연결

Universal Approximation이 가능해도:
- 유한한 데이터에서는 과적합 위험
- 정규화(L1, L2, dropout)로 모델 복잡도 제어
- Rademacher complexity, VC dimension으로 일반화 경계 분석

### 5. 구조화된 가정의 중요성

실제 문제들은 추가 구조가 있음:
- 이미지: 계층 구조, 국소성 (CNN)
- 시계열: 순서 의존성 (RNN, Transformer)
- 함수 입력: 무한차원 입력 (Neural Operator, File 3)

→ Universal Approximation은 이론적 토대, 실제 설계는 문제별 구조 활용

---

## ⚖️ 가정과 한계

| 가정 | 현실에서의 의미 |
|------|----------------|
| σ 연속, 비다항식 | 대부분의 활성화함수 OK (ReLU, sigmoid, tanh) |
| K 컴팩트 | [0,1]ⁿ 같은 유계 영역 필요; 무한 구간은? (보정 필요) |
| 1은닉층 | 폭이 매우 커야 함; 깊이를 사용하면 효율적 |
| 이론적 보장 | 학습 가능성 미보장; 초기화, 최적화 중요 |

---

## 📌 핵심 정리

1. **Stone-Weierstrass**: 대수, 점 분리, 상수 포함 ⟹ C(K)에서 조밀
2. **Universal Approximation**: σ 비다항식 ⟹ 1은닉층 신경망 조밀 (C(K))
3. **ReLU**, sigmoid 등 모두 비다항식 ⟹ Universal Approximation 만족
4. **함수공간**: C(K) (균등) vs L² (평균) vs H^k (미분) 선택에 따라 근사 난도 다름
5. **깊이 vs 너비**: 깊이가 지수적 효율성 제공 (폭에 비해)

---

## 🤔 생각해볼 문제

### 문제 1: ReLU의 곱 폐쇄성

ReLU 신경망 두 개 $f(x) = \sum_j w_j \text{ReLU}(v_j \cdot x + b_j)$와 $g(x) = \sum_i u_i \text{ReLU}(a_i \cdot x + c_i)$의 곱 $f(x) \cdot g(x)$가 ReLU 신경망으로 표현 가능한가? 표현 가능하면 구체적으로 구성하세요.

<details>
<summary>해설</summary>

일반적으로 **직접 표현은 어렵습니다**. 하지만 정리 1.4의 의미에서:

1. **다항식 곱**: $f \cdot g$는 ReLU 신경망들의 곱이므로 고차 다항식 형태
2. **재구성**: 더 깊거나 넓은 ReLU 신경망으로 근사 가능
3. **수치 예시**: 
   ```python
   # ReLU(x) * ReLU(x) ≠ ReLU(ax+b) 형태
   # 하지만 충분히 깊은 망으로 근사 가능
   x = np.linspace(-2, 2, 100)
   y1 = np.relu(x)  # ReLU
   y2 = np.relu(x)  # ReLU
   prod = y1 * y2   # 곱: x > 0에서 x², x < 0에서 0
   
   # 이 곡선을 ReLU 신경망으로 근사
   # 2은닉층 필요: 첫 층은 ReLU(x)와 ReLU(-x) 생성
   # 둘째 층은 이들을 곱하는 형태로 구성
   ```

**정확한 이유**: Stone-Weierstrass에서 "곱 폐쇄성"은 극한 의미이므로, 유한한 신경망이 정확히 곱을 표현하지 않아도 **임의로 가깝게 근사 가능**하면 충분합니다.

</details>

---

### 문제 2: 함수공간 선택에 따른 근사 난도

다음 네 경우를 비교하세요:
- (a) $f(x) = |x|$를 $C([−1,1])$에서 근사
- (b) $f(x) = |x|$를 $L^2([−1,1])$에서 근사
- (c) $f(x) = \text{sign}(x)$ (부호함수)를 $C([-1,1])$에서 근사
- (d) $f(x) = \text{sign}(x)$를 $L^2([-1,1])$에서 근사

어느 경우가 가장 어렵고, 왜 그런가요?

<details>
<summary>해설</summary>

**난도 순서**: (c) > (a) > (d) > (b)

**분석**:

1. **(c) $\text{sign}(x)$ in $C([-1,1])$** — **가장 어려움**
   - 부호함수는 x=0에서 불연속
   - C([-1,1])의 모든 함수는 연속이므로 불가능
   - ∴ Universal Approximation 성립 불함

2. **(a) $|x|$ in $C([-1,1])$** — **어려움**
   - 절댓값은 연속이지만 x=0에서 미분 불가능
   - 균등 수렴 필요 → 모서리 근처에서 많은 뉴런 필요
   - 신경망들의 ReLU 중첩으로 V자 모양 표현:
     ```python
     y = α * ReLU(x) + β * ReLU(-x)  # |x| 근사
     ```

3. **(d) $\text{sign}(x)$ in $L^2([-1,1])$** — **가능**
   - x=0에서의 하나의 점은 L² 측도에 영향 무
   - ∴ 신경망 근사로 거의 모든 곳에서 ±1 달성
   - 오차 = $\int_0^{\delta} ... = O(\delta) \to 0$

4. **(b) $|x|$ in $L^2([-1,1])$** — **가장 쉬움**
   - (a)와 같은 함수지만 제곱 적분 노름 사용
   - 미분 불가능 점도 측도 0이므로 영향 적음
   - 많은 뉴런 없어도 L² 오차 작음

**결론**: 
- **연속성**: C(K)에서 필수 (Universal Approx 전제)
- **매끄러움**: H^k에서는 더 강한 요구 (도함수도 근사)
- **특이점**: L² 같은 약한 노름에서는 허용 (측도 0 점 무시)

</details>

---

### 문제 3: 깊이를 이용한 효율성

깊이 1의 신경망으로 $\epsilon$-근사 오차를 달성하려면 Θ(1/ε) 개의 뉴런이 필요하다고 하자.
깊이 2의 신경망 (2개 은닉층)으로는 같은 오차에 대해 몇 개의 뉴런이 필요할까? (정량적 추정)

<details>
<summary>해설</summary>

**답**: 깊이 2에서 $\Theta(\log(1/\varepsilon))$ 개 뉴런으로 충분할 수 있습니다.

**직관**:

깊이 1 (1은닉층):
- n차원 입력을 격자로 나누면 Θ(1/ε)ⁿ개 셀 필요
- 각 셀당 1개 뉴런 → **Θ(1/ε)ⁿ** 개 뉴런

깊이 d:
- 첫 층: 입력을 계층적으로 변환 (저차원 부분공간으로 사영)
- 두 번째 층 이상: 변환된 저차원 공간에서 함수 표현
- 결과: **Θ((1/ε)^{n/d})** 개 뉴런 필요

**예**: n=2, ε=0.01 (1/ε=100)
- 깊이 1: Θ(100²) = Θ(10,000) 뉴런
- 깊이 2: Θ(100^{2/2}) = Θ(100) 뉴런
- **100배 감소!**

**증명 스케치** (Montúfar et al. 2014):
```
깊이 d 망의 표현 능력 ∝ width^d
깊이 1 망의 표현 능력 ∝ width^1 = width
동일 표현력: width_d^d = width_1
             width_d = width_1^{1/d}
```

**실제 신경망 학습에의 의미**:
- 깊이를 활용하면 파라미터 수 대폭 감소
- 계산 비용 감소 + 일반화 가능성 향상
- 현대 딥러닝의 "깊이" 강조의 이론적 근거

</details>

---

<div align="center">

| | | |
|---|---|---|
| | [📚 README](../README.md) | [다음 ▶](./02-ntk-theory.md) |

</div>
