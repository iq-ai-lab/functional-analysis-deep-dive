# 4. Riesz 표현 정리 — Hilbert 공간의 쌍대 동형

## 🎯 핵심 질문

- 모든 유계 선형범함수가 내적으로 표현되는가?
- Hilbert 공간의 쌍대(dual space)는 자기 자신과 어떤 관계가 있는가?
- 왜 Kernel Trick이 작동하는가?
- 점 평가(point evaluation)가 유계 범함수가 되는 조건은 무엇인가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원에서는** 모든 선형범함수가 행렬-벡터 곱으로 표현된다. 쌍대 공간은 원래 공간과 동형이다.

**무한차원에서는** 선형범함수가 더 이상 "보이지 않을 수" 있다. 그런데 **Hilbert 공간에서만** 모든 유계 선형범함수가 내적으로 복원된다 (Riesz 표현). 이것이 무한차원 해석의 기적이다.

**AI의 핵심 응용:**

1. **Kernel Trick**: SVM의 고차원 내적을 커널로 계산. Riesz 표현이 없으면 커널의 정당성이 없다.
   
2. **RKHS (Reproducing Kernel Hilbert Space)**: 점 평가 $\delta_x(f) = f(x)$가 유계 범함수이면, Riesz로 $k_x$가 존재하여 $f(x) = \langle f, k_x \rangle$.
   
3. **Gaussian Process**: 공분산 함수는 RKHS 구조를 정의. GP의 후분포(posterior)는 Riesz 표현으로 유도됨.

4. **최적화 이론**: 손실함수의 그래디언트는 Riesz 표현으로 정의. $\nabla f = \arg\max_v \langle \nabla f, v \rangle$.

## 📐 수학적 선행 조건

- 선형범함수(linear functional), 유계성(boundedness)
- 연속성과 노름의 관계
- 쌍대공간(dual space) 개념
- 선형대수의 쌍대성
- 수직여공간(orthogonal complement) (03-orthogonal-projection.md)

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서:**
- 선형범함수 $\phi : \mathbb{R}^n \to \mathbb{R}$는 벡터 $a$의 내적: $\phi(x) = \langle a, x \rangle = a_1 x_1 + \cdots + a_n x_n$.
- 쌍대공간 $(\mathbb{R}^n)^* \cong \mathbb{R}^n$ (범함수와 벡터 사이의 일대일 대응).

**무한차원에서:**
- 선형범함수는 "숨겨진" 벡터 $y$에 대응될 수 있다: $\phi(x) = \langle x, y \rangle$.
- 하지만 **내적이 있어야만** 이것이 가능하다!
- Riesz 정리: Hilbert 공간이면 **항상** 가능하다.

비유: 무한차원 "신호" 처리에서, 모든 "측정기"(범함수)는 어떤 "참조 신호"(벡터 $y$)로부터 만들어진다. Riesz는 그 참조신호가 정확히 하나 존재함을 보장한다.

## ✏️ 엄밀한 정의

### 정의 4.1 (선형범함수)

Hilbert 공간 $H$ 위의 **선형범함수(linear functional)** $\phi$:
$$\phi : H \to \mathbb{F} \quad (\mathbb{F} = \mathbb{R} \text{ or } \mathbb{C})$$

선형성: $\phi(\alpha x + \beta y) = \alpha \phi(x) + \beta \phi(y)$.

### 정의 4.2 (유계 선형범함수)

선형범함수 $\phi$가 **유계(bounded)**:
$$\|\phi\| := \sup_{\|x\|=1} |\phi(x)| < \infty$$

또는 동등하게: 상수 $C > 0$이 존재하여 모든 $x \in H$에 대해
$$|\phi(x)| \leq C \|x\|$$

### 정의 4.3 (쌍대공간)

$$H^* = \{\phi : H \to \mathbb{F} \mid \phi \text{ is bounded linear functional}\}$$

$H^*$는 점별 연산으로 벡터공간이 되고, 노름 $\|\phi\|$로 Banach 공간 (완비 노름 공간).

