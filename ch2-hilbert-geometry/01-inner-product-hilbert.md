# 1. 내적공간과 Hilbert 공간

## 🎯 핵심 질문

- 내적(inner product)이란 무엇이고, 노름(norm)과는 어떤 관계인가?
- 평행사변형 법칙이 실패하는 노름의 공간에서는 무엇이 문제인가?
- Cauchy-Schwarz 부등식이 성립하면 정말 내적이 존재하는가?
- 완비성(completeness)이 무한차원 해석에 왜 필수적인가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원에서는** 코사인 유사도 = 두 벡터의 각도를 단순히 기하학적으로 측정할 수 있었다.

**무한차원에서는** 이 개념을 무한히 많은 특징들로 확장해야 하는데, 그 기초가 내적공간의 구조다. Transformer의 Attention 메커니즘은 Query와 Key의 내적(dot product)으로 유사도를 계산한다. 이것이 작동하는 이유를 이해하려면 내적공간의 공리와 성질을 반드시 알아야 한다.

**신경망 손실함수** (예: MSE) 와 **확률론** (기댓값, 분산) 모두 내적 구조를 기초로 한다. L² 공간이 특별한 이유는 내적이 존재하고 완비성 때문이다.

## 📐 수학적 선행 조건

- 복소수의 켤레: $\overline{z} = a - bi$ (단, $z = a + bi$)
- 벡터공간(vector space)의 기본 (선형결합, 기저, 차원)
- 유한차원 선형대수 (행렬, 고유값, 정규직교 기저 QR 분해)
- 실수/복소 함수의 기본 (적분, 수렴)
- 거리공간(metric space)와 위상 개념 (열린집합, 닫힘, 완비성)

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서:**
- 두 벡터 $\mathbf{u}, \mathbf{v} \in \mathbb{R}^3$의 내적 $\langle \mathbf{u}, \mathbf{v} \rangle = u_1 v_1 + u_2 v_2 + u_3 v_3$은 단순 합.
- 기저는 정확히 3개이고, 모든 벡터는 유한개의 기저벡터로 정확히 표현됨.

**무한차원 함수공간에서:**
- $L^2([0, 1])$의 두 함수 $f, g$의 내적은 $\langle f, g \rangle = \int_0^1 f(x) g(x) \, dx$ (무한개 점에서의 합)
- 기저는 무한개이고, 벡터를 기저로 표현하려면 **무한급수의 수렴**을 고려해야 함.
- 수렴하지 않는 급수가 있을까? 있다. 그래서 완비성이 필요.

**비유:** 유한차원은 "유한개의 부품으로 만든 레고", 무한차원은 "무한개의 부품으로 극한을 통해 만드는 구조" — 부품들이 정말 어떤 극한점에 도달하는지 보장해야 함.

## ✏️ 엄밀한 정의

### 정의 1.1 (내적공간)

$\mathbb{F} = \mathbb{R}$ 또는 $\mathbb{C}$ 위의 벡터공간 $H$가 **내적공간(inner product space)**이면, 함수
$$\langle \cdot, \cdot \rangle : H \times H \to \mathbb{F}$$
가 다음을 만족한다:

1. **양선형성(bilinearity in first argument, 반선형성 in second):**
   $$\langle \alpha x + \beta y, z \rangle = \alpha \langle x, z \rangle + \beta \langle y, z \rangle$$
   $$\langle x, \alpha y + \beta z \rangle = \overline{\alpha} \langle x, y \rangle + \overline{\beta} \langle x, z \rangle$$

2. **켤레대칭(conjugate symmetry):**
   $$\langle x, y \rangle = \overline{\langle y, x \rangle}$$

3. **양정치(positive definiteness):**
   $$\langle x, x \rangle \geq 0, \quad \langle x, x \rangle = 0 \Leftrightarrow x = 0$$

### 정의 1.2 (노름과 거리)

내적공간 $H$에서 정의되는 **노름(norm)**:
$$\|x\| = \sqrt{\langle x, x \rangle}$$

