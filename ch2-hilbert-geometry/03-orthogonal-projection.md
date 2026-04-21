# 3. 수직투영(Orthogonal Projection) — 무한차원의 최선의 근사

## 🎯 핵심 질문

- 무한차원 공간에서 부분공간으로의 수직투영이 존재하는가?
- 투영 연산자가 가지는 성질은 무엇인가?
- Gram-Schmidt 정규직교화가 무한차원에서도 작동하는가?
- 왜 PCA와 정규화(Ridge Regression)가 투영과 동등한가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원에서는** 벡터를 부분공간으로 정사영하는 것이 단순한 선형대수 문제였다. QR 분해로 직교기저를 구하면 끝.

**무한차원에서는** 부분공간이 "닫혀있지 않을 수" 있다. 그래서 최선의 근사(best approximation)가 존재한다는 것을 **증명**해야 한다. Hilbert 공간의 완비성이 바로 여기서 힘을 발휘한다.

**AI의 핵심 응용:**
1. **PCA (Principal Component Analysis)**: 고차원 데이터를 저차원 부분공간에 투영.
2. **Attention 메커니즘**: Query를 Key-Value 공간에 투영.
3. **정규화 (Ridge/Lasso)**: 가중치를 작은 공간에 투영 (또는 제약).
4. **Manifold 학습**: 고차원 데이터의 저차원 다양체를 학습.
5. **신호 복원 (Compressed Sensing)**: 스파스 신호를 부분공간에 투영.

## 📐 수학적 선행 조건

- 내적공간과 Hilbert 공간 (01-inner-product-hilbert.md)
- 선형공간의 부분공간
- 직교성(orthogonality) 개념
- 완비성(completeness)과 수렴

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서:**
- 벡터 $x \in \mathbb{R}^3$를 평면 $V = \text{span}\{v_1, v_2\}$에 투영.
- 정사영점 $P_V(x)$는 항상 존재하고 유일.
- 투영 공식: $P_V(x) = \langle x, v_1 \rangle v_1 + \langle x, v_2 \rangle v_2$ (정규직교기저인 경우).

**무한차원에서:**
- 부분공간이 닫혀있지 않을 수 있다!
- 예: $L^2$의 유한차원 다항식 부분공간 (조밀하지만 닫혀있지 않음).
- **닫힌** 부분공간 $M$에 대해서만: 투영 $P_M(x)$이 존재하고 유일.
- 투영의 특성화: $x - P_M(x) \perp M$ (수직 특성).

비유: 무한차원 건축에서 지지대를 계획할 때, 그 지지대가 "닫혀있는" (안정적인) 구조여야만 최고의 지점을 찾을 수 있다.

## ✏️ 엄밀한 정의

### 정의 3.1 (수직성)

Hilbert 공간 $H$에서:
- $x \perp y$ (수직): $\langle x, y \rangle = 0$.
- $x \perp M$ ($x$가 부분공간 $M$에 수직): 모든 $m \in M$에 대해 $\langle x, m \rangle = 0$.
- $M^{\perp}$ ($M$의 수직여공간): $M^{\perp} = \{x \in H : x \perp M\}$.

### 정의 3.2 (최근점)

$C \subseteq H$, $x \in H$. 점 $y \in C$가 **$x$의 최근점(nearest point in C)**:
$$\|x - y\| = \inf_{z \in C} \|x - z\|$$

### 정의 3.3 (투영 연산자)

$M$이 Hilbert 공간 $H$의 닫힌 부분공간이면, 각 $x \in H$에 대해 **유일한 투영** $P_M(x) \in M$이 존재하여:
$$x = P_M(x) + (x - P_M(x))$$
여기서 $P_M(x) \in M$, $x - P_M(x) \in M^{\perp}$.

## 🔬 정리와 증명

### 정리 3.4 (최근점의 존재성과 유일성)

$H$가 Hilbert 공간이고 $C$가 닫힌 볼록 부분집합이면, 임의의 $x \in H$에 대해 **유일한** 점 $y_0 \in C$가 존재하여:
$$\|x - y_0\| = \inf_{y \in C} \|x - y\|$$

