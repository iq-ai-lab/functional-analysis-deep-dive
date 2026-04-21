# 1. 약미분(Weak Derivative) — 고전 미분의 확장

## 🎯 핵심 질문

함수 $f(x) = |x|$는 $x=0$에서 미분가능하지 않습니다. 하지만 적분 의미에서 이 함수를 "미분"할 수 있을까요? 이것이 가능하다면, 미분불가능한 함수도 편미분방정식(PDE)의 해가 될 수 있다는 뜻입니다. 약미분(weak derivative)이 이를 가능하게 합니다.

## 🔍 왜 이 이론이 AI에서 중요한가

- **PINN (Physics-Informed Neural Networks)**: 신경망으로 PDE를 풀 때, ReLU나 다른 비매끄러운 활성화 함수의 도함수를 약미분으로 정의합니다.
- **분포론(Distribution Theory)**: 델타 함수나 단계 함수의 도함수는 고전 미분이 불가능하지만 약미분으로 정의되며, 신호처리와 PDE에서 핵심 개념입니다.
- **수치 PDE 해석**: 유한요소법(FEM)과 같은 방법은 정확히 약미분을 이용해 PDE 해를 정의합니다.

## 📐 수학적 선행 조건

- **Lebesgue 적분**: 측도론과 L¹, L²공간의 기본 개념
- **배분지지 함수** (Compactly Supported): 유한 영역 밖에서 0인 함수
- **부분적분 공식**: 일변수 함수에 대한 부분적분 이해
- **극한과 수렴**: 함수의 점수렴, 균등수렴, 적분 수렴

## 📖 직관적 이해

### 고전 미분의 한계

**유한차원**: 미분가능한 함수는 대부분의 점에서 "기울기"가 명확합니다.

$$f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$$

하지만 $f(x) = |x|$는 $x=0$에서 이 극한이 존재하지 않습니다:
- 좌미분: $-1$
- 우미분: $+1$

**무한차원**: PDE의 해(약해)는 종종 고전적으로 미분불가능합니다. 예를 들어, 열확산 방정식의 초기값이 불연속이면 해도 초기에 불연속적입니다.

### 적분을 통한 재정의

핵심 아이디어: **미분을 직접 계산하지 말고, 적분 관점에서 정의하자.**

고전 부분적분:
$$\int_a^b f'(x) g(x) \, dx = f(b)g(b) - f(a)g(a) - \int_a^b f(x) g'(x) \, dx$$

경계항이 0이 되는 **테스트 함수** $\phi$를 선택하면:
$$\int_{\Omega} f'(x) \phi(x) \, dx = -\int_{\Omega} f(x) \phi'(x) \, dx$$

이제 우변을 계산하는 것만으로 $f'$의 역할을 하는 함수 $v$를 정의할 수 있습니다:
$$\int_{\Omega} v(x) \phi(x) \, dx = -\int_{\Omega} f(x) \phi'(x) \, dx \quad \forall \phi \in C_c^{\infty}(\Omega)$$

## ✏️ 엄밀한 정의

### 테스트 함수 공간

**정의**: 열린집합 $\Omega \subset \mathbb{R}^n$에 대해,
$$C_c^{\infty}(\Omega) = \{\phi \in C^{\infty}(\Omega) : \text{supp}(\phi) \text{ is compact and } \text{supp}(\phi) \subset \Omega\}$$

여기서 $\text{supp}(\phi) = \overline{\{x : \phi(x) \neq 0\}}$ (지지집합)

**성질**:
- 모든 도함수 $D^{\alpha}\phi$가 연속입니다.
- 경계에서 $\phi = 0$입니다.
- 부분적분이 완벽하게 작동합니다.

### 약미분의 정의

**정의 (Weak Derivative)**: $u \in L^1_{\text{loc}}(\Omega)$ (국소적으로 적분가능)와 다중지수 $\alpha = (\alpha_1, \ldots, \alpha_n)$에 대해, 함수 $v \in L^1_{\text{loc}}(\Omega)$가 $u$의 $\alpha$-차 약도함수(weak partial derivative)라는 것은:

$$\int_{\Omega} v(x) \phi(x) \, dx = (-1)^{|\alpha|} \int_{\Omega} u(x) D^{\alpha}\phi(x) \, dx \quad \forall \phi \in C_c^{\infty}(\Omega)$$

여기서 $|\alpha| = \alpha_1 + \cdots + \alpha_n$.

**기호**: $D^{\alpha}u = v$ 또는 $\partial^{\alpha}u = v$

## 🔬 정리와 증명

### 정리 1: 약미분의 유일성

**정리**: 약도함수가 존재하면, 거의 모든 곳(a.e.)에서 유일합니다.

**증명**:

$v_1$과 $v_2$가 모두 $u$의 $\alpha$-차 약도함수라고 가정하자.

$$\int_{\Omega} v_1(x) \phi(x) \, dx = (-1)^{|\alpha|} \int_{\Omega} u(x) D^{\alpha}\phi(x) \, dx$$

$$\int_{\Omega} v_2(x) \phi(x) \, dx = (-1)^{|\alpha|} \int_{\Omega} u(x) D^{\alpha}\phi(x) \, dx$$

따라서 모든 $\phi \in C_c^{\infty}(\Omega)$에 대해:
$$\int_{\Omega} (v_1(x) - v_2(x)) \phi(x) \, dx = 0$$

임의의 컴팩트 부분집합 $K \subset \Omega$에 대해, 하한의 정의에 의해:

$\psi = (v_1 - v_2)^+ = \max(v_1 - v_2, 0)$라 하면, $\psi$는 거의 모든 곳에서 측정 가능합니다.

$K$ 위에서 $\psi$를 근사하는 $C_c^{\infty}$ 함수열 $\phi_n$을 구성하면:

$$\int_K \psi^2 \, dx = \lim_{n \to \infty} \int_{\Omega} (v_1 - v_2) \phi_n \, dx = 0$$

따라서 $\psi = 0$ a.e. on $K$이고, $K$가 임의이므로 $(v_1 - v_2)^+ = 0$ a.e.

마찬가지로 $(v_1 - v_2)^- = 0$ a.e., 즉 $v_1 = v_2$ a.e. □

### 정리 2: 고전 미분과의 관계

**정리**: $u \in C^1(\Omega)$이면, 고전 도함수 $\partial u/\partial x_i$가 약도함수입니다.

**증명**:

부분적분 공식을 직접 적용:

$$\int_{\Omega} \frac{\partial u}{\partial x_i}(x) \phi(x) \, dx = -\int_{\Omega} u(x) \frac{\partial \phi}{\partial x_i}(x) \, dx$$

이는 약미분의 정의를 만족합니다. □

**주의**: 역은 성립하지 않습니다. 약도함수가 존재해도 고전 미분이 존재할 수 없습니다.

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import quad
from scipy.optimize import fminbound

# 약미분 검증: f(x) = |x|의 약도함수는 sgn(x)

def f(x):
    """f(x) = |x|"""
    return np.abs(x)

def weak_derivative_theory(x):
    """f(x) = |x|의 약도함수: sgn(x)"""
    return np.sign(x)

def test_function(x, a=-1, b=1, smooth=True):
    """테스트 함수 φ ∈ C_c^∞(Ω)"""
    if smooth:
        # 부드러운 범프 함수: exp(-1/(1-x²)) for |x| < 1
        result = np.zeros_like(x, dtype=float)
        mask = np.abs(x) < 1
        result[mask] = np.exp(-1 / (1 - x[mask]**2))
        return result / np.max(result)  # 정규화
    else:
        # 단순 테스트: hat function
        result = np.maximum(1 - np.abs(x), 0)
        return result