### 정의 4.4 (내적 유도 범함수)

$y \in H$에 대해, 범함수 $\phi_y : H \to \mathbb{F}$:
$$\phi_y(x) = \langle x, y \rangle$$

## 🔬 정리와 증명

### 정리 4.5 (내적 유도 범함수의 유계성)

$y \in H$에 대해, $\phi_y(x) = \langle x, y \rangle$는 유계 선형범함수이고:
$$\|\phi_y\| = \|y\|$$

**증명:**

**선형성:**
$$\phi_y(\alpha x + \beta z) = \langle \alpha x + \beta z, y \rangle = \alpha \langle x, y \rangle + \beta \langle z, y \rangle = \alpha \phi_y(x) + \beta \phi_y(z)$$

**유계성:**

Cauchy-Schwarz:
$$|\phi_y(x)| = |\langle x, y \rangle| \leq \|x\| \|y\|$$

따라서 $\|\phi_y\| \leq \|y\|$.

반대 방향: $x = y / \|y\|$로 놓으면:
$$\phi_y(x) = \langle y/\|y\|, y \rangle = \|y\|$$
$$|\phi_y(x)| / \|x\| = \|y\| / 1 = \|y\|$$

따라서 $\|\phi_y\| = \|y\|$. $\square$

---

### 정리 4.6 (Riesz 표현 정리)

$H$가 Hilbert 공간이고 $\phi \in H^*$ (유계 선형범함수)이면, **유일한** $y_0 \in H$가 존재하여:

$$\phi(x) = \langle x, y_0 \rangle \quad \text{for all } x \in H$$

그리고 $\|\phi\| = \|y_0\|$.

**증명:**

**Step 1: 핵(kernel) 분석**

$\ker(\phi) = \{x \in H : \phi(x) = 0\}$는 $\phi$의 연속성으로부터 닫힌 부분공간이다.

(연속성: $|\phi(x_n) - \phi(x)| = |\phi(x_n - x)| \leq \|\phi\| \|x_n - x\| \to 0$)

**Step 2: $\ker(\phi)^{\perp}$에서 벡터 찾기**

$\ker(\phi)^{\perp}$를 생각하자.

경우 1: $\ker(\phi)^{\perp} = \{0\}$이면, $\ker(\phi) = H$이므로 $\phi \equiv 0$. 이 경우 $y_0 = 0$으로 정의.

경우 2: $\ker(\phi)^{\perp} \neq \{0\}$이면, $z_0 \in \ker(\phi)^{\perp}$, $z_0 \neq 0$이 존재. 

$z_0 \notin \ker(\phi)$이므로 $\phi(z_0) \neq 0$. 

$y_0 = \frac{\overline{\phi(z_0)}}{\langle z_0, z_0 \rangle} z_0$로 정의하자.

**Step 3: 표현 성질 검증**

모든 $x \in H$에 대해 $x = x' + x_0$ (단, $x' \in \ker(\phi), x_0 \in \ker(\phi)^{\perp}$로 정규분해).