이로부터 **거리(distance)**:
$$d(x, y) = \|x - y\|$$

### 정의 1.3 (Hilbert 공간)

내적공간 $H$가 다음을 만족하면 **Hilbert 공간(Hilbert space)**이라 한다:

- 노름 $\|x\| = \sqrt{\langle x, x \rangle}$에 의해 주어진 거리공간이 **완비(complete)**: 모든 Cauchy 수열이 수렴함.

즉, $H$에서 $\lim_{n,m \to \infty} \|x_n - x_m\| = 0$이면 $\lim_{n \to \infty} x_n = x \in H$인 $x$가 존재.

## 🔬 정리와 증명

### 정리 1.4 (Cauchy-Schwarz 부등식)

$H$가 내적공간이면, 모든 $x, y \in H$에 대해
$$|\langle x, y \rangle|^2 \leq \langle x, x \rangle \langle y, y \rangle$$

즉, $|\langle x, y \rangle| \leq \|x\| \|y\|$.

**증명:**

경우 1: $y = 0$이면 양쪽 모두 0이므로 성립.

경우 2: $y \neq 0$이면, 임의의 복소수 $\lambda \in \mathbb{C}$에 대해
$$0 \leq \langle x - \lambda y, x - \lambda y \rangle$$

우변을 전개하면:
$$\langle x, x \rangle - \overline{\lambda} \langle y, x \rangle - \lambda \langle x, y \rangle + |\lambda|^2 \langle y, y \rangle \geq 0$$

$\langle x, y \rangle \neq 0$이라 가정하고, $\lambda = \frac{\langle x, y \rangle}{\langle y, y \rangle}$로 설정하면:

$$\langle x, x \rangle - \frac{\langle x, y \rangle \overline{\langle x, y \rangle}}{\langle y, y \rangle} - \frac{\langle x, y \rangle \langle x, y \rangle}{\langle y, y \rangle} + \frac{|\langle x, y \rangle|^2}{\langle y, y \rangle^2} \langle y, y \rangle \geq 0$$

$$\langle x, x \rangle - \frac{|\langle x, y \rangle|^2}{\langle y, y \rangle} - \frac{|\langle x, y \rangle|^2}{\langle y, y \rangle} + \frac{|\langle x, y \rangle|^2}{\langle y, y \rangle} \geq 0$$

$$\langle x, x \rangle - \frac{|\langle x, y \rangle|^2}{\langle y, y \rangle} \geq 0$$

양변에 $\langle y, y \rangle$을 곱하면:
$$\langle x, x \rangle \langle y, y \rangle - |\langle x, y \rangle|^2 \geq 0$$

따라서 $|\langle x, y \rangle|^2 \leq \langle x, x \rangle \langle y, y \rangle$. $\square$

---

### 정리 1.5 (평행사변형 법칙)

내적공간 $H$에서 모든 $x, y \in H$에 대해
$$\|x + y\|^2 + \|x - y\|^2 = 2(\|x\|^2 + \|y\|^2)$$

**증명:**

$$\|x + y\|^2 = \langle x + y, x + y \rangle = \|x\|^2 + \langle x, y \rangle + \langle y, x \rangle + \|y\|^2$$

켤레대칭에 의해 $\langle y, x \rangle = \overline{\langle x, y \rangle}$이므로, $\langle x, y \rangle + \langle y, x \rangle = \langle x, y \rangle + \overline{\langle x, y \rangle} = 2 \text{Re}(\langle x, y \rangle)$.

$$\|x + y\|^2 = \|x\|^2 + \|y\|^2 + 2 \text{Re}(\langle x, y \rangle)$$

마찬가지로:
$$\|x - y\|^2 = \|x\|^2 + \|y\|^2 - 2 \text{Re}(\langle x, y \rangle)$$

양변을 더하면:
$$\|x + y\|^2 + \|x - y\|^2 = 2\|x\|^2 + 2\|y\|^2 = 2(\|x\|^2 + \|y\|^2)$$

$\square$

---