def weak_derivative_check(n_points=500, a=-1, b=1):
    """
    약미분의 정의를 수치적으로 검증:
    ∫ v(x)φ(x) dx = -∫ f(x)φ'(x) dx
    """
    x = np.linspace(a, b, n_points)
    dx = (b - a) / (n_points - 1)
    
    # 여러 테스트 함수로 검증
    results = []
    
    for smooth in [True, False]:
        phi = test_function(x, a, b, smooth=smooth)
        
        # φ의 도함수 (유한차분)
        dphi = np.gradient(phi, dx)
        
        # 약미분의 정의 좌변: ∫ v(x)φ(x) dx, v(x) = sgn(x)
        v = weak_derivative_theory(x)
        lhs = np.trapz(v * phi, x)
        
        # 약미분의 정의 우변: -∫ f(x)φ'(x) dx
        rhs = -np.trapz(f(x) * dphi, x)
        
        error = np.abs(lhs - rhs)
        rel_error = error / (np.abs(rhs) + 1e-10)
        
        results.append({
            'type': 'smooth' if smooth else 'piecewise',
            'lhs': lhs,
            'rhs': rhs,
            'error': error,
            'rel_error': rel_error
        })
    
    return results, x, phi, dphi, v

# 검증 실행
results, x, phi, dphi, v = weak_derivative_check()

print("=" * 60)
print("약미분 검증: f(x) = |x|의 약도함수 = sgn(x)")
print("=" * 60)
for result in results:
    print(f"\n테스트 함수 타입: {result['type']}")
    print(f"  좌변 (∫vφ dx):        {result['lhs']:.8f}")
    print(f"  우변 (-∫fφ' dx):      {result['rhs']:.8f}")
    print(f"  절대오차:             {result['error']:.2e}")
    print(f"  상대오차:             {result['rel_error']:.2e}")

# 시각화
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Plot 1: 함수와 고전 미분가능 근사
ax = axes[0, 0]
x_smooth = np.linspace(-1, 1, 500)
ax.plot(x_smooth, f(x_smooth), 'b-', linewidth=2, label='f(x) = |x|')
# 고전 미분가능한 근사
epsilon = 0.05
f_smooth = np.sqrt(x_smooth**2 + epsilon**2)
ax.plot(x_smooth, f_smooth, 'r--', linewidth=2, label=f'√(x² + ε²), ε={epsilon}')
ax.set_xlabel('x')
ax.set_ylabel('f(x)')
ax.set_title('f(x) = |x|와 매끄러운 근사')
ax.grid(True, alpha=0.3)
ax.legend()

# Plot 2: 고전 미분 vs 약미분
ax = axes[0, 1]
x_pos = np.linspace(0.01, 1, 200)
classical_approx = np.sqrt(x_pos**2 + epsilon**2)
classical_deriv = x_pos / np.sqrt(x_pos**2 + epsilon**2)
ax.plot(x_pos, classical_deriv, 'r--', linewidth=2, label='고전 미분 (ε=0.05)')
ax.axhline(y=1, color='b', linestyle='-', linewidth=2, label='약미분 = sgn(x) = 1 (x>0)')
ax.set_xlabel('x')
ax.set_ylabel("f'(x)")
ax.set_title('고전 미분 vs 약미분 (x > 0에서)')
ax.grid(True, alpha=0.3)
ax.legend()
ax.set_ylim([0.5, 1.5])

# Plot 3: 테스트 함수
ax = axes[1, 0]
ax.plot(x, phi, 'g-', linewidth=2, label='φ(x) [테스트 함수]')
ax.plot(x, dphi, 'orange', linewidth=2, label="φ'(x)")
ax.set_xlabel('x')
ax.set_ylabel('φ(x)')
ax.set_title('테스트 함수 φ ∈ C_c^∞')
ax.grid(True, alpha=0.3)
ax.legend()

# Plot 4: 약미분 검증 오차
ax = axes[1, 1]
categories = [r['type'] for r in results]
errors = [r['rel_error'] for r in results]
ax.bar(categories, errors, color=['skyblue', 'lightcoral'])
ax.set_ylabel('상대 오차')
ax.set_title('약미분 정의 검증 오차')
ax.set_yscale('log')
ax.grid(True, alpha=0.3, axis='y')

