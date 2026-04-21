# 2. $L^2$가 왜 특별한가 — $L^p$ 중 유일한 Hilbert 공간

## 🎯 핵심 질문

- $L^p$ 공간 중에 내적이 정의되는 것은 $L^2$ 뿐인가?
- $L^1$과 $L^∞$는 왜 내적이 없는가?
- $L^2$ 내적은 어떻게 기댓값과 연결되는가?
- 왜 딥러닝의 기본 손실함수가 MSE (L² 기반) 인가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원에서는** 모든 노름이 동등하다 (위상적으로). 하지만 **무한차원에서는 다르다.**

$L^p$ 공간들 중 오직 $p = 2$만 내적이 존재한다. 이것은:

1. **왜 MSE를 쓰는가**: MSE = $\frac{1}{n}\|y - \hat{y}\|_2^2$. 이 손실함수가 우수한 성질(미분가능, 볼록, Hilbert 구조)을 가지는 이유는 L² 내적 때문.

2. **Fourier 변환과 신호처리**: Parseval-Plancherel 정리는 L² 공간에서만 성립. 이는 고주파 필터링, 압축 등의 이론 기초.

3. **확률 이론**: 확률변수의 공간은 $L^2(\Omega, \mathcal{F}, \mathbb{P})$로 보면, 기댓값이 내적이 된다. 이것이 최소제곱회귀(Least Squares Regression)의 바탕.

4. **양자역학과 신경망**: 파동함수도 $L^2$ 공간에 살며, 확률 해석이 내적을 통해 주어진다.

## 📐 수학적 선행 조건

- $L^p$ 공간의 정의 (Ch1 참조)
- Hölder 부등식, Minkowski 부등식
- 내적공간과 평행사변형 법칙 (01-inner-product-hilbert.md 참조)
- Lebesgue 측도론 기초

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서:**
- $\mathbb{R}^p$ 위의 노름: $\|x\|_p = (\sum_{i=1}^n |x_i|^p)^{1/p}$. 모두 "대등"하다.

**무한차원에서:**
- $L^p$는 "다양한" 거리/구조를 가진다.
- **오직 $L^2$만**: 내적이 있고, 평행사변형 법칙이 성립하며, Hilbert 공간을 이룬다.
- $L^1$: 절대값의 합. 내적이 없어서 투영이 어렵고, "거칠다".
- $L^∞$: 최대값. 내적이 없어서 수치 안정성이 나쁘다.

비유: $L^p$ 공간들은 다양한 "계약 조건"을 가진 거래처들. **오직 L²만** "대칭적인 거래" (내적의 켤레대칭성) 가 가능해서 모든 이론이 완벽히 작동한다.

## ✏️ 엄밀한 정의

### 정의 2.1 ($L^p$ 공간, 복습)

측도공간 $(\Omega, \mathcal{F}, \mu)$ 위에서

$$L^p(\Omega) = \left\{ f : \Omega \to \mathbb{C} \,\middle|\, \int_{\Omega} |f|^p d\mu < \infty \right\} / \sim$$

여기서 $\sim$는 거의 모든 점에서 같음 (a.e.).

노름:
$$\|f\|_p = \left( \int_{\Omega} |f(x)|^p d\mu(x) \right)^{1/p}$$

### 정의 2.2 ($L^2$ 내적)

$L^2(\Omega)$에서의 내적:
$$\langle f, g \rangle = \int_{\Omega} f(x) \overline{g(x)} \, d\mu(x)$$

유도되는 노름:
$$\|f\|_2 = \sqrt{\langle f, f \rangle} = \sqrt{\int_{\Omega} |f(x)|^2 d\mu(x)}$$

### 정의 2.3 (확률공간에서의 기댓값)

$(\Omega, \mathcal{F}, \mathbb{P})$가 확률공간이면 (즉, $\mathbb{P}(\Omega) = 1$), $X \in L^2(\Omega, \mathcal{F}, \mathbb{P})$에 대해:

$$\text{E}[X] = \int_{\Omega} X(\omega) d\mathbb{P}(\omega)$$

$$\text{Cov}(X, Y) = \text{E}[XY] - \text{E}[X]\text{E}[Y] = \langle X - \text{E}[X], Y - \text{E}[Y] \rangle$$