**증명:**

$d = \inf_{y \in C} \|x - y\|$라 하자.

**1단계: 최소화 수열의 존재**

정의에 의해, 수열 $(y_n) \subseteq C$가 존재하여:
$$\|x - y_n\| \to d \text{ as } n \to \infty$$

**2단계: Cauchy 수열임을 보이기 (평행사변형 법칙 활용)**

$n, m \in \mathbb{N}$에 대해, $y_n, y_m \in C$. $C$가 볼록이므로 $\frac{y_n + y_m}{2} \in C$.

평행사변형 법칙:
$$\left\|x - \frac{y_n + y_m}{2}\right\|^2 + \left\|(x - y_n) - (x - y_m)\right\|^2 = 2\left(\|x - y_n\|^2 + \|x - y_m\|^2\right) / 2$$

정리하면:
$$\left\|\frac{y_n - y_m}{2}\right\|^2 = \frac{\|x - y_n\|^2 + \|x - y_m\|^2}{2} - \left\|x - \frac{y_n + y_m}{2}\right\|^2$$

우변을 평가하면:
- $\|x - y_n\|^2 \to d^2$, $\|x - y_m\|^2 \to d^2$.
- $\left\|x - \frac{y_n + y_m}{2}\right\|^2 \geq d^2$ (infimum).

따라서:
$$\left\|\frac{y_n - y_m}{2}\right\|^2 \leq \frac{d^2 + d^2}{2} - d^2 = 0$$

따라서 $\left\|y_n - y_m\right\| = 2 \left\|\frac{y_n - y_m}{2}\right\| \to 0$.

**3단계: 극한점 수렴 (완비성)**

$(y_n)$이 Cauchy 수열이고 $H$가 완비이므로, $y_0 \in H$가 존재하여 $y_n \to y_0$.

$C$가 닫혀있으므로 $y_0 \in C$.

$\|x - y_0\| = \lim_{n \to \infty} \|x - y_n\| = d$.

**4단계: 유일성**

$y_1, y_2 \in C$가 모두 최근점이면, 평행사변형 법칙을 다시 적용하면:
$$\left\|\frac{y_1 - y_2}{2}\right\|^2 \leq \frac{d^2 + d^2}{2} - d^2 = 0$$

따라서 $y_1 = y_2$. $\square$

---

### 정리 3.5 (부분공간으로의 투영과 수직 특성)

$H$가 Hilbert 공간이고 $M$이 닫힌 부분공간이면, 임의의 $x \in H$에 대해 유일한 $y_0 = P_M(x) \in M$이 존재하여:

**(특성화)** $y_0$은 $M$으로의 최근점이고, 다음과 동치:
$$x - y_0 \perp M \quad (\text{즉, } \langle x - y_0, m \rangle = 0 \text{ for all } m \in M)$$

**증명:**

3.4에서 $C = M$ (닫혀있음)에 적용하면 최근점이 존재한다.

$y_0$가 최근점이라 하자. $m \in M$, $t \in \mathbb{R}$에 대해:
$$\|x - (y_0 + tm)\|^2 \geq \|x - y_0\|^2$$

좌변을 전개하면:
$$\|x - y_0\|^2 - 2t \text{Re}(\langle x - y_0, m \rangle) + t^2 \|m\|^2 \geq \|x - y_0\|^2$$

$$-2t \text{Re}(\langle x - y_0, m \rangle) + t^2 \|m\|^2 \geq 0$$

이것이 모든 $t \in \mathbb{R}$에 대해 성립하려면 (특히 $t \to 0$):
$$\text{Re}(\langle x - y_0, m \rangle) = 0$$

복소수인 경우, $m$을 $im$으로 바꾸면:
$$\text{Im}(\langle x - y_0, m \rangle) = 0$$

따라서 $\langle x - y_0, m \rangle = 0$ for all $m \in M$. $\square$

---

### 정리 3.6 (수직분해)

$H$가 Hilbert 공간이고 $M$이 닫힌 부분공간이면:
$$H = M \oplus M^{\perp}$$

즉, 임의의 $x \in H$는 유일하게 표현된다:
$$x = P_M(x) + P_{M^{\perp}}(x)$$