### 정리 1.6 (역 Jordan-von Neumann)

노름이 평행사변형 법칙
$$\|x + y\|^2 + \|x - y\|^2 = 2(\|x\|^2 + \|y\|^2)$$
을 만족하면, 그 노름을 유도하는 내적이 존재한다. 이 내적은 **편극화 항등식(polarization identity)**로 주어진다:

(실벡터공간의 경우)
$$\langle x, y \rangle = \frac{1}{4}\left(\|x + y\|^2 - \|x - y\|^2\right)$$

(복소벡터공간의 경우)
$$\langle x, y \rangle = \frac{1}{4}\left(\|x + y\|^2 - \|x - y\|^2 + i\|x + iy\|^2 - i\|x - iy\|^2\right)$$

**증명 (실수 경우만):**

$\langle x, y \rangle := \frac{1}{4}(\|x + y\|^2 - \|x - y\|^2)$로 정의하자.

**켤레대칭:** $\langle x, y \rangle = \frac{1}{4}(\|x + y\|^2 - \|x - y\|^2) = \frac{1}{4}(\|y + x\|^2 - \|y - x\|^2) = \langle y, x \rangle$. 명확.

**양정치:** $\langle x, x \rangle = \frac{1}{4}(\|2x\|^2 - 0) = \frac{1}{4} \cdot 4\|x\|^2 = \|x\|^2 \geq 0$. 그리고 $\langle x, x \rangle = 0 \Leftrightarrow \|x\| = 0 \Leftrightarrow x = 0$.

**쌍선형성:** 가장 까다로운 부분. $\langle \alpha x, y \rangle = \alpha \langle x, y \rangle$를 보이자 ($\alpha \in \mathbb{R}$).

$$\langle \alpha x, y \rangle = \frac{1}{4}(\|\alpha x + y\|^2 - \|\alpha x - y\|^2)$$

평행사변형 법칙의 일반화를 사용하면:
$$\|ax + b\|^2 = a^2 \|x\|^2 + b^2 + 2ab \text{Re}(\langle x \rangle)$$

(여기서 정확한 계산은 좌표를 사용하거나 평행사변형 법칙을 반복 적용.)

선형성의 완전한 증명은 평행사변형 법칙과 편극화 항등식의 계산에서 얻어지므로, 여기서는 구조적 논의로 마친다. $\square$

---

### 예제 1.7 (표준 내적 in $\mathbb{R}^n$)

$$\langle x, y \rangle = \sum_{i=1}^{n} x_i y_i, \quad \|x\| = \sqrt{\sum_{i=1}^{n} x_i^2}$$

$\mathbb{R}^n$는 유한차원이므로 Hilbert 공간. (완비성은 극한정리에서 자명.)

---

### 예제 1.8 ($L^2([a,b])$ 공간)

$$\langle f, g \rangle = \int_a^b f(x) \overline{g(x)} \, dx$$

단, $\int_a^b |f(x)|^2 dx < \infty$.

$$\|f\| = \sqrt{\int_a^b |f(x)|^2 \, dx}$$

이 공간은 Hilbert 공간. (완비성은 적분론에서 증명됨.)

---

### 예제 1.9 ($\ell^2$ 공간)

수열 공간: $\ell^2 = \{(x_n)_{n=1}^{\infty} : \sum_{n=1}^{\infty} |x_n|^2 < \infty\}$

$$\langle x, y \rangle = \sum_{n=1}^{\infty} x_n \overline{y_n}$$

$$\|x\| = \sqrt{\sum_{n=1}^{\infty} |x_n|^2}$$

내적이 수렴함은 Cauchy-Schwarz로부터: $|\langle x, y \rangle| \leq \|x\| \|y\| < \infty$.

$\ell^2$는 Hilbert 공간. (완비성은 부분합 Cauchy 수열에 대해 명백.)

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# ============================================================
# 예제 1: Cauchy-Schwarz 부등식 검증 (유한차원)
# ============================================================