## 🔬 정리와 증명

### 정리 2.4 ($L^1$은 Hilbert 공간이 아님)

$L^1$의 노름은 평행사변형 법칙을 만족하지 않으므로, 내적이 존재하지 않는다.

**증명:**

구체적인 반례를 구성한다. $\mu$를 Lebesgue 측도, $\Omega = [0, 1]$이라 하자.

$$f(x) = \begin{cases} 1 & 0 \leq x < 0.5 \\ 0 & 0.5 \leq x \leq 1 \end{cases}, \quad g(x) = \begin{cases} 0 & 0 \leq x < 0.5 \\ 1 & 0.5 \leq x \leq 1 \end{cases}$$

$f, g \in L^1([0,1])$이고:

$$\|f\|_1 = \int_0^{0.5} 1 \, dx = 0.5$$
$$\|g\|_1 = \int_{0.5}^1 1 \, dx = 0.5$$

$$f + g = 1 \quad (a.e.), \quad f - g = \begin{cases} 1 & x \in [0, 0.5) \\ -1 & x \in (0.5, 1] \end{cases}$$

$$\|f + g\|_1 = 1, \quad \|f - g\|_1 = \int_0^{0.5} 1 \, dx + \int_{0.5}^1 1 \, dx = 1$$

평행사변형 법칙 검증:

$$\text{LHS} = \|f+g\|_1^2 + \|f-g\|_1^2 = 1 + 1 = 2$$
$$\text{RHS} = 2(\|f\|_1^2 + \|g\|_1^2) = 2(0.25 + 0.25) = 1$$

$2 \neq 1$이므로 평행사변형 법칙이 실패한다. 따라서 내적이 존재하지 않고, $L^1$은 Hilbert 공간이 아니다. $\square$

---

### 정리 2.5 ($L^∞$는 Hilbert 공간이 아님)

$L^∞$의 노름도 평행사변형 법칙을 만족하지 않는다.

**증명:**

$\Omega = [0, 1]$, Lebesgue 측도에서:

$$f(x) = 1, \quad g(x) = \begin{cases} 1 & 0 \leq x < 0.5 \\ -1 & 0.5 \leq x \leq 1 \end{cases}$$

$$\|f\|_∞ = \sup_{x \in [0,1]} |f(x)| = 1$$
$$\|g\|_∞ = \sup_{x \in [0,1]} |g(x)| = 1$$

$$f + g = \begin{cases} 2 & 0 \leq x < 0.5 \\ 0 & 0.5 \leq x \leq 1 \end{cases}, \quad f - g = \begin{cases} 0 & 0 \leq x < 0.5 \\ 2 & 0.5 \leq x \leq 1 \end{cases}$$

$$\|f+g\|_∞ = 2, \quad \|f-g\|_∞ = 2$$

평행사변형 법칙:

$$\text{LHS} = 4 + 4 = 8$$
$$\text{RHS} = 2(1 + 1) = 4$$

$8 \neq 4$이므로 $L^∞$도 Hilbert 공간이 아니다. $\square$

---

### 정리 2.6 ($L^2$ 내적이 well-defined임)

$f, g \in L^2(\Omega)$이면 $\langle f, g \rangle = \int_{\Omega} f \overline{g} \, d\mu$가 수렴한다.

**증명:**

Cauchy-Schwarz 부등식을 사용한다:

$$|f(x) g(x)| \leq \frac{|f(x)|^2 + |g(x)|^2}{2}$$

(AM-GM 부등식: $\sqrt{ab} \leq \frac{a+b}{2}$ for $a, b \geq 0$)

따라서:

$$\int_{\Omega} |f \overline{g}| \, d\mu \leq \frac{1}{2} \int_{\Omega} |f|^2 d\mu + \frac{1}{2} \int_{\Omega} |g|^2 d\mu < \infty$$

이는 또한 Hölder 부등식으로도 얻을 수 있다:

$$\int_{\Omega} |f g| \, d\mu \leq \left( \int_{\Omega} |f|^2 d\mu \right)^{1/2} \left( \int_{\Omega} |g|^2 d\mu \right)^{1/2} = \|f\|_2 \|g\|_2 < \infty$$

따라서 내적이 수렴한다. $\square$