여기서 $P_{M^{\perp}}(x) = x - P_M(x)$.

**증명:**

**1단계: $M^{\perp}$는 닫힌 부분공간**

$M^{\perp} = \{x \in H : \langle x, m \rangle = 0 \text{ for all } m \in M\}$는 내적이 연속함수이므로 닫혀있다.

**2단계: 직교분해**

3.5에 의해, $x = P_M(x) + (x - P_M(x))$이고 $x - P_M(x) \in M^{\perp}$.

**3단계: 유일성**

$x = m_1 + z_1 = m_2 + z_2$ (단, $m_i \in M, z_i \in M^{\perp}$)이면:
$$m_1 - m_2 = z_2 - z_1$$

좌변은 $M$에 속하고 우변은 $M^{\perp}$에 속한다. 교집합은 $\{0\}$이므로:
$$m_1 = m_2, \quad z_1 = z_2$$

$\square$

---

### 정리 3.7 (투영 연산자의 성질)

$M$이 Hilbert 공간 $H$의 닫힌 부분공간이면, 투영 $P_M : H \to H$는 다음을 만족한다:

**(1)** 선형성: $P_M(\alpha x + \beta y) = \alpha P_M(x) + \beta P_M(y)$.

**(2)** 자기수반성 (self-adjoint): $\langle P_M(x), y \rangle = \langle x, P_M(y) \rangle$.

**(3)** 멱등성 (idempotent): $P_M^2 = P_M$ (즉, $P_M(P_M(x)) = P_M(x)$).

**(4)** 노름: $\|P_M\| = 1$ (단, $M \neq \{0\}$).

**증명:**

**(1) 선형성:**

$x = P_M(x) + z_x$ (단, $z_x \in M^{\perp}$), $y = P_M(y) + z_y$.

$$\alpha x + \beta y = \alpha P_M(x) + \beta P_M(y) + (\alpha z_x + \beta z_y)$$

우변에서 첫 항은 $M$에 속하고 괄호 안은 $M^{\perp}$에 속한다. 유일성에 의해:
$$P_M(\alpha x + \beta y) = \alpha P_M(x) + \beta P_M(y)$$

**(2) 자기수반성:**

$x = P_M(x) + z_x$, $y = P_M(y) + z_y$.

$$\langle P_M(x), y \rangle = \langle P_M(x), P_M(y) + z_y \rangle = \langle P_M(x), P_M(y) \rangle$$

(마지막 항은 0, $P_M(x) \in M$, $z_y \in M^{\perp}$).

마찬가지로 $\langle x, P_M(y) \rangle = \langle P_M(x), P_M(y) \rangle$.

따라서 $\langle P_M(x), y \rangle = \langle x, P_M(y) \rangle$.

**(3) 멱등성:**

$P_M(x) \in M$이므로 $P_M(P_M(x)) = P_M(x)$.

**(4) 노름:**

$$\|P_M(x)\| \leq \|x\| \text{ for all } x$$

(투영은 거리를 감소시키지 않음은 아니지만, 최근점이므로 성립)

따라서 $\|P_M\| = \sup_{\|x\|=1} \|P_M(x)\| \leq 1$.

$M \neq \{0\}$이면, $m \in M$ with $\|m\| = 1$에 대해 $P_M(m) = m$이므로:
$$\|P_M(m)\| = 1$$

따라서 $\|P_M\| = 1$. $\square$

---

### 정리 3.8 (Gram-Schmidt 정규직교화, 무한차원)

가분(separable) Hilbert 공간 $H$에서 선형독립인 가산 수열 $(x_n)$에 대해, 정규직교 수열 $(e_n)$이 존재하여:
$$\text{span}\{x_1, \ldots, x_n\} = \text{span}\{e_1, \ldots, e_n\} \quad \text{for all } n$$

**구성:**

$e_n$을 다음과 같이 귀납적으로 정의:

$$v_n = x_n - \sum_{k=1}^{n-1} \langle x_n, e_k \rangle e_k$$

$$e_n = \frac{v_n}{\|v_n\|}$$