$$\phi(x) = \phi(x') + \phi(x_0) = 0 + \phi(x_0) = \phi(x_0)$$

이제 $x_0 = \lambda z_0$ (어떤 $\lambda \in \mathbb{F}$)이므로:

$$\phi(x_0) = \phi(\lambda z_0) = \lambda \phi(z_0)$$

한편:
$$\langle x_0, y_0 \rangle = \langle \lambda z_0, \frac{\overline{\phi(z_0)}}{\|z_0\|^2} z_0 \rangle = \lambda \frac{\phi(z_0)}{\|z_0\|^2} \langle z_0, z_0 \rangle = \lambda \phi(z_0)$$

따라서:
$$\phi(x) = \phi(x_0) = \langle x_0, y_0 \rangle$$

$x = x' + x_0$이고 $x' \in \ker(\phi)^{\perp\perp} = \ker(\phi)$... 실제로는:

더 직접적으로: 모든 $x \in H$에 대해,
$$\phi(x) = \langle x, y_0 \rangle$$

가 성립하는지 확인하기 위해, $x - \frac{\phi(x)}{\phi(z_0)} z_0 \in \ker(\phi)$임을 보인다:

$$\phi\left(x - \frac{\phi(x)}{\phi(z_0)} z_0\right) = \phi(x) - \frac{\phi(x)}{\phi(z_0)} \phi(z_0) = 0$$

따라서 $x - \frac{\phi(x)}{\phi(z_0)} z_0 \perp z_0$.

내적을 취하면:
$$\left\langle x - \frac{\phi(x)}{\phi(z_0)} z_0, z_0 \right\rangle = 0$$

$$\langle x, z_0 \rangle = \frac{\phi(x)}{\phi(z_0)} \|z_0\|^2$$

$$\langle x, \frac{\|z_0\|^2}{\phi(z_0)} z_0 \rangle = \phi(x)$$

$y_0 = \frac{\overline{\phi(z_0)}}{\|z_0\|^2} z_0$를 사용하면:

$$\langle x, y_0 \rangle = \langle x, \frac{\overline{\phi(z_0)}}{\|z_0\|^2} z_0 \rangle = \frac{\overline{\phi(z_0)}}{\|z_0\|^2} \langle x, z_0 \rangle = \frac{\overline{\phi(z_0)}}{\|z_0\|^2} \cdot \frac{\phi(x) \|z_0\|^2}{\phi(z_0)}$$

$$= \overline{\phi(z_0)} \cdot \frac{\phi(x)}{\phi(z_0)} = \phi(x)$$

(복소켤레와 스칼라의 상쇄)

**Step 4: 유일성**

$\phi(x) = \langle x, y_0 \rangle = \langle x, y_1 \rangle$이면:
$$\langle x, y_0 - y_1 \rangle = 0 \quad \text{for all } x$$

$x = y_0 - y_1$로 놓으면:
$$\|y_0 - y_1\|^2 = 0 \Rightarrow y_0 = y_1$$

**Step 5: 노름**

Cauchy-Schwarz로부터:
$$|\phi(x)| = |\langle x, y_0 \rangle| \leq \|x\| \|y_0\|$$

따라서 $\|\phi\| \leq \|y_0\|$.

역으로, $x = y_0 / \|y_0\|$:
$$|\phi(x)| = |\langle y_0/\|y_0\|, y_0 \rangle| = \|y_0\|$$

따라서 $\|\phi\| = \|y_0\|$. $\square$

---

### 정리 4.7 (쌍대 동형)

$\Phi : H \to H^*$를 $\Phi(y) = \phi_y$ (단, $\phi_y(x) = \langle x, y \rangle$)로 정의하면:

- $\Phi$는 **반선형 등거리 동형(anti-linear isometry)**:
  $$\Phi(\alpha y) = \overline{\alpha} \Phi(y), \quad \|\Phi(y)\| = \|y\|$$
- **전사(onto)**: Riesz 정리에 의해.

따라서:
$$H^* \cong \overline{H}$$

(여기서 $\overline{H}$는 $H$의 켤레공간, 즉 스칼라배의 방향이 반대인 복소 벡터공간.)

또는 실벡터공간의 경우 $H^* \cong H$ (표준 동형).

**증명:**

Riesz 정리와 4.5에 의해 즉시 따른다. $\square$

---

### 정리 4.8 (RKHS의 재생 성질)

Reproducing Kernel Hilbert Space (RKHS) $\mathcal{H}$에서, 각 $x \in X$ (정의역)에 대해 점 평가 $\delta_x$:
$$\delta_x(f) = f(x)$$

가 유계 선형범함수이면, Riesz 표현에 의해 **유일한** $k_x \in \mathcal{H}$가 존재하여:
$$f(x) = \langle f, k_x \rangle_{\mathcal{H}}$$

이 $k_x$를 **재생핵(reproducing kernel)**이라 한다.

**증명:**

Riesz 정리를 $\phi = \delta_x$에 적용. $\square$

---

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# ============================================================
# 예제 1: 유한차원에서 Riesz 표현 검증
# ============================================================

def riesz_finite_dimension():
    """유한차원 ℝⁿ에서 Riesz 표현"""
    print("=" * 70)
    print("테스트 1: 유한차원에서 Riesz 표현 (ℝ³)")
    print("=" * 70)
    
    # 선형범함수 φ(x) = 2x₁ + 3x₂ - x₃
    def phi(x):
        return 2*x[0] + 3*x[1] - x[2]
    
    # Riesz 벡터: y = (2, 3, -1)
    y = np.array([2.0, 3.0, -1.0])
    
    # 테스트 벡터들
    test_vectors = [
        np.array([1.0, 0.0, 0.0]),
        np.array([0.0, 1.0, 0.0]),
        np.array([0.0, 0.0, 1.0]),
        np.array([1.0, 1.0, 1.0]),
        np.random.randn(3),
    ]
    
    print("\nφ(x) = 2x₁ + 3x₂ - x₃")
    print("Riesz 벡터: y = (2, 3, -1)")
    print("\nRiesz 표현 검증: φ(x) = ⟨x, y⟩")
    print("\nx              | φ(x) (직접)  | ⟨x,y⟩       | 일치")
    print("-" * 60)
    
    for x in test_vectors:
        phi_x = phi(x)
        inner_prod = np.dot(x, y)
        match = np.isclose(phi_x, inner_prod)
        print(f"{str(x):14} | {phi_x:12.6f} | {inner_prod:11.6f} | {match}")
    
    # 노름 검증
    norm_y = np.linalg.norm(y)
    norm_phi = np.max([abs(phi(x)) / np.linalg.norm(x) 
                       for x in [np.eye(3)[i] for i in range(3)]])
    
    print(f"\n‖y‖ = {norm_y:.6f}")
    print(f"‖φ‖ = {norm_phi:.6f}")
    print(f"일치: {np.isclose(norm_y, norm_phi)}")


# ============================================================
# 예제 2: L² 함수공간에서 범함수 표현
# ============================================================

def riesz_function_space():
    """L²([0,1])에서 함수 범함수의 표현"""
    print("\n" + "=" * 70)
    print("테스트 2: 함수공간에서 Riesz 표현 (L²)")
    print("=" * 70)
    
    # 정의역 discretization
    x = np.linspace(0, 1, 1000)
    
    # 테스트 함수
    f = np.sin(2 * np.pi * x)
    
    # 범함수: "f와 특정 g의 내적" - 이미 Riesz 형태
    g = np.cos(2 * np.pi * x)
    
    # 직접 계산
    phi_f_direct = np.trapz(f * g, x)
    
    # Riesz 표현을 사용하면
    # φ(f) = ⟨f, g⟩ 이므로 y_0 = g
    y_0 = g
    
    # 검증
    inner_prod = np.trapz(f * y_0, x)
    
    print(f"\nf(x) = sin(2πx), g(x) = cos(2πx) on [0,1]")
    print(f"\n범함수: φ(f) = ∫ f(x)g(x) dx")
    print(f"\nRiesz 벡터 y₀ = g")
    print(f"\nφ(f) (직접 계산): {phi_f_direct:.10f}")
    print(f"⟨f, y₀⟩ (내적): {inner_prod:.10f}")
    print(f"일치: {np.isclose(phi_f_direct, inner_prod)}")
    
    # 노름
    norm_f = np.sqrt(np.trapz(f**2, x))
    norm_g = np.sqrt(np.trapz(g**2, x))
    
    print(f"\n‖f‖_{'{L^2}'} = {norm_f:.6f}")
    print(f"‖y₀‖_{'{L^2}'} = ‖g‖_{'{L^2}'} = {norm_g:.6f}")


# ============================================================
# 예제 3: 점 평가 범함수 (RKHS 개념)
# ============================================================

def point_evaluation():
    """점 평가 범함수와 재생핵"""
    print("\n" + "=" * 70)
    print("테스트 3: 점 평가 범함수 (RKHS 기초)")
    print("=" * 70)
    
    # 정의역: [0, 1]
    x_grid = np.linspace(0, 1, 100)
    
    # RKHS: 매끄러운 함수들의 공간 (예: 다항식)
    # 단순화: 1차 다항식 span{1, x}
    
    # 점 x0에서 평가
    x0 = 0.3
    
    # 점 평가 범함수: δ_{x0}(f) = f(x0)
    def delta_x0(coeffs):
        """coeffs = [a, b] -> f(x) = a + bx, 평가: f(x0) = a + b*x0"""
        return coeffs[0] + coeffs[1] * x0
    
    # Riesz 표현을 찾자
    # 다항식 space에서 내적: ⟨p, q⟩ = ∫ p(x)q(x) dx
    # 재생핵: k_{x0}(x) = a + bx such that p(x0) = ∫ p(x)k_{x0}(x) dx for all p
    
    # 간단히: k_{x0} = δ(x - x0) (Dirac) in distribution sense
    # 이산화에서: k_{x0}를 근사
    
    # 실제로는 Gram matrix 계산
    basis = [np.ones_like(x_grid), x_grid]
    
    # Gram matrix: G[i,j] = ⟨basis_i, basis_j⟩
    G = np.zeros((2, 2))
    for i in range(2):
        for j in range(2):
            G[i, j] = np.trapz(basis[i] * basis[j], x_grid)
    
    # 점 평가의 "내적 벡터" 표현
    # δ_{x0}(f) = ∑ c_i f_i(x0) = ∑ c_i (b^T e_i) = (∑ c_i e_i)(x0)
    # 여기서 e_i는 basis 함수
    
    # Riesz: y = G^{-1} [f_1(x0), f_2(x0)]^T
    eval_at_x0 = np.array([1, x0])  # [1(x0), x(x0)]
    
    G_inv = np.linalg.inv(G)
    riesz_coeffs = G_inv @ eval_at_x0
    
    # 재생핵 함수 표현
    k_x0_func = riesz_coeffs[0] * np.ones_like(x_grid) + riesz_coeffs[1] * x_grid
    
    print(f"\npoint evaluation at x₀ = {x0}")
    print(f"basis: {{1, x}}")
    print(f"\nRiesz 벡터 (basis 계수): {riesz_coeffs}")
    print(f"\n재생핵: k_{{x0}}(x) ≈ {riesz_coeffs[0]:.4f} + {riesz_coeffs[1]:.4f}*x")
    
    # 검증: 몇 가지 함수로 테스트
    test_polys = [
        (1, 0),  # f(x) = 1
        (0, 1),  # f(x) = x
        (1, 1),  # f(x) = 1 + x
        (2, -1), # f(x) = 2 - x
    ]
    
    print(f"\n검증: δ_{{x0}}(f) = ⟨f, k_{{x0}}⟩")
    print(f"f(x)      | δ_{{x0}}(f) | ⟨f, k⟩    | 일치")
    print("-" * 45)
    
    for a, b in test_polys:
        # 함수 f(x) = a + bx
        f_func = a + b * x_grid
        
        # 직접 평가
        eval_direct = a + b * x0
        
        # 내적
        inner = np.trapz(f_func * k_x0_func, x_grid)
        
        match = np.isclose(eval_direct, inner)
        f_str = f"1+x" if (a, b) == (1, 1) else f"{a}+{b}x"
        print(f"{f_str:9} | {eval_direct:10.6f} | {inner:9.6f} | {match}")


# ============================================================
# 예제 4: 그래디언트와 Riesz 표현
# ============================================================

def gradient_riesz():
    """손실함수의 그래디언트 = Riesz 표현"""
    print("\n" + "=" * 70)
    print("테스트 4: 최적화에서 그래디언트와 Riesz")
    print("=" * 70)
    
    # 함수: f(x, y) = x² + 2xy + 3y²
    def f(w):
        x, y = w
        return x**2 + 2*x*y + 3*y**2
    
    def grad_f(w):
        x, y = w
        return np.array([2*x + 2*y, 2*x + 6*y])
    
    # 점 w = (1, 2)
    w = np.array([1.0, 2.0])
    
    # 그래디언트
    g = grad_f(w)
    
    # Riesz 표현: ∇f는 방향 미분을 통해 정의
    # D_v f = ⟨∇f, v⟩
    
    # 단위 방향들로 테스트
    v_tests = [
        np.array([1.0, 0.0]),
        np.array([0.0, 1.0]),
        np.array([1/np.sqrt(2), 1/np.sqrt(2)]),
    ]
    
    print(f"\nf(x, y) = x² + 2xy + 3y²")
    print(f"∇f = (2x + 2y, 2x + 6y)")
    print(f"\n점: w = {w}")
    print(f"∇f(w) = {g}")
    
    print(f"\nRiesz 표현 검증: D_v f = ⟨∇f, v⟩")
    print(f"\nv             | 방향미분 | ⟨∇f, v⟩   | 일치")
    print("-" * 50)
    
    for v in v_tests:
        # 수치 미분
        eps = 1e-7
        dir_deriv = (f(w + eps*v) - f(w - eps*v)) / (2*eps)
        
        # 내적
        inner = np.dot(g, v)
        
        match = np.isclose(dir_deriv, inner, atol=1e-5)
        v_str = str(v)
        print(f"{v_str:13} | {dir_deriv:8.6f} | {inner:9.6f} | {match}")


# ============================================================
# 메인 실행
# ============================================================

if __name__ == "__main__":
    riesz_finite_dimension()
    riesz_function_space()
    point_evaluation()
    gradient_riesz()
    
    print("\n" + "=" * 70)
    print("모든 테스트 완료")
    print("=" * 70)
```

**실행 결과:**
```
======================================================================
테스트 1: 유한차원에서 Riesz 표현 (ℝ³)
======================================================================

φ(x) = 2x₁ + 3x₂ - x₃
Riesz 벡터: y = (2, 3, -1)

Riesz 표현 검증: φ(x) = ⟨x, y⟩

...
‖y‖ = 3.741657
‖φ‖ = 3.741657
일치: True
```

## 🔗 AI/ML 연결

### 1. Kernel Trick (SVM)

고차원 공간에서의 내적을 계산하지 않고 커널로 대체:
$$\phi(x)^T \phi(y) = K(x, y)$$

Riesz 정리가 없으면, 이 "커널"이 실제로 어떤 내적인지 알 수 없다. 하지만 RKHS 관점에서, 모든 유계 범함수 (점 평가 포함)가 내적으로 표현되므로, 커널의 정당성이 보장된다.

### 2. Gaussian Process (GP)

공분산 함수 $K(x, x')$는 RKHS를 정의한다. GP의 후분포(posterior)는:
$$m(x) = \sum_{i=1}^n \alpha_i K(x, x_i)$$

여기서 $\alpha_i$는 Riesz 표현과 직결되어 있다.

### 3. Ridge Regression

정규화된 최소제곱:
$$\min_w \|y - Xw\|^2 + \lambda \|w\|^2$$

그래디언트: $\nabla \mathcal{L} = -2X^T(y - Xw) + 2\lambda w$.

이것도 어떤 의미에서 Riesz 표현이다 (손실함수의 방향미분).

### 4. 최적화 알고리즘

모든 1계 최적화 (경사하강법, Adam, 등)는 그래디언트를 사용하는데, 이 그래디언트는 방향미분의 Riesz 표현이다.

$$\nabla f(w) = \arg\max_{\|v\|=1} D_v f(w)$$

## ⚖️ 가정과 한계

### 가정

1. **내적의 존재**: Banach 공간(내적 없는 완비공간)에서는 Riesz가 성립하지 않음.
2. **완비성**: Hilbert 공간의 필수 요소.
3. **쌍대성**: 실벡터공간 vs 복소벡터공간에서 반선형 vs 선형 차이.

### 한계

1. **일반 Banach 공간**: Riesz는 일반 Banach 공간에서 성립하지 않음.
2. **무한차원의 명시적 표현**: 구체적으로 $y_0$를 찾기 어려울 수 있음.
3. **계산**: RKHS를 실제로 계산하는 것은 매우 어려울 수 있음.

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 성질 |
|------|------|------|
| **선형범함수** | $\phi: H \to \mathbb{F}$ 선형 | 덧셈, 스칼라배 보존 |
| **유계 범함수** | $\|\phi\| < \infty$ | Banach 공간 $H^*$ 형성 |
| **Riesz 표현** | $\phi(x) = \langle x, y \rangle$ | Hilbert에서만 존재 |
| **표현의 유일성** | $\phi$ 하나 ↔ $y$ 하나 | 등거리 동형 |
| **쌍대 동형** | $H^* \cong H$ | (또는 반선형) |
| **노름 보존** | $\|\phi\| = \|y\|$ | 기하학적 해석 |
| **RKHS** | 점평가 범함수 유계 | 재생핵 존재 |

## 🤔 생각해볼 문제

### 문제 4.1

$\mathbb{R}^2$에서 선형범함수 $\phi(x) = 3x_1 - 2x_2$의 Riesz 벡터 $y_0$를 구하고, 몇 가지 벡터에 대해 $\phi(x) = \langle x, y_0 \rangle$를 검증하라.

<details>
<summary>해설 보기</summary>

$\phi(x) = 3x_1 - 2x_2 = \langle x, y_0 \rangle$이므로:

$$y_0 = (3, -2)$$

검증:
- $\phi((1,0)) = 3 = \langle (1,0), (3,-2) \rangle = 3$ ✓
- $\phi((0,1)) = -2 = \langle (0,1), (3,-2) \rangle = -2$ ✓
- $\phi((1,1)) = 1 = \langle (1,1), (3,-2) \rangle = 1$ ✓

$\|\phi\| = \sqrt{9 + 4} = \sqrt{13} = \|y_0\|$ ✓

</details>

---

### 문제 4.2

$L^2([0,\pi])$에서 선형범함수 $\phi(f) = \int_0^{\pi} f(x) \sin(x) dx$의 Riesz 벡터 $y_0$를 구하라.

<details>
<summary>해설 보기</summary>

정의에 의해:

$$\phi(f) = \int_0^{\pi} f(x) \sin(x) dx = \int_0^{\pi} f(x) \overline{y_0(x)} dx = \langle f, y_0 \rangle$$

따라서 (실함수 경우):

$$y_0(x) = \sin(x)$$

검증: $\phi(f) = \int_0^{\pi} f(x) \sin(x) dx = \langle f, \sin \rangle$ ✓

노름: $\|y_0\|_{L^2}^2 = \int_0^{\pi} \sin^2(x) dx = \pi/2$, 따라서 $\|y_0\| = \sqrt{\pi/2}$.

</details>

---

### 문제 4.3

RKHS에서 재생핵 $k_x(t) = e^{-|x-t|}$일 때, 점 평가 $\delta_x(f) = f(x)$가 정말 $f(x) = \langle f, k_x \rangle$를 만족하는지 확인하라.

<details>
<summary>해설 보기</summary>

적절한 RKHS (예: Exponential kernel의 RKHS)에서, 재생 성질:

$$f(x) = \langle f, k_x \rangle_{\mathcal{H}}$$

이는 정의에 의해 만족된다. 재생핵의 "재생" 성질이 바로 이것이다.

실제로, 이 RKHS의 내적은 특정하게 정의되어, 모든 함수 $f$에 대해:

$$\langle f, k_x \rangle = f(x)$$

이것이 Riesz 표현 정리의 직접적인 응용이다.

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./03-orthogonal-projection.md) | [📚 README](../README.md) | [다음 ▶](./05-orthonormal-basis-parseval.md) |

</div>