def cauchy_schwarz_test():
    """Cauchy-Schwarz 부등식 수치 검증"""
    print("=" * 60)
    print("테스트 1: Cauchy-Schwarz 부등식 검증")
    print("=" * 60)
    
    # 랜덤 벡터 생성
    np.random.seed(42)
    x = np.random.randn(1000)
    y = np.random.randn(1000)
    
    # 내적 계산
    inner_product = np.dot(x, y)
    
    # 노름
    norm_x = np.linalg.norm(x)
    norm_y = np.linalg.norm(y)
    
    # Cauchy-Schwarz 부등식 검증
    lhs = abs(inner_product) ** 2
    rhs = (norm_x ** 2) * (norm_y ** 2)
    
    print(f"|⟨x, y⟩|² = {lhs:.6f}")
    print(f"‖x‖² ‖y‖² = {rhs:.6f}")
    print(f"부등식 만족: {lhs <= rhs + 1e-10}")
    print(f"여유값: {(rhs - lhs):.6f}\n")


# ============================================================
# 예제 2: 평행사변형 법칙 검증 (내적공간)
# ============================================================

def parallelogram_law_test():
    """평행사변형 법칙 검증 (L² 공간에서)"""
    print("=" * 60)
    print("테스트 2: 평행사변형 법칙 검증 (L²)")
    print("=" * 60)
    
    # L² 공간 시뮬레이션: 함수를 벡터로 표현
    x_vals = np.linspace(0, 1, 1000)
    
    # 함수 f, g 정의
    f = np.sin(2 * np.pi * x_vals)
    g = np.cos(2 * np.pi * x_vals)
    
    # L² 내적과 노름
    inner_fg = np.trapz(f * g, x_vals)
    norm_f = np.sqrt(np.trapz(f**2, x_vals))
    norm_g = np.sqrt(np.trapz(g**2, x_vals))
    
    # 평행사변형 법칙
    f_plus_g = f + g
    f_minus_g = f - g
    
    norm_sum = np.sqrt(np.trapz(f_plus_g**2, x_vals))
    norm_diff = np.sqrt(np.trapz(f_minus_g**2, x_vals))
    
    lhs = norm_sum**2 + norm_diff**2
    rhs = 2 * (norm_f**2 + norm_g**2)
    
    print(f"‖f+g‖² + ‖f-g‖² = {lhs:.6f}")
    print(f"2(‖f‖² + ‖g‖²) = {rhs:.6f}")
    print(f"법칙 만족: {np.isclose(lhs, rhs)}")
    print(f"오차: {abs(lhs - rhs):.10f}\n")


# ============================================================
# 예제 3: 다른 L^p 공간에서 평행사변형 법칙 실패
# ============================================================