수렴성: $\|v_n\| > 0$ (선형독립이므로), 따라서 모든 $n$에서 정의됨.

**증명 스케치:**

귀납법으로 $\{e_1, \ldots, e_n\}$이 정규직교임을 보인다.

$\langle e_n, e_k \rangle = 0$ for $k < n$:
$$\langle e_n, e_k \rangle = \frac{1}{\|v_n\|} \langle v_n, e_k \rangle = \frac{1}{\|v_n\|} \left( \langle x_n, e_k \rangle - \langle x_n, e_k \rangle \right) = 0$$

$\|e_n\| = 1$은 정의에 의해 자명. $\square$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.linalg import qr

# ============================================================
# 예제 1: ℝ³에서 부분공간으로의 투영
# ============================================================

def projection_r3():
    """ℝ³에서 평면으로의 투영"""
    print("=" * 70)
    print("테스트 1: ℝ³에서 평면으로의 투영")
    print("=" * 70)
    
    # 평면을 span{v1, v2}로 정의
    v1 = np.array([1, 0, 0])
    v2 = np.array([0, 1, 0])
    
    # Gram-Schmidt로 정규직교화
    e1 = v1 / np.linalg.norm(v1)
    v2_orth = v2 - np.dot(v2, e1) * e1
    e2 = v2_orth / np.linalg.norm(v2_orth)
    
    # 벡터 x를 평면에 투영
    x = np.array([1, 2, 3])
    
    # 투영: P_M(x) = ⟨x, e1⟩e1 + ⟨x, e2⟩e2
    proj = np.dot(x, e1) * e1 + np.dot(x, e2) * e2
    
    # 수직 성분
    perp = x - proj
    
    print(f"\n벡터 x = {x}")
    print(f"부분공간 M = span{{{v1}, {v2}}} (xy-평면)")
    print(f"\n투영 P_M(x) = {proj}")
    print(f"수직 성분 x - P_M(x) = {perp}")
    print(f"\n검증: (x - P_M(x)) ⟂ e1: ⟨perp, e1⟩ = {np.dot(perp, e1):.10f}")
    print(f"검증: (x - P_M(x)) ⟂ e2: ⟨perp, e2⟩ = {np.dot(perp, e2):.10f}")
    print(f"\n분해: x = proj + perp")
    print(f"확인: {np.allclose(x, proj + perp)}")


# ============================================================
# 예제 2: 함수공간에서 다항식으로의 투영
# ============================================================

def projection_polynomials():
    """L²([0,1])에서 다항식으로의 투영"""
    print("\n" + "=" * 70)
    print("테스트 2: L²([0,1])에서 다항식으로의 투영")
    print("=" * 70)
    
    # 함수 f(x) = exp(x)를 0차, 1차 다항식으로 근사
    x = np.linspace(0, 1, 1000)
    f = np.exp(x)
    
    # 다항식 기저: 1, x (Gram-Schmidt로 정규직교화)
    p0 = np.ones_like(x)
    p1 = x - np.mean(x)
    
    # 정규화
    e0 = p0 / np.sqrt(np.trapz(p0**2, x))
    e1 = p1 / np.sqrt(np.trapz(p1**2, x))
    
    # 투영 계수
    c0 = np.trapz(f * e0, x)
    c1 = np.trapz(f * e1, x)
    
    # 투영된 함수
    f_proj = c0 * e0 + c1 * e1
    
    # 오차
    error = f - f_proj
    l2_error = np.sqrt(np.trapz(error**2, x))
    
    print(f"\n함수: f(x) = exp(x) on [0,1]")
    print(f"부분공간: 1차 다항식")
    print(f"\n투영 계수:")
    print(f"  c₀ = ⟨f, e₀⟩ = {c0:.6f}")
    print(f"  c₁ = ⟨f, e₁⟩ = {c1:.6f}")
    print(f"\nL² 오차: ‖f - P(f)‖₂ = {l2_error:.6f}")


# ============================================================
# 예제 3: Gram-Schmidt 정규직교화
# ============================================================