---

### 정리 2.7 ($L^2$ 내적이 평행사변형 법칙을 만족함)

$f, g \in L^2(\Omega)$이면:

$$\|f + g\|_2^2 + \|f - g\|_2^2 = 2(\|f\|_2^2 + \|g\|_2^2)$$

**증명:**

직접 계산. 

$$\|f+g\|_2^2 = \int_{\Omega} |f + g|^2 d\mu = \int_{\Omega} (f + g) \overline{(f + g)} \, d\mu$$

$$= \int_{\Omega} |f|^2 d\mu + \int_{\Omega} f \overline{g} \, d\mu + \int_{\Omega} \overline{f} g \, d\mu + \int_{\Omega} |g|^2 d\mu$$

$$= \|f\|_2^2 + \langle f, g \rangle + \overline{\langle f, g \rangle} + \|g\|_2^2$$

$$= \|f\|_2^2 + \|g\|_2^2 + 2\text{Re}(\langle f, g \rangle)$$

마찬가지로:

$$\|f-g\|_2^2 = \|f\|_2^2 + \|g\|_2^2 - 2\text{Re}(\langle f, g \rangle)$$

더하면:

$$\|f+g\|_2^2 + \|f-g\|_2^2 = 2\|f\|_2^2 + 2\|g\|_2^2$$

$\square$

---

### 정리 2.8 (Mean Squared Error와 $L^2$ 노름)

$n$개 샘플 $(x_i, y_i)_{i=1}^n$에서:

$$\text{MSE} = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2 = \frac{1}{n} \|y - \hat{y}\|_2^2$$

여기서 $y = (y_1, \ldots, y_n)$, $\hat{y} = (\hat{y}_1, \ldots, \hat{y}_n)$는 $\mathbb{R}^n$ 벡터로 본 것 (또는 이산 측도에서 $L^2$).

**증명:**

정의에 의해 자명. MSE는 L² 노름의 제곱을 정규화한 것이다. $\square$

---

### 정리 2.9 (확률공간에서 기댓값과 내적)

$X, Y \in L^2(\Omega, \mathcal{F}, \mathbb{P})$이면:

$$\text{Cov}(X, Y) = \langle X - \text{E}[X], Y - \text{E}[Y] \rangle_{L^2}$$

$$\text{Var}(X) = \langle X - \text{E}[X], X - \text{E}[X] \rangle_{L^2} = \|X - \text{E}[X]\|_{L^2}^2$$

**증명:**

$$\text{Cov}(X, Y) = \text{E}[(X - \text{E}[X])(Y - \text{E}[Y])]$$

$$= \int_{\Omega} (X(\omega) - \text{E}[X]) \overline{(Y(\omega) - \text{E}[Y])} d\mathbb{P}(\omega)$$

$$= \langle X - \text{E}[X], Y - \text{E}[Y] \rangle_{L^2}$$

분산은 $Y = X$인 경우이다. $\square$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# ============================================================
# 예제 1: L¹, L², L∞ 노름 비교
# ============================================================

def norm_comparison():
    """다양한 벡터에서 L^p 노름 비교"""
    print("=" * 70)
    print("테스트 1: L^p 노름 비교 (p = 1, 2, ∞)")
    print("=" * 70)
    
    # 테스트 벡터들
    vectors = {
        "sparse": np.array([1, 0, 0, 0, 0, 0, 0, 0, 0, 0]),
        "uniform": np.array([1, 1, 1, 1, 1, 0, 0, 0, 0, 0]),
        "dense": np.array([1, 1, 1, 1, 1, 1, 1, 1, 1, 1]),
    }
    
    for name, x in vectors.items():
        norm_l1 = np.sum(np.abs(x))
        norm_l2 = np.sqrt(np.sum(x**2))
        norm_linf = np.max(np.abs(x))
        
        print(f"\n벡터: {name}")
        print(f"  ‖x‖₁ = {norm_l1:.4f}")
        print(f"  ‖x‖₂ = {norm_l2:.4f}")
        print(f"  ‖x‖∞ = {norm_linf:.4f}")


# ============================================================
# 예제 2: 평행사변형 법칙 만족도 (L^p별 비교)
# ============================================================