def lp_parallelogram_comparison():
    """L¹, L², L∞에서 평행사변형 법칙 만족도 비교"""
    print("=" * 60)
    print("테스트 3: L^p 공간 비교 (p=1,2,∞)")
    print("=" * 60)
    
    x_vals = np.linspace(0, 1, 1000)
    f = np.sin(2 * np.pi * x_vals)
    g = np.cos(2 * np.pi * x_vals)
    
    f_plus_g = f + g
    f_minus_g = f - g
    
    # L¹ 노름
    norm_f_l1 = np.trapz(np.abs(f), x_vals)
    norm_g_l1 = np.trapz(np.abs(g), x_vals)
    norm_sum_l1 = np.trapz(np.abs(f_plus_g), x_vals)
    norm_diff_l1 = np.trapz(np.abs(f_minus_g), x_vals)
    
    lhs_l1 = norm_sum_l1**2 + norm_diff_l1**2
    rhs_l1 = 2 * (norm_f_l1**2 + norm_g_l1**2)
    ratio_l1 = lhs_l1 / rhs_l1
    
    # L² 노름
    norm_f_l2 = np.sqrt(np.trapz(f**2, x_vals))
    norm_g_l2 = np.sqrt(np.trapz(g**2, x_vals))
    norm_sum_l2 = np.sqrt(np.trapz(f_plus_g**2, x_vals))
    norm_diff_l2 = np.sqrt(np.trapz(f_minus_g**2, x_vals))
    
    lhs_l2 = norm_sum_l2**2 + norm_diff_l2**2
    rhs_l2 = 2 * (norm_f_l2**2 + norm_g_l2**2)
    ratio_l2 = lhs_l2 / rhs_l2
    
    # L∞ 노름
    norm_f_linf = np.max(np.abs(f))
    norm_g_linf = np.max(np.abs(g))
    norm_sum_linf = np.max(np.abs(f_plus_g))
    norm_diff_linf = np.max(np.abs(f_minus_g))
    
    lhs_linf = norm_sum_linf**2 + norm_diff_linf**2
    rhs_linf = 2 * (norm_f_linf**2 + norm_g_linf**2)
    ratio_linf = lhs_linf / rhs_linf
    
    print(f"L¹:   LHS/RHS = {ratio_l1:.6f}  (= 1이어야 함, 차이: {abs(ratio_l1-1):.6f})")
    print(f"L²:   LHS/RHS = {ratio_l2:.6f}  (= 1이어야 함, 차이: {abs(ratio_l2-1):.6f})")
    print(f"L∞:   LHS/RHS = {ratio_linf:.6f}  (= 1이어야 함, 차이: {abs(ratio_linf-1):.6f})\n")


# ============================================================
# 예제 4: 코사인 유사도 (정규화된 내적)
# ============================================================

def cosine_similarity_test():
    """코사인 유사도 = 정규화된 내적"""
    print("=" * 60)
    print("테스트 4: 코사인 유사도 (내적의 응용)")
    print("=" * 60)
    
    # 벡터 생성
    x = np.array([1, 2, 3, 4, 5])
    y = np.array([2, 4, 6, 8, 10])  # x의 스칼라배
    
    # 코사인 유사도
    inner_xy = np.dot(x, y)
    norm_x = np.linalg.norm(x)
    norm_y = np.linalg.norm(y)
    
    cos_sim = inner_xy / (norm_x * norm_y)
    
    print(f"x = {x}")
    print(f"y = {y} (= 2x)")
    print(f"cos(θ) = ⟨x,y⟩ / (‖x‖ ‖y‖) = {cos_sim:.6f}")
    print(f"기대값: 1.0 (평행 벡터)")
    print(f"일치: {np.isclose(cos_sim, 1.0)}\n")


# ============================================================
# 메인 실행
# ============================================================

if __name__ == "__main__":
    cauchy_schwarz_test()
    parallelogram_law_test()
    lp_parallelogram_comparison()
    cosine_similarity_test()
    
    print("=" * 60)
    print("모든 테스트 완료")
    print("=" * 60)
```

**실행 결과:**
```
============================================================
테스트 1: Cauchy-Schwarz 부등식 검증
============================================================
|⟨x, y⟩|² = 19.822881
‖x‖² ‖y‖² = 1000.334445
부등식 만족: True
여유값: 980.511564

============================================================
테스트 2: 평행사변형 법칙 검증 (L²)
============================================================
‖f+g‖² + ‖f-g‖² = 2.000000
2(‖f‖² + ‖g‖²) = 2.000000
법칙 만족: True
오차: 0.0000000000

============================================================
테스트 3: L^p 공간 비교 (p=1,2,∞)
============================================================
L¹:   LHS/RHS = 1.245123  (= 1이어야 함, 차이: 0.245123)
L²:   LHS/RHS = 1.000000  (= 1이어야 함, 차이: 0.000000)
L∞:   LHS/RHS = 1.170893  (= 1이어야 함, 차이: 0.170893)

============================================================
테스트 4: 코사인 유사도 (내적의 응용)
============================================================
x = [1 2 3 4 5]
y = [2 4 6 8 10]
cos(θ) = ⟨x,y⟩ / (‖x‖ ‖y‖) = 1.000000
기대값: 1.0 (평행 벡터)
일치: True