def gram_schmidt():
    """Gram-Schmidt 정규직교화 구현"""
    print("\n" + "=" * 70)
    print("테스트 3: Gram-Schmidt 정규직교화")
    print("=" * 70)
    
    # 기저 벡터들 (선형독립이지만 정규직교가 아님)
    v = [
        np.array([1, 1, 0]),
        np.array([0, 1, 1]),
        np.array([1, 0, 1])
    ]
    
    # Gram-Schmidt
    e = []
    for vi in v:
        # 이전 기저로부터 빼기
        u = vi.copy()
        for ej in e:
            u -= np.dot(vi, ej) * ej
        
        # 정규화
        norm_u = np.linalg.norm(u)
        if norm_u > 1e-10:
            e.append(u / norm_u)
    
    print(f"\n입력 벡터:")
    for i, vi in enumerate(v):
        print(f"  v{i+1} = {vi}")
    
    print(f"\n정규직교기저:")
    for i, ei in enumerate(e):
        print(f"  e{i+1} = {ei}")
    
    # 검증
    print(f"\n정규직교성 검증:")
    for i in range(len(e)):
        for j in range(len(e)):
            dot = np.dot(e[i], e[j])
            if i == j:
                print(f"  ⟨e{i+1}, e{j+1}⟩ = {dot:.10f} (should be 1)")
            else:
                print(f"  ⟨e{i+1}, e{j+1}⟩ = {dot:.10f} (should be 0)")


# ============================================================
# 예제 4: 투영 연산자의 성질 검증
# ============================================================

def projection_operator_properties():
    """투영 연산자의 성질"""
    print("\n" + "=" * 70)
    print("테스트 4: 투영 연산자의 성질")
    print("=" * 70)
    
    # 2D 부분공간 (ℝ⁵에서)
    v1 = np.array([1, 0, 0, 0, 0])
    v2 = np.array([1, 1, 0, 0, 0])
    v2 = v2 / np.linalg.norm(v2)
    
    # 투영 행렬
    P = np.outer(v1, v1) + np.outer(v2, v2)
    
    # 랜덤 벡터
    x = np.random.randn(5)
    y = np.random.randn(5)
    
    # 성질 검증
    Px = P @ x
    Py = P @ y
    
    # (1) 선형성
    alpha, beta = 2.5, -1.3
    linear_check = np.allclose(P @ (alpha*x + beta*y), 
                               alpha*Px + beta*Py)
    
    # (2) 자기수반성: ⟨Px, y⟩ = ⟨x, Py⟩
    self_adjoint_check = np.isclose(np.dot(Px, y), np.dot(x, Py))
    
    # (3) 멱등성: P² = P
    idempotent_check = np.allclose(P @ P, P)
    
    # (4) 노름
    norm_P = np.linalg.norm(P)
    
    print(f"\n선형성 검증: α*Px + β*Py = P(αx + βy)")
    print(f"  {linear_check}")
    
    print(f"\n자기수반성 검증: ⟨Px, y⟩ = ⟨x, Py⟩")
    print(f"  ⟨Px, y⟩ = {np.dot(Px, y):.10f}")
    print(f"  ⟨x, Py⟩ = {np.dot(x, Py):.10f}")
    print(f"  {self_adjoint_check}")
    
    print(f"\n멱등성 검증: P² = P")
    print(f"  {idempotent_check}")
    
    print(f"\n노름: ‖P‖ ≈ {norm_P:.6f}")


# ============================================================
# 예제 5: PCA를 투영으로 본다
# ============================================================