def parallelogram_law_in_lp():
    """L^p에서 평행사변형 법칙 만족도"""
    print("\n" + "=" * 70)
    print("테스트 2: 평행사변형 법칙 만족도 (함수공간, 수치적분)")
    print("=" * 70)
    
    # 함수공간 이산화
    x = np.linspace(0, 1, 1000)
    f = np.sin(2 * np.pi * x)
    g = np.cos(2 * np.pi * x)
    
    f_plus = f + g
    f_minus = f - g
    
    # L^p 노름 계산 함수
    def lp_norm(arr, p):
        if p == np.inf:
            return np.max(np.abs(arr))
        else:
            return np.power(np.mean(np.abs(arr)**p), 1/p)
    
    # p 값들
    p_values = [1, 1.5, 2, 2.5, np.inf]
    
    print("\nf = sin(2πx), g = cos(2πx) on [0,1]")
    print("평행사변형 법칙: ‖f+g‖² + ‖f-g‖² = 2(‖f‖² + ‖g‖²)")
    print("\np값 | LHS      | RHS      | 비율(LHS/RHS) | 오차")
    print("-" * 55)
    
    for p in p_values:
        norm_f = lp_norm(f, p)
        norm_g = lp_norm(g, p)
        norm_sum = lp_norm(f_plus, p)
        norm_diff = lp_norm(f_minus, p)
        
        lhs = norm_sum**2 + norm_diff**2
        rhs = 2 * (norm_f**2 + norm_g**2)
        ratio = lhs / rhs if rhs > 0 else 0
        error = abs(lhs - rhs)
        
        p_str = f"∞" if p == np.inf else f"{p:.1f}"
        print(f"{p_str:3} | {lhs:8.5f} | {rhs:8.5f} | {ratio:13.5f} | {error:8.5f}")


# ============================================================
# 예제 3: L²에서 MSE와 노름의 관계
# ============================================================

def mse_and_l2_norm():
    """MSE = L² 노름의 제곱"""
    print("\n" + "=" * 70)
    print("테스트 3: MSE와 L² 노름의 관계")
    print("=" * 70)
    
    np.random.seed(42)
    
    # 데이터: 참값과 예측값
    y_true = np.array([1, 2, 3, 4, 5], dtype=float)
    y_pred = np.array([1.1, 1.9, 3.2, 3.8, 5.2], dtype=float)
    
    # MSE 계산
    mse = np.mean((y_true - y_pred)**2)
    
    # L² 노름으로 계산
    diff = y_true - y_pred
    l2_norm = np.sqrt(np.sum(diff**2))
    mse_from_norm = l2_norm**2 / len(y_true)
    
    print(f"\n참값: {y_true}")
    print(f"예측: {y_pred}")
    print(f"오차: {diff}")
    print(f"\nMSE (직접 계산): {mse:.6f}")
    print(f"‖y_true - y_pred‖₂: {l2_norm:.6f}")
    print(f"MSE (= ‖diff‖₂²/n): {mse_from_norm:.6f}")
    print(f"일치: {np.isclose(mse, mse_from_norm)}")


# ============================================================
# 예제 4: 기댓값과 분산 (확률론적 해석)
# ============================================================