plt.tight_layout()
plt.savefig('/tmp/weak_derivative.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/weak_derivative.png")

# 추가: Heaviside 함수의 약미분
print("\n" + "=" * 60)
print("Heaviside 함수의 약미분")
print("=" * 60)

def heaviside(x):
    """H(x): x > 0에서 1, x ≤ 0에서 0"""
    return np.where(x > 0, 1.0, 0.0)

def heaviside_weak_derivative(x):
    """H(x)의 약미분 = δ₀ (델타 함수)"""
    # 수치적으로: x=0 주변에 spike
    epsilon = 0.01
    return np.exp(-x**2 / epsilon**2) / (np.sqrt(np.pi) * epsilon)

# Heaviside의 약미분 검증
x_h = np.linspace(-0.5, 0.5, 300)
H = heaviside(x_h)
phi_h = test_function(x_h, -0.5, 0.5, smooth=True)
dphi_h = np.gradient(phi_h, x_h[1] - x_h[0])

# 좌변: ∫ δ₀ · φ dx ≈ φ(0)
delta_approx = heaviside_weak_derivative(x_h)
lhs_h = np.trapz(delta_approx * phi_h, x_h)

# 우변: -∫ H · φ' dx
rhs_h = -np.trapz(H * dphi_h, x_h)

print(f"\nHeaviside H(x)의 약미분 검증:")
print(f"  φ(0) [이상적 좌변]:        {phi_h[np.argmin(np.abs(x_h))]:.8f}")
print(f"  ∫δ₀·φ dx [근사 좌변]:     {lhs_h:.8f}")
print(f"  -∫H·φ' dx [실제 우변]:    {rhs_h:.8f}")
print(f"  오차:                      {np.abs(lhs_h - rhs_h):.2e}")

plt.show()
```

## 🔗 AI/ML 연결

### 1. ReLU와 Subgradient

신경망의 ReLU 활성화 함수:
$$\text{ReLU}(x) = \max(x, 0) = \begin{cases} x & x > 0 \\ 0 & x \leq 0 \end{cases}$$

고전적으로는 $x=0$에서 미분불가능하지만, 약미분은 존재합니다:

$$\frac{d_w}{\text{dx}} \text{ReLU}(x) = \begin{cases} 1 & x > 0 \\ 0 & x < 0 \\ [0,1] & x = 0 \text{ (subgradient)} \end{cases}$$

역전파(backpropagation) 계산할 때, $x=0$에서의 그래디언트는 약미분 또는 부분그래디언트로 정의합니다.

### 2. PINN (Physics-Informed Neural Networks)

PINN에서 PDE의 잔차를 계산할 때:
$$\mathcal{L}_{\text{PDE}} = \left\| \frac{\partial u_{\theta}}{\partial t} + u_{\theta} \frac{\partial u_{\theta}}{\partial x} - \nu \frac{\partial^2 u_{\theta}}{\partial x^2} \right\|^2$$

신경망 $u_{\theta}$의 도함수들 ($\partial u_{\theta}/\partial x$ 등)은 자동 미분으로 계산되는데, 이는 정확히 약미분을 계산하는 것과 같습니다.

### 3. 비매끄러운 함수의 학습

데이터가 불연속이거나 미분불가능한 점을 가질 때:
- 고전 미분으로 정의된 손실함수는 작동하지 않습니다.
- 약미분을 이용하면 비매끄러운 함수도 PDE 제약조건으로 정의할 수 있습니다.
- 예: 충격파(shock)를 가진 쌍곡선 PDE의 약해를 신경망으로 학습합니다.

## ⚖️ 가정과 한계

| 가정 | 설명 | 한계 |
|------|------|------|
| $\Omega$는 열린집합 | 경계에서의 동작을 피함 | 경계 근처에서 약미분이 존재하지 않을 수 있음 |
| $u \in L^1_{\text{loc}}(\Omega)$ | 국소적 적분가능성 | 분포(distribution)는 함수가 아님 |
| 약미분은 a.e. 유일 | 측도 0인 집합에서 수정 가능 | 점 하나에서의 값은 의미 없음 |
| 테스트 함수는 무한번 미분가능 | 부분적분이 완벽하게 작동 | 실제 함수는 매끄럽지 않음 |

## 📌 핵심 정리

1. **약미분**: 적분 정의를 통해 미분불가능한 함수도 "미분"할 수 있습니다.
   - 정의: $\int_{\Omega} v \phi \, dx = (-1)^{|\alpha|} \int_{\Omega} u D^{\alpha}\phi \, dx$ for all $\phi \in C_c^{\infty}$

2. **유일성**: 약도함수는 거의 모든 곳에서 유일합니다.

3. **일반화**: 고전 미분 ⊂ 약미분 (고전 미분이 존재하면 약미분도 존재)

4. **응용**: ReLU, 불연속 함수, PDE의 약해 등을 다룰 수 있습니다.

## 🤔 생각해볼 문제

### 문제 1: 약미분의 존재성

$\Omega = (-1, 1)$에서 $u(x) = \begin{cases} 0 & x \leq 0 \\ x^2 & x > 0 \end{cases}$의 1계 약도함수 $D^1 u$를 구하세요.

<details>
<summary>💡 풀이 (클릭)</summary>

부분적분 공식을 사용합니다:
$$\int_{-1}^{1} (D^1 u)(x) \phi(x) \, dx = -\int_{-1}^{1} u(x) \phi'(x) \, dx$$

우변을 계산:
$$-\int_{-1}^{1} u(x) \phi'(x) \, dx = -\int_{-1}^{0} 0 \cdot \phi'(x) \, dx - \int_{0}^{1} x^2 \phi'(x) \, dx$$

$\phi \in C_c^{\infty}$이므로, 부분적분을 $(0, 1)$에서 적용:
$$= -\left[ x^2 \phi(x) \right]_0^1 + \int_0^1 2x \phi(x) \, dx = 0 + \int_0^1 2x \phi(x) \, dx$$

($\phi(1) = 0$ 경계 조건 사용)

따라서 약도함수는:
$$D^1 u = \begin{cases} 0 & -1 < x < 0 \\ 2x & 0 < x < 1 \end{cases}$$

**주의**: $x=0$에서의 값은 중요하지 않습니다. (거의 모든 곳에서의 정의)

</details>

### 문제 2: 고전 미분 vs 약미분

다음 함수 $f(x) = |x|$에 대해, 2계 약미분 $D^2 f$를 구하세요. (1계 약미분은 $\text{sgn}(x)$)

<details>
<summary>💡 풀이 (클릭)</summary>

1계 약미분부터:
$$D^1 f = v_1(x) = \text{sgn}(x)$$

2계 약미분:
$$\int_{-1}^{1} (D^2 f)(x) \phi(x) \, dx = -\int_{-1}^{1} v_1(x) \phi'(x) \, dx$$

$$= -\int_{-1}^{0} (-1) \phi'(x) \, dx - \int_{0}^{1} (1) \phi'(x) \, dx$$

$$= \int_{-1}^{0} \phi'(x) \, dx - \int_{0}^{1} \phi'(x) \, dx$$

$$= [\phi(0) - \phi(-1)] - [\phi(1) - \phi(0)] = 2\phi(0)$$

따라서 $D^2 f = 2\delta_0$ (델타 함수의 2배)

이는 분포 의미에서의 약미분이고, 함수가 아닙니다!

</details>

### 문제 3: PINN과 약미분

신경망 $u_\theta(x) = \theta_1 x + \theta_2 |x|$에서, 자동 미분으로 계산한 $\frac{du_\theta}{dx}|_{x=0}$은 어떻게 정의되어야 할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

$u_\theta(x) = \theta_1 x + \theta_2 |x|$의 약미분:

$$\frac{d u_\theta}{dx} = \theta_1 + \theta_2 \cdot \text{sgn}(x)$$

$x=0$에서:
- 좌미분: $\theta_1 - \theta_2$
- 우미분: $\theta_1 + \theta_2$
- 부분그래디언트(subgradient): $[\theta_1 - \theta_2, \theta_1 + \theta_2]$

자동 미분 프레임워크(PyTorch, TensorFlow)는 보통:
1. $x \neq 0$에서는 약미분 값을 반환
2. $x = 0$에서는 임의의 부분그래디언트 값 선택 (보통 0 또는 1)

PINN에서 PDE 손실을 계산할 때, 이러한 미분불가능한 점에서의 근사는 수렴성에 영향을 줄 수 있으므로 주의가 필요합니다.

</details>

<div align="center">

| | | |
|---|---|---|
| | [📚 README](../README.md) | [다음 ▶](./02-sobolev-spaces.md) |

</div>