def pca_as_projection():
    """PCA는 주성분 부분공간으로의 투영"""
    print("\n" + "=" * 70)
    print("테스트 5: PCA를 투영으로 해석")
    print("=" * 70)
    
    np.random.seed(42)
    
    # 2D 데이터 (실제로는 타원형 분포)
    n_samples = 100
    X = np.random.randn(n_samples, 2)
    X[:, 0] *= 3  # 첫 번째 축을 확대
    
    # 평균 빼기
    X_mean = np.mean(X, axis=0)
    X_centered = X - X_mean
    
    # SVD로 주성분 구하기
    U, S, Vt = np.linalg.svd(X_centered, full_matrices=False)
    
    # 첫 번째 주성분 (가장 큰 분산)
    v1 = Vt[0]
    
    # 첫 주성분 축으로의 투영
    X_proj_1d = X_centered @ np.outer(v1, v1)
    
    # 원본과 투영의 오차
    error = X_centered - X_proj_1d
    l2_error = np.linalg.norm(error, 'fro')
    
    print(f"\n데이터 크기: {X.shape}")
    print(f"주성분 1: {v1}")
    print(f"분산: {S[0]**2 / (n_samples - 1):.6f}")
    print(f"\n1D 투영 후 재구성 오차: {l2_error:.6f}")
    print(f"\n이것이 PCA의 원리다:")
    print(f"  데이터를 주성분(부분공간)에 투영")
    print(f"  → 분산 최대화")
    print(f"  → 정보 손실 최소화")


# ============================================================
# 메인 실행
# ============================================================

if __name__ == "__main__":
    projection_r3()
    projection_polynomials()
    gram_schmidt()
    projection_operator_properties()
    pca_as_projection()
    
    print("\n" + "=" * 70)
    print("모든 테스트 완료")
    print("=" * 70)
```

**실행 결과:**
```
======================================================================
테스트 1: ℝ³에서 평면으로의 투영
======================================================================

벡터 x = [1 2 3]
부분공간 M = span{[1 0 0], [0 1 0]} (xy-평면)

투영 P_M(x) = [1. 2. 0.]
수직 성분 x - P_M(x) = [0. 0. 3.]

검증: (x - P_M(x)) ⟂ e1: ⟨perp, e1⟩ = 0.0000000000
검증: (x - P_M(x)) ⟂ e2: ⟨perp, e2⟩ = 0.0000000000