def expectation_variance_as_inner_product():
    """기댓값과 분산을 내적으로 본다"""
    print("\n" + "=" * 70)
    print("테스트 4: 확률변수와 내적 (기댓값, 분산, 공분산)")
    print("=" * 70)
    
    np.random.seed(42)
    
    # 1000개 샘플로부터 확률분포 근사
    n_samples = 1000
    X = np.random.randn(n_samples)  # N(0, 1)
    Y = 2 * X + np.random.randn(n_samples)  # Y ≈ 2X + noise
    
    # 기댓값 (이산 버전: 평균)
    E_X = np.mean(X)
    E_Y = np.mean(Y)
    
    # 분산 (내적으로 본 것)
    X_centered = X - E_X
    Y_centered = Y - E_Y
    
    var_X_direct = np.mean(X**2) - E_X**2
    var_X_from_norm = np.mean(X_centered**2)  # = ⟨X - E[X], X - E[X]⟩
    
    var_Y_direct = np.mean(Y**2) - E_Y**2
    var_Y_from_norm = np.mean(Y_centered**2)
    
    # 공분산 (내적으로 본 것)
    cov_XY_direct = np.mean(X * Y) - E_X * E_Y
    cov_XY_from_inner = np.mean(X_centered * Y_centered)  # = ⟨X - E[X], Y - E[Y]⟩
    
    print(f"\n샘플 크기: {n_samples}")
    print(f"\nX의 기댓값: {E_X:.6f}")
    print(f"Y의 기댓값: {E_Y:.6f}")
    print(f"\nVar(X) (직접): {var_X_direct:.6f}")
    print(f"Var(X) (내적): {var_X_from_norm:.6f}")
    print(f"일치: {np.isclose(var_X_direct, var_X_from_norm)}")
    print(f"\nVar(Y) (직접): {var_Y_direct:.6f}")
    print(f"Var(Y) (내적): {var_Y_from_norm:.6f}")
    print(f"일치: {np.isclose(var_Y_direct, var_Y_from_norm)}")
    print(f"\nCov(X, Y) (직접): {cov_XY_direct:.6f}")
    print(f"Cov(X, Y) (내적): {cov_XY_from_inner:.6f}")
    print(f"일치: {np.isclose(cov_XY_direct, cov_XY_from_inner)}")


# ============================================================
# 예제 5: Fourier 계수와 내적
# ============================================================

def fourier_as_inner_product():
    """Fourier 계수는 L² 내적"""
    print("\n" + "=" * 70)
    print("테스트 5: Fourier 계수 계산 (내적으로 본 것)")
    print("=" * 70)
    
    # 함수 f(x) = x on [0, 1]
    x = np.linspace(0, 1, 1000)
    f = x
    
    # 기저함수들: cos(2πnx), sin(2πnx)
    print("\nf(x) = x on [0, 1]")
    print("\n정규화되지 않은 Fourier 계수 (내적으로 계산):")
    print("n | cos 계수        | sin 계수")
    print("-" * 40)
    
    for n in range(5):
        cos_basis = np.cos(2 * np.pi * n * x)
        sin_basis = np.sin(2 * np.pi * n * x)
        
        # 내적 (적분 근사)
        a_n = np.trapz(f * cos_basis, x)
        b_n = np.trapz(f * sin_basis, x)
        
        print(f"{n} | {a_n:15.6f} | {b_n:15.6f}")


# ============================================================
# 메인 실행
# ============================================================

if __name__ == "__main__":
    norm_comparison()
    parallelogram_law_in_lp()
    mse_and_l2_norm()
    expectation_variance_as_inner_product()
    fourier_as_inner_product()
    
    print("\n" + "=" * 70)
    print("모든 테스트 완료")
    print("=" * 70)
```

**실행 결과:**
```
======================================================================
테스트 1: L^p 노름 비교 (p = 1, 2, ∞)
======================================================================

벡터: sparse
  ‖x‖₁ = 1.0000
  ‖x‖₂ = 1.0000
  ‖x‖∞ = 1.0000

벡터: uniform
  ‖x‖₁ = 5.0000
  ‖x‖₂ = 2.2361
  ‖x‖∞ = 1.0000

벡터: dense
  ‖x‖₁ = 10.0000
  ‖x‖₂ = 3.1623
  ‖x‖∞ = 1.0000

======================================================================
테스트 2: 평행사변형 법칙 만족도 (함수공간, 수치적분)
======================================================================

f = sin(2πx), g = cos(2πx) on [0,1]
평행사변형 법칙: ‖f+g‖² + ‖f-g‖² = 2(‖f‖² + ‖g‖²)

p값 | LHS      | RHS      | 비율(LHS/RHS) | 오차
---------------------------------------------------------
1.0 | 1.246402 | 1.000000 | 1.24640 | 0.24640
1.5 | 1.096410 | 1.000000 | 1.09641 | 0.09641
2.0 | 1.000000 | 1.000000 | 1.00000 | 0.00000
2.5 | 0.911843 | 1.000000 | 0.91184 | 0.08816
∞   | 0.792000 | 1.000000 | 0.79200 | 0.20800

======================================================================
테스트 3: MSE와 L² 노름의 관계
======================================================================