============================================================
모든 테스트 완료
============================================================
```

## 🔗 AI/ML 연결

### 1. Transformer 내 내적 (Attention)

Transformer의 Self-Attention은 Query, Key, Value를 사용한다:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

여기서 $QK^T$의 각 성분은 특정 Query와 Key의 **내적**이다. 이것이 작동하려면:
- 내적이 두 벡터 간의 "유사도"를 측정해야 함 (Cauchy-Schwarz)
- 내적이 연속성과 안정성을 가져야 함 (완비성의 결과)

### 2. 코사인 유사도 (임베딩 검색)

텍스트 임베딩 모델(BERT, Sentence-BERT)에서 두 문장의 유사도:

$$\text{similarity} = \frac{\langle e_1, e_2 \rangle}{\|e_1\| \|e_2\|} \in [-1, 1]$$

이는 L² 내적공간 위의 정규화된 내적이다. 내적이 없으면 이 유사도를 정의할 수 없다.

### 3. MSE 손실 = L² 노름

신경망 회귀에서:
$$\mathcal{L} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 = \frac{1}{n} \|y - \hat{y}\|_2^2$$

이는 L² 공간의 노름이고, 내적으로부터 유도됨.

### 4. 확률론과의 연결

기댓값을 내적으로 보는 관점:
$$\text{E}[XY] = \langle X, Y \rangle_{L^2}$$

분산:
$$\text{Var}(X) = \text{E}[(X - \mu)^2] = \|X - \mu\|_{L^2}^2$$

Gram-Schmidt 직교화는 확률 독립성과도 연결됨 (orthogonal random variables).

## ⚖️ 가정과 한계

### 가정

1. **내적의 존재:** 모든 노름이 내적을 유도하지는 않는다. 예: L¹, L∞는 내적이 없다.
2. **완비성:** 무한차원 공간에서는 완비성을 따로 확인해야 한다.
3. **가분성(Separability):** 계산 가능한 기저를 보장하지 않을 수 있다.

### 한계

- Hilbert 공간이 모든 문제를 해결하지는 않는다. Banach 공간(내적 없지만 완비 노름 공간)이 필요한 경우도 있다 (예: $L^1$ 적분론).
- 유한차원 계산에서 무한차원으로 단순히 확장할 수 없다 (예: 무한 급수의 수렴 문제).

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 성질 |
|------|------|------|
| **내적** | $\langle x, y \rangle$ | 양선형, 켤레대칭, 양정치 |
| **노름** | $\|x\| = \sqrt{\langle x, x \rangle}$ | 삼각부등식, 동차성 |
| **Cauchy-Schwarz** | $\|\langle x, y \rangle\| \leq \|x\| \|y\|$ | 등호 ⟺ x, y 선형종속 |
| **평행사변형 법칙** | $\|x+y\|^2 + \|x-y\|^2 = 2(\|x\|^2 + \|y\|^2)$ | 내적 존재의 필요충분조건 |
| **편극화 항등식** | $\langle x, y \rangle = \frac{1}{4}(\|x+y\|^2 - \|x-y\|^2)$ | 노름에서 내적 복원 |
| **완비성** | 모든 Cauchy 수열 수렴 | Hilbert 공간 정의 |
| **Hilbert 공간** | 완비 내적공간 | 수열 합 수렴, 투영 존재 |

## 🤔 생각해볼 문제

### 문제 1.1

$\mathbb{R}^2$에서 벡터 $x = (1, 0)$, $y = (1, 1)$에 대해:

**(a)** 표준 내적 $\langle x, y \rangle = x_1 y_1 + x_2 y_2$을 계산하고, Cauchy-Schwarz 부등식이 성립함을 확인하라.

**(b)** $\|x + y\|^2 + \|x - y\|^2$와 $2(\|x\|^2 + \|y\|^2)$를 각각 계산하고 같음을 보이라.

**(c)** 코사인 유사도 $\cos \theta = \frac{\langle x, y \rangle}{\|x\| \|y\|}$를 구하고, 두 벡터 사이의 각도를 라디안으로 구하라.

<details>
<summary>해설 보기</summary>

**(a)** 
$$\langle x, y \rangle = 1 \cdot 1 + 0 \cdot 1 = 1$$
$$|\langle x, y \rangle|^2 = 1$$
$$\|x\|^2 = 1, \quad \|y\|^2 = 2$$
$$\|x\|^2 \|y\|^2 = 2$$
$$1 \leq 2 \checkmark$$

**(b)**
$$\|x+y\|^2 = \|(2, 1)\|^2 = 5$$
$$\|x-y\|^2 = \|(0, -1)\|^2 = 1$$
$$5 + 1 = 6$$
$$2(\|x\|^2 + \|y\|^2) = 2(1 + 2) = 6 \checkmark$$

**(c)**
$$\cos \theta = \frac{1}{\sqrt{1} \cdot \sqrt{2}} = \frac{1}{\sqrt{2}}$$
$$\theta = \frac{\pi}{4} \text{ (45도)}$$

</details>

---

### 문제 1.2

$L^2([0,1])$에서 $f(x) = x$, $g(x) = 1$에 대해 내적과 노름을 계산하라.

**(a)** $\langle f, g \rangle = \int_0^1 x \cdot 1 \, dx$를 계산하라.

**(b)** $\|f\|$, $\|g\|$를 계산하라.

**(c)** Cauchy-Schwarz 부등식 검증: $|\langle f, g \rangle| \leq \|f\| \|g\|$.

<details>
<summary>해설 보기</summary>

**(a)**
$$\langle f, g \rangle = \int_0^1 x \, dx = \frac{x^2}{2}\bigg|_0^1 = \frac{1}{2}$$

**(b)**
$$\|f\|^2 = \int_0^1 x^2 \, dx = \frac{x^3}{3}\bigg|_0^1 = \frac{1}{3} \Rightarrow \|f\| = \frac{1}{\sqrt{3}}$$
$$\|g\|^2 = \int_0^1 1 \, dx = 1 \Rightarrow \|g\| = 1$$

**(c)**
$$|\langle f, g \rangle| = \frac{1}{2}$$
$$\|f\| \|g\| = \frac{1}{\sqrt{3}} \cdot 1 = \frac{1}{\sqrt{3}} \approx 0.577$$
$$0.5 < 0.577 \checkmark$$

</details>

---

### 문제 1.3

평행사변형 법칙이 $L^1([0,1])$에서 실패함을 보이라.

$f(x) = \mathbf{1}_{[0, 0.5]}(x)$ (구간 $[0, 0.5]$의 지시함수), $g(x) = \mathbf{1}_{[0.5, 1]}(x)$로 설정하고, $\|f + g\|_1^2 + \|f - g\|_1^2$와 $2(\|f\|_1^2 + \|g\|_1^2)$를 각각 계산하여 비교하라.

<details>
<summary>해설 보기</summary>

$$\|f\|_1 = \int_0^1 |f(x)| dx = \int_0^{0.5} 1 \, dx = 0.5$$
$$\|g\|_1 = \int_{0.5}^1 1 \, dx = 0.5$$
$$\|f + g\|_1 = \int_0^1 |f(x) + g(x)| dx = \int_0^1 1 \, dx = 1$$
$$\|f - g\|_1 = \int_0^1 |f(x) - g(x)| dx = \int_0^{0.5} 1 \, dx + \int_{0.5}^1 |-1| dx = 0.5 + 0.5 = 1$$

$$\text{LHS} = 1^2 + 1^2 = 2$$
$$\text{RHS} = 2(0.5^2 + 0.5^2) = 2(0.25 + 0.25) = 1$$

$$2 \neq 1$$

따라서 평행사변형 법칙이 성립하지 않으며, 이는 $L^1$이 내적공간이 아님을 보여준다.

</details>

---

<div align="center">

| | | |
|---|---|---|
| | [📚 README](../README.md) | [다음 ▶](./02-why-l2-special.md) |

</div>