분해: x = proj + perp
확인: True
```

## 🔗 AI/ML 연결

### 1. PCA (주성분 분석)

고차원 데이터 $X \in \mathbb{R}^{n \times d}$를 저차원 부분공간 $M$에 투영:
$$X_{proj} = X V_k$$
여기서 $V_k$는 상위 $k$개 주성분.

**수학적 의미**: 분산을 최대화하는 $k$-차원 부분공간을 찾는다 = 투영.

### 2. Attention 메커니즘

Query $Q$를 Key-Value 공간에 투영:
$$\text{Attention}(Q, K, V) = \text{softmax}(QK^T) V$$

이것도 어떤 의미에서 "투영"에 가깝다 (완전한 투영은 아니지만, 가중 합).

### 3. Ridge Regression (정규화)

가중치 $w$를 작은 공간에 제약:
$$\min_w \|y - Xw\|_2^2 + \lambda \|w\|_2^2$$

이는 $w$를 "작은" 부분공간에 투영하는 것과 동등한 효과.

### 4. Manifold 학습

고차원 데이터가 실제로는 저차원 다양체 위에 있다고 가정. 투영으로 그 다양체를 찾는다 (Laplacian Eigenmap, Isomap).

### 5. Compressed Sensing

스파스 신호 $s$를 스파스 부분공간에 투영하여 복원:
$$\min_s \|As - y\|_2^2 \text{ subject to } \|s\|_0 \text{ small}$$

## ⚖️ 가정과 한계

### 가정

1. **닫힌 부분공간**: 부분공간이 닫혀있어야 투영이 유일하게 존재.
2. **완비성**: Hilbert 공간의 필수 조건.
3. **볼록성**: 최근점의 유일성을 위해.

### 한계

1. **무한차원의 부분공간**: 닫혀있지 않으면 투영이 존재하지 않을 수 있다.
2. **비선형 투영**: Hilbert 기하학은 선형 공간에만 적용.
3. **계산 복잡도**: 무한차원에서 정규직교기저를 명시적으로 구할 수 없을 수 있다.

## 📌 핵심 정리 (표로 요약)

| 개념 | 유한차원 | 무한차원 (Hilbert) |
|------|---------|-------------------|
| **최근점** | 항상 존재 (QR) | 닫힌 볼록집합에서 존재 |
| **투영 유일성** | 자명 | 완비성 필수 |
| **투영 선형성** | 자명 | 부분공간 닫혀있어야 함 |
| **수직 특성** | $x - P(x) \perp M$ | 같음 |
| **직교분해** | $H = M \oplus M^{\perp}$ | 닫힌 부분공간에서 성립 |
| **Gram-Schmidt** | 유한 수열 | 가산 수열, 수렴 확인 필요 |
| **응용** | 선형대수 | PCA, Ridge, Manifold 학습 |

## 🤔 생각해볼 문제

### 문제 3.1

$\mathbb{R}^3$에서 직선 $L = \text{span}\{(1, 1, 1)\}$로의 투영을 구하라.

벡터 $x = (1, 2, 3)$에 대해:

**(a)** 기저벡터를 정규화하라.

**(b)** 투영 $P_L(x)$를 구하라.

**(c)** 수직 성분 $x - P_L(x)$를 구하고, 이것이 $L$에 수직임을 확인하라.

<details>
<summary>해설 보기</summary>

**(a)** 
$$v = (1, 1, 1), \quad \|v\| = \sqrt{3}$$
$$e = \frac{1}{\sqrt{3}}(1, 1, 1)$$

**(b)**
$$P_L(x) = \langle x, e \rangle e = \frac{1 + 2 + 3}{\sqrt{3}} \cdot \frac{1}{\sqrt{3}}(1, 1, 1) = 2(1, 1, 1) = (2, 2, 2)$$

**(c)**
$$x - P_L(x) = (1, 2, 3) - (2, 2, 2) = (-1, 0, 1)$$

검증:
$$\langle (-1, 0, 1), (1, 1, 1) \rangle = -1 + 0 + 1 = 0 \checkmark$$

</details>

---

### 문제 3.2

$L^2([0,1])$에서 상수함수 $M = \{c : c \in \mathbb{R}\}$로의 투영을 구하라.

함수 $f(x) = x$에 대해:

**(a)** 상수 기저를 정규화하라.

**(b)** $f$의 투영을 구하라.

**(c)** 투영이 기댓값과 같음을 보이라.

<details>
<summary>해설 보기</summary>

**(a)**
기저: $v(x) = 1$, $\|v\|_{L^2}^2 = \int_0^1 1 dx = 1$, 따라서 $e(x) = 1$.

**(b)**
$$P_M(f) = \langle f, e \rangle e = \left(\int_0^1 x \cdot 1 dx\right) \cdot 1 = \frac{1}{2}$$

**(c)**
이 상수는 $f$의 $L^2$ 평균이며, 기댓값 개념과 같다:
$$\text{E}[f] = \int_0^1 f(x) dx = \frac{1}{2}$$

</details>

---

### 문제 3.3

Gram-Schmidt 정규직교화를 다음 벡터들에 적용하라:
$$v_1 = (1, 0, 0), \quad v_2 = (1, 1, 0), \quad v_3 = (1, 1, 1)$$

정규직교기저 $(e_1, e_2, e_3)$을 구하고, 각 단계에서 계산하는 수직 성분을 명시하라.

<details>
<summary>해설 보기</summary>

**Step 1:**
$$e_1 = \frac{v_1}{\|v_1\|} = \frac{(1,0,0)}{1} = (1, 0, 0)$$

**Step 2:**
$$u_2 = v_2 - \langle v_2, e_1 \rangle e_1 = (1,1,0) - 1 \cdot (1,0,0) = (0, 1, 0)$$
$$e_2 = \frac{u_2}{\|u_2\|} = \frac{(0,1,0)}{1} = (0, 1, 0)$$

**Step 3:**
$$u_3 = v_3 - \langle v_3, e_1 \rangle e_1 - \langle v_3, e_2 \rangle e_2$$
$$= (1,1,1) - 1 \cdot (1,0,0) - 1 \cdot (0,1,0) = (0, 0, 1)$$
$$e_3 = \frac{u_3}{\|u_3\|} = (0, 0, 1)$$

결과: $(e_1, e_2, e_3) = ((1,0,0), (0,1,0), (0,0,1))$ (표준기저)

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./02-why-l2-special.md) | [📚 README](../README.md) | [다음 ▶](./04-riesz-representation.md) |

</div>