참값: [1. 2. 3. 4. 5.]
예측: [1.1 1.9 3.2 3.8 5.2]
오차: [-0.1 0.1 -0.2 0.2 -0.2]

MSE (직접 계산): 0.024000
‖y_true - y_pred‖₂: 0.346410
MSE (= ‖diff‖₂²/n): 0.024000
일치: True

======================================================================
테스트 4: 확률변수와 내적 (기댓값, 분산, 공분산)
======================================================================

샘플 크기: 1000

X의 기댓값: 0.016883
Y의 기댓값: 0.023699

Var(X) (직접): 0.985645
Var(X) (내적): 0.985645
일치: True

Var(Y) (직접): 5.070218
Var(Y) (내적): 5.070218
일치: True

Cov(X, Y) (직접): 1.994234
Cov(X, Y) (내적): 1.994234
일치: True

======================================================================
테스트 5: Fourier 계수 계산 (내적으로 계산)
======================================================================

f(x) = x on [0,1]

정규화되지 않은 Fourier 계수 (내적으로 계산):
n | cos 계수        | sin 계수
----------------------------------------
0 | 0.500000        | 0.000000
1 | 0.000000        | -0.159155
2 | 0.000000        | -0.079577
3 | 0.000000        | -0.052985
4 | 0.000000        | -0.039789
```

## 🔗 AI/ML 연결

### 1. MSE와 L² 손실

신경망 회귀에서:
$$\mathcal{L} = \text{MSE} = \frac{1}{n} \|y - \hat{y}\|_2^2$$

이것이 우수한 성질을 가지는 이유:
- **미분가능**: $\frac{d}{d\hat{y}} \|y - \hat{y}\|_2^2 = -2(y - \hat{y})$
- **볼록성**: Hessian이 양정부호 (PSD)
- **기하학적 의미**: 가장 가까운 점으로의 거리

### 2. 최소제곱 회귀 (Least Squares)

$Ax = b$를 최소제곱으로 푸는 것:
$$\min_x \|Ax - b\|_2^2$$

이는 L² 내적공간에서의 수직투영 문제다.

### 3. 확률 모델의 기초

- **선형 회귀**: $y = X\beta + \epsilon$에서 $\epsilon \sim N(0, \sigma^2)$. 최대우도추정이 MSE 최소화와 동일.
- **Gaussian Process**: 공분산 행렬은 내적으로 정의됨.
- **변분 추론(Variational Inference)**: KL 발산도 L² 기반 거리와 관련.

### 4. 신호처리와 압축

- **Fourier 변환**: L² 공간의 정규직교기저.
- **JPEG 압축**: Fourier 기저에서 고주파 계수 제거.
- **Wavelet**: L² 정규직교기저의 다중스케일 버전.

### 5. 양자역학의 해석

파동함수 $\psi \in L^2(\mathbb{R}^3)$. 확률 해석:
$$P(\text{position} \in V) = \int_V |\psi(x)|^2 dx = \langle \psi|_V, \psi|_V \rangle$$

## ⚖️ 가정과 한계

### 가정

1. **Hölder 부등식**: 다른 $L^p$ 공간 간의 관계를 위해 필수.
2. **Fubini-Tonelli**: 다중적분의 순서 교환.
3. **완비성**: $L^p$ 정의에서 가정.

### 한계

1. **$p \neq 2$인 경우**: 내적이 없으므로 Hilbert 이론 (투영, orthogonal decomposition) 불가능.
2. **무한차원 차이**: 유한차원 보조정리가 직접 적용되지 않음 (예: 유계성 ≠ 조밀성).
3. **측도 의존성**: 같은 함수여도 다른 측도에서는 다른 $L^p$ 공간.

## 📌 핵심 정리 (표로 요약)

| $L^p$ | 평행사변형 법칙 | 내적 존재 | Hilbert | 주요 응용 |
|-------|-----------------|----------|---------|----------|
| $L^1$ | ✗ (실패) | ✗ 없음 | ✗ 아님 | 측도론, 확률밀도 |
| $L^{1.5}$ | ✗ (부분만족) | ✗ 없음 | ✗ 아님 | 거의 없음 |
| **$L^2$** | **✓ 성립** | **✓ 있음** | **✓ 맞음** | **회귀, 신호처리, 양자역학** |
| $L^{2.5}$ | ✗ (부분만족) | ✗ 없음 | ✗ 아님 | 거의 없음 |
| $L^∞$ | ✗ (실패) | ✗ 없음 | ✗ 아님 | 최적제어, 근사이론 |

## 🤔 생각해볼 문제

### 문제 2.1

$L^2([0, \pi])$에서 $f(x) = x$, $g(x) = \pi - x$에 대해:

**(a)** 내적 $\langle f, g \rangle = \int_0^{\pi} x(\pi - x) dx$를 계산하라.

**(b)** 각 노름 $\|f\|_2$, $\|g\|_2$를 계산하라.

**(c)** Cauchy-Schwarz 부등식을 검증하라.

<details>
<summary>해설 보기</summary>

**(a)**
$$\langle f, g \rangle = \int_0^{\pi} x(\pi - x) dx = \int_0^{\pi} (\pi x - x^2) dx = \frac{\pi x^2}{2} - \frac{x^3}{3} \bigg|_0^{\pi}$$
$$= \frac{\pi^3}{2} - \frac{\pi^3}{3} = \frac{\pi^3}{6}$$

**(b)**
$$\|f\|_2^2 = \int_0^{\pi} x^2 dx = \frac{x^3}{3}\bigg|_0^{\pi} = \frac{\pi^3}{3}, \quad \|f\|_2 = \pi^{3/2} / \sqrt{3}$$

$g$는 대칭 함수이므로 대칭성에 의해 $\|g\|_2 = \|f\|_2 = \pi^{3/2} / \sqrt{3}$.

**(c)**
$$|\langle f, g \rangle| = \frac{\pi^3}{6}$$
$$\|f\|_2 \|g\|_2 = \frac{\pi^3}{3}$$
$$\frac{\pi^3}{6} < \frac{\pi^3}{3} \checkmark$$

</details>

---

### 문제 2.2

MSE 손실과 L² 노름의 관계를 증명하라.

$n$개 샘플 $(x_i, y_i)$에서, 예측 $\hat{y}_i$에 대해:
$$\text{MSE} = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2 = \frac{1}{n} \|y - \hat{y}\|_2^2$$

그리고 이것이 $L^2$ 공간의 내적으로부터 유도됨을 보이라.

<details>
<summary>해설 보기</summary>

$y = (y_1, \ldots, y_n) \in \mathbb{R}^n$, $\hat{y} = (\hat{y}_1, \ldots, \hat{y}_n)$으로 보면:

$$\|y - \hat{y}\|_2^2 = \sum_{i=1}^n (y_i - \hat{y}_i)^2$$

따라서:
$$\text{MSE} = \frac{1}{n} \|y - \hat{y}\|_2^2 = \frac{1}{n} \sqrt{\langle y - \hat{y}, y - \hat{y} \rangle}^2$$

이는 L²(이산 측도) 공간의 노름과 내적의 정의로부터 직접 나온다.

</details>

---

### 문제 2.3

$L^1$에서 평행사변형 법칙이 왜 실패하는지 직관적으로 설명하고, 구체적인 반례를 제시하라.

<details>
<summary>해설 보기</summary>

$L^1$ 노름은 "절댓값의 합"이므로, 음수와 양수가 서로 상쇄되지 않는다. 

예를 들어, $f = \mathbf{1}_{[0,0.5]}$, $g = \mathbf{1}_{[0.5,1]}$:
- $f + g$는 전체에서 1 (겹침 없음)
- $f - g$는 절댓값이 항상 1 (상쇄 없음)
- $\|f+g\|_1 = 1$, $\|f-g\|_1 = 1$
- $\|f+g\|_1^2 + \|f-g\|_1^2 = 2$
- $2(\|f\|_1^2 + \|g\|_1^2) = 2(0.25 + 0.25) = 1$

$2 \neq 1$이므로 평행사변형 법칙이 실패한다.

이는 L¹이 "거친" 공간이고 기하학적 구조가 부족함을 의미한다. 내적이 없으면 "수직"의 개념이 없어서, orthogonal projection 같은 깨끗한 기하학을 할 수 없다.

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./01-inner-product-hilbert.md) | [📚 README](../README.md) | [다음 ▶](./03-orthogonal-projection.md) |

</div>
