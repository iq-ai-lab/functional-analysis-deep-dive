# 2. Sobolev 공간 $W^{k,p}(\Omega)$ — 약미분이 있는 함수들의 Banach 공간

## 🎯 핵심 질문

약미분을 가진 함수들의 집합은 어떤 구조를 가질까요? 이들 함수의 "크기"를 어떻게 측정할까요? 특히, 도함수까지 포함한 노름을 정의하면 완비성(completeness)을 얻을 수 있을까요?

## 🔍 왜 이 이론이 AI에서 중요한가

- **함수공간의 구조**: PINN과 신경망이 "함수"를 학습할 때, 그 함수가 살고 있는 Sobolev 공간의 성질이 수렴성과 오차를 결정합니다.
- **정칙화(Regularization)**: L² 손실에 도함수 항을 더하면 ($\|u\|_p^2 + \lambda \|Du\|_p^2$), Sobolev 노름을 최소화하는 것입니다.
- **재생핵 Hilbert 공간(RKHS)**: Sobolev 공간 $H^k$는 특정 재생핵을 가진 RKHS입니다 (Ch. 5 연결).

## 📐 수학적 선행 조건

- **Lebesgue 공간**: $L^p(\Omega)$ 정의와 성질, 내적공간
- **약미분**: 정의와 유일성 (Chapter 1)
- **Banach/Hilbert 공간**: 완비성, 노름, 내적
- **측도론**: 거의 모든 곳(a.e.) 개념

## 📖 직관적 이해

### 함수공간의 계층

**유한차원**: 벡터 $(x_1, \ldots, x_n)$의 노름: $\|x\|_p = (\sum |x_i|^p)^{1/p}$

**무한차원**: 함수 $u(x)$의 노름을 정의하려면 "얼마나 크거나 진동하는가"를 측정해야 합니다.

- **$L^p$ 노름**: 함수의 크기만 측정. 미분가능하지 않은 함수도 포함.
  $$\|u\|_{L^p} = \left( \int_{\Omega} |u(x)|^p \, dx \right)^{1/p}$$

- **Sobolev 노름**: 함수와 그 도함수의 크기를 함께 측정. 매끄러움도 반영.
  $$\|u\|_{W^{k,p}} = \left( \sum_{|\alpha| \leq k} \int_{\Omega} |D^{\alpha}u(x)|^p \, dx \right)^{1/p}$$

### 예제: 1D에서 $W^{1,2}([0,1])$

**좋은 예제**:
- $u(x) = x$: $\|u\|_2^2 = \int_0^1 x^2 \, dx = 1/3$, $\|u'\|_2^2 = 1$
- $u(x) = \sin(\pi x)$: 부드럽고 미분가능

**나쁜 예제** (Sobolev 공간에 없음):
- $u(x) = |x|$: $\|u\|_2 < \infty$이지만 $\|u'\|_2 = \infty$ (약미분이 $L^2$에 없음)
  
실제로: $u(x) = |x|$는 $L^2([0,1])$에는 있지만 $W^{1,2}([0,1])$에는 없습니다.

## ✏️ 엄밀한 정의

### Sobolev 공간의 정의

**정의**: $1 \leq p \leq \infty$, 양의 정수 $k$에 대해,
$$W^{k,p}(\Omega) = \{ u \in L^p(\Omega) : D^{\alpha}u \in L^p(\Omega) \text{ for all } |\alpha| \leq k \}$$

여기서 $D^{\alpha}u$는 약미분입니다.

### Sobolev 노름

$$\|u\|_{W^{k,p}(\Omega)} = \begin{cases}
\displaystyle \left( \sum_{|\alpha| \leq k} \int_{\Omega} |D^{\alpha}u|^p \, dx \right)^{1/p} & p < \infty \\[0.5cm]
\displaystyle \sum_{|\alpha| \leq k} \|D^{\alpha}u\|_{L^{\infty}} & p = \infty
\end{cases}$$

**세미노름** (치우친 노름):
$$|u|_{W^{k,p}(\Omega)} = \left( \sum_{|\alpha|=k} \int_{\Omega} |D^{\alpha}u|^p \, dx \right)^{1/p}$$

(최고차 도함수만 포함)

### Hilbert 공간 $H^k(\Omega)$

**정의**: $H^k(\Omega) = W^{k,2}(\Omega)$

내적:
$$\langle u, v \rangle_{H^k(\Omega)} = \sum_{|\alpha| \leq k} \int_{\Omega} D^{\alpha}u(x) \overline{D^{\alpha}v(x)} \, dx$$

대응하는 노름:
$$\|u\|_{H^k}^2 = \sum_{|\alpha| \leq k} \int_{\Omega} |D^{\alpha}u|^2 \, dx$$

### 0-경계 Sobolev 공간

**정의**: 
$$W^{k,p}_0(\Omega) = \overline{C_c^{\infty}(\Omega)}^{\|\cdot\|_{W^{k,p}}}$$

즉, $C_c^{\infty}(\Omega)$ (컴팩트 지지 함수)의 Sobolev 노름에 대한 완비화입니다.

**직관**: 경계에서 0인 함수들과 그들의 도함수들이 $L^p$에 있는 함수들입니다.

## 🔬 정리와 증명

### 정리 1: $W^{k,p}$는 Banach 공간

**정리**: $(W^{k,p}(\Omega), \|\cdot\|_{W^{k,p}})$는 Banach 공간(완비 노름공간)입니다.

**증명**:

**1단계**: 노름 공리 확인
- 양성(Positivity): $\|u\| \geq 0$, 등호 $\Leftrightarrow$ $u = 0$ a.e. ✓
- 동차성(Homogeneity): $\|\lambda u\| = |\lambda| \|u\|$ ✓
- 삼각부등식: Minkowski 부등식으로부터 ✓

**2단계**: Cauchy 수열의 수렴성 검증

$\{u_n\} \subset W^{k,p}$가 Cauchy 수열이라 하자:
$$\|u_n - u_m\|_{W^{k,p}} \to 0 \text{ as } n,m \to \infty$$

각 다중지수 $|\alpha| \leq k$에 대해:
$$\|D^{\alpha}u_n - D^{\alpha}u_m\|_{L^p} \to 0$$

**2-1**: $L^p$의 완비성에 의해, 각 $|\alpha|$에 대해 $v_{\alpha} \in L^p$가 존재하여:
$$D^{\alpha}u_n \to v_{\alpha} \text{ in } L^p$$

**2-2**: $\alpha = 0$인 경우:
$$u_n \to v_0 =: u \text{ in } L^p$$

**2-3**: 약미분의 관점에서, $v_{\alpha}$가 정말 $D^{\alpha}u$인지 확인해야 합니다.

모든 $\phi \in C_c^{\infty}(\Omega)$에 대해:
$$\int_{\Omega} (D^{\alpha}u_n)(x) \phi(x) \, dx = (-1)^{|\alpha|} \int_{\Omega} u_n(x) D^{\alpha}\phi(x) \, dx$$

극한을 취하면 ($n \to \infty$):
$$\int_{\Omega} v_{\alpha}(x) \phi(x) \, dx = (-1)^{|\alpha|} \int_{\Omega} u(x) D^{\alpha}\phi(x) \, dx$$

따라서 $v_{\alpha} = D^{\alpha}u$ (약미분의 정의).

**2-4**: $v_{\alpha} \in L^p$ for all $|\alpha| \leq k$이므로 $u \in W^{k,p}$.

**2-5**: 
$$\|u_n - u\|_{W^{k,p}} = \left(\sum_{|\alpha| \leq k} \|D^{\alpha}u_n - D^{\alpha}u\|_{L^p}^p\right)^{1/p} \to 0$$

따라서 $u_n \to u$ in $W^{k,p}$. □

### 정리 2: $H^k$ 위계

**정리**: $s > t \geq 0$이면, $H^s(\Omega) \subset H^t(\Omega)$ (연속 포함).

**증명**:

$u \in H^s(\Omega)$이면, $|\alpha| \leq t < s$인 모든 $\alpha$에 대해:
$$\int_{\Omega} |D^{\alpha}u|^2 \, dx < \infty$$

따라서 $u \in H^t(\Omega)$.

연속성: $\|u\|_{H^t} \leq C_s \|u\|_{H^s}$ (정수 $s, t$에 대해). □

### 정리 3: $H^k$는 Hilbert 공간

**정리**: $(H^k(\Omega), \langle \cdot, \cdot \rangle_{H^k})$는 Hilbert 공간입니다.

**증명**:

내적:
$$\langle u, v \rangle_{H^k} = \sum_{|\alpha| \leq k} \int_{\Omega} D^{\alpha}u \cdot D^{\alpha}v \, dx$$

유도되는 노름: $\|u\|_{H^k}^2 = \langle u, u \rangle_{H^k}$

Schwarz 부등식과 Minkowski 부등식로부터 내적공간의 공리 확인.

$L^2$의 완비성 (정리 1)로부터 Hilbert 공간. □

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import quad
from scipy.interpolate import CubicSpline
from scipy.linalg import solve

# Sobolev 공간의 함수 근사와 노름 계산

def sobolev_norm_1d(u, du, x, p=2):
    """
    1D에서 W^{1,p} 노름 계산
    Args:
        u: 함수값 배열
        du: 도함수값 배열
        x: 격자점
        p: p-노름 (기본값 2)
    Returns:
        W^{1,p} 노름
    """
    dx = np.diff(x)
    if p == 2:
        # L^2 내의 || u ||^2 + || u' ||^2
        norm_u = np.sqrt(np.trapz(u**2, x))
        norm_du = np.sqrt(np.trapz(du**2, x))
        return np.sqrt(norm_u**2 + norm_du**2)
    else:
        # 일반적인 p-노름
        term1 = (np.trapz(np.abs(u)**p, x))**(1/p)
        term2 = (np.trapz(np.abs(du)**p, x))**(1/p)
        return (term1**p + term2**p)**(1/p)

def sobolev_seminorm_1d(du, x, p=2):
    """W^{1,p} 세미노름: ||u'||_p만 포함"""
    if p == 2:
        return np.sqrt(np.trapz(du**2, x))
    else:
        return (np.trapz(np.abs(du)**p, x))**(1/p)

def finite_difference_derivative(u, h):
    """중심 차분으로 도함수 근사"""
    du = np.zeros_like(u)
    du[1:-1] = (u[2:] - u[:-2]) / (2*h)
    # 경계: forward/backward 차분
    du[0] = (u[1] - u[0]) / h
    du[-1] = (u[-1] - u[-2]) / h
    return du

# 테스트 함수들
x = np.linspace(0, 1, 400)
h = x[1] - x[0]

# 함수 1: 부드러운 함수 u = sin(πx)
u1 = np.sin(np.pi * x)
du1_exact = np.pi * np.cos(np.pi * x)
du1_fd = finite_difference_derivative(u1, h)

# 함수 2: 덜 부드러운 함수 u = x(1-x)
u2 = x * (1 - x)
du2_exact = 1 - 2*x
du2_fd = finite_difference_derivative(u2, h)

# 함수 3: 매우 진동하는 함수 u = sin(4πx)
u3 = np.sin(4 * np.pi * x)
du3_exact = 4 * np.pi * np.cos(4 * np.pi * x)
du3_fd = finite_difference_derivative(u3, h)

# Sobolev 노름 계산
funcs = [
    (u1, du1_exact, "sin(πx)"),
    (u2, du2_exact, "x(1-x)"),
    (u3, du3_exact, "sin(4πx)")
]

print("=" * 70)
print("Sobolev 공간 W^{1,2} 노름 계산")
print("=" * 70)

norms_dict = {}
for u, du, name in funcs:
    norm_w12 = sobolev_norm_1d(u, du, x, p=2)
    norm_l2 = np.sqrt(np.trapz(u**2, x))
    seminorm_h1 = sobolev_seminorm_1d(du, x, p=2)
    
    norms_dict[name] = {
        'L2': norm_l2,
        'seminorm': seminorm_h1,
        'W1,2': norm_w12
    }
    
    print(f"\n함수: {name}")
    print(f"  ||u||_L² =                {norm_l2:.6f}")
    print(f"  |u|_{'{H¹'} (세미노름) =    {seminorm_h1:.6f}")
    print(f"  ||u||_{{H¹}} (Sobolev) =  {norm_w12:.6f}")

# H^1 위계 검증
print("\n" + "=" * 70)
print("Hilbert 공간 내적과 노름")
print("=" * 70)

# H^1 내적: <u,v>_H1 = <u,v>_L2 + <u',v'>_L2
u1_vals = np.sin(2 * np.pi * x)
u2_vals = np.cos(2 * np.pi * x)
du1 = finite_difference_derivative(u1_vals, h)
du2 = finite_difference_derivative(u2_vals, h)

inner_product_l2 = np.trapz(u1_vals * u2_vals, x)
inner_product_h1deriv = np.trapz(du1 * du2, x)
inner_product_h1 = inner_product_l2 + inner_product_h1deriv

# 노름으로부터 재구성
norm_u1_h1 = np.sqrt(
    np.trapz(u1_vals**2, x) + np.trapz(du1**2, x)
)
norm_u2_h1 = np.sqrt(
    np.trapz(u2_vals**2, x) + np.trapz(du2**2, x)
)

# 삼각부등식 검증
u_sum = u1_vals + u2_vals
du_sum = du1 + du2
norm_sum = np.sqrt(
    np.trapz(u_sum**2, x) + np.trapz(du_sum**2, x)
)

print(f"\nu₁ = sin(2πx), u₂ = cos(2πx)")
print(f"\n<u₁, u₂>_H¹ =           {inner_product_h1:.6f}")
print(f"||u₁||_H¹ =             {norm_u1_h1:.6f}")
print(f"||u₂||_H¹ =             {norm_u2_h1:.6f}")
print(f"||u₁ + u₂||_H¹ =        {norm_sum:.6f}")
print(f"삼각부등식 검증:")
print(f"  ||u₁ + u₂||_H¹ ≤ ||u₁||_H¹ + ||u₂||_H¹")
print(f"  {norm_sum:.6f} ≤ {norm_u1_h1 + norm_u2_h1:.6f} ✓")

# Sobolev 공간 포함 관계
print("\n" + "=" * 70)
print("Sobolev 공간 포함 관계: H^s ⊂ H^t (s > t)")
print("=" * 70)

# 더 고차 도함수 포함
x_test = np.linspace(0, 1, 300)
h_test = x_test[1] - x_test[0]

u_test = np.sin(2 * np.pi * x_test)
du_test = 2 * np.pi * np.cos(2 * np.pi * x_test)
ddu_test = -(2 * np.pi)**2 * np.sin(2 * np.pi * x_test)

# 수치 도함수
du_fd = finite_difference_derivative(u_test, h_test)
ddu_fd = finite_difference_derivative(du_fd, h_test)

# 노름들
norm_h0 = np.sqrt(np.trapz(u_test**2, x_test))
norm_h1 = np.sqrt(
    np.trapz(u_test**2, x_test) + np.trapz(du_test**2, x_test)
)
norm_h2 = np.sqrt(
    np.trapz(u_test**2, x_test) + 
    np.trapz(du_test**2, x_test) + 
    np.trapz(ddu_test**2, x_test)
)

print(f"\nu = sin(2πx)에 대해:")
print(f"  ||u||_L² = ||u||_H⁰ =  {norm_h0:.6f}")
print(f"  ||u||_H¹ =             {norm_h1:.6f}")
print(f"  ||u||_H² =             {norm_h2:.6f}")
print(f"\n포함 관계: H² ⊂ H¹ ⊂ L² (또는 H⁰)")
print(f"  ||u||_H² ≥ ||u||_H¹ ≥ ||u||_L²")
print(f"  {norm_h2:.4f} ≥ {norm_h1:.4f} ≥ {norm_h0:.4f} ✓")

# 시각화
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# Plot 1: 함수들과 도함수
ax = axes[0, 0]
for u, du, name in funcs:
    ax.plot(x, u, label=name, alpha=0.7)
ax.set_xlabel('x')
ax.set_ylabel('u(x)')
ax.set_title('테스트 함수들')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 2: 정확한 도함수와 수치 도함수 비교
ax = axes[0, 1]
ax.plot(x, du1_exact, 'b-', linewidth=2, label="d/dx sin(πx) [정확]")
ax.plot(x, du1_fd, 'r--', linewidth=1.5, label="유한차분", alpha=0.7)
ax.set_xlabel('x')
ax.set_ylabel("u'(x)")
ax.set_title('도함수 근사 오차')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 3: Sobolev 노름 비교
ax = axes[1, 0]
names = list(norms_dict.keys())
norms_l2 = [norms_dict[n]['L2'] for n in names]
norms_h1 = [norms_dict[n]['W1,2'] for n in names]
x_pos = np.arange(len(names))
width = 0.35
ax.bar(x_pos - width/2, norms_l2, width, label='||u||_L²', alpha=0.8)
ax.bar(x_pos + width/2, norms_h1, width, label='||u||_H¹', alpha=0.8)
ax.set_ylabel('노름')
ax.set_title('L² vs H¹ 노름')
ax.set_xticks(x_pos)
ax.set_xticklabels(names, rotation=15)
ax.legend()
ax.grid(True, alpha=0.3, axis='y')

# Plot 4: 에너지 기여도
ax = axes[1, 1]
energy_u = [norms_dict[n]['L2']**2 for n in names]
energy_du = [norms_dict[n]['seminorm']**2 for n in names]
x_pos = np.arange(len(names))
ax.bar(x_pos, energy_u, label='||u||²_L²', alpha=0.8)
ax.bar(x_pos, energy_du, bottom=energy_u, label="|u'|²_L²", alpha=0.8)
ax.set_ylabel('에너지')
ax.set_title('H¹ 에너지 분해')
ax.set_xticks(x_pos)
ax.set_xticklabels(names, rotation=15)
ax.legend()
ax.grid(True, alpha=0.3, axis='y')

plt.tight_layout()
plt.savefig('/tmp/sobolev_spaces.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/sobolev_spaces.png")

# Sobolev 공간 근사: Galerkin 법
print("\n" + "=" * 70)
print("Galerkin 근사와 Sobolev 공간")
print("=" * 70)

# 예제: u = sin(πx)를 다항식으로 근사 in W^{1,2}
x_fine = np.linspace(0, 1, 500)
u_target = np.sin(np.pi * x_fine)
du_target = np.pi * np.cos(np.pi * x_fine)

# 저차 다항식으로 근사
from numpy.polynomial import polynomial as P

# Legendre 다항식 (직교 기저)
poly_order = 5
coeffs = np.polyfit(x_fine, u_target, poly_order)
u_approx = np.polyval(coeffs, x_fine)
du_approx = np.polyval(np.polyder(coeffs), x_fine)

error_l2 = np.sqrt(np.trapz((u_target - u_approx)**2, x_fine))
error_h1 = np.sqrt(
    np.trapz((u_target - u_approx)**2, x_fine) +
    np.trapz((du_target - du_approx)**2, x_fine)
)

print(f"\nsin(πx)를 {poly_order}차 다항식으로 근사:")
print(f"  L² 오차:  {error_l2:.2e}")
print(f"  H¹ 오차:  {error_h1:.2e}")

plt.figure(figsize=(12, 4))

plt.subplot(1, 2, 1)
plt.plot(x_fine, u_target, 'b-', linewidth=2, label='sin(πx) [정확]')
plt.plot(x_fine, u_approx, 'r--', linewidth=2, label=f'다항식 근사 (deg={poly_order})')
plt.xlabel('x')
plt.ylabel('u(x)')
plt.title('L² 근사')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(1, 2, 2)
plt.plot(x_fine, du_target, 'b-', linewidth=2, label="d/dx sin(πx) [정확]")
plt.plot(x_fine, du_approx, 'r--', linewidth=2, label="근사의 도함수")
plt.xlabel('x')
plt.ylabel("u'(x)")
plt.title('H¹ (도함수) 근사')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/sobolev_approximation.png', dpi=150, bbox_inches='tight')
print("그래프 저장: /tmp/sobolev_approximation.png")

plt.show()
```

## 🔗 AI/ML 연결

### 1. Neural Network와 Sobolev 공간

신경망 $u_\theta: \Omega \to \mathbb{R}$를 생각할 때:
- **입력**: $x \in \Omega$
- **출력**: $u_\theta(x) \approx u_{\text{target}}(x)$
- **목표**: 신경망이 Sobolev 공간의 함수로 수렴하기를 원함

신경망의 도함수(자동 미분):
$$\frac{\partial u_\theta}{\partial x_i} = \sum_{j} \theta_{ij} \sigma'(w_j^T x + b_j)$$

이들이 $L^p$에 있어야 신경망이 $W^{1,p}$에 있습니다.

### 2. SIREN (Sine-Activated Residual Networks)

SIREN은 정현파 활성화를 사용하여 높은 주파수 성분을 학습할 수 있고, 자동으로 높은 Sobolev 정규성을 달성합니다.

손실함수: $\mathcal{L} = \|u_\theta - u_{\text{data}}\|_{L^2}^2 + \lambda \|\nabla u_\theta\|_{L^2}^2$

이는 정확히 $H^1$ 노름을 최소화하는 것입니다!

### 3. Sobolev 공간 기반 오차 추정

유한요소법과 신경망 근사의 오차는 Sobolev 노름으로 측정됩니다:
$$\|u - u_h\|_{H^1} \leq C h^{k-1} |u|_{H^k}$$

여기서 $|u|_{H^k}$는 Sobolev 세미노름(최고차 도함수만).

## ⚖️ 가정과 한계

| 가정 | 설명 | 한계 |
|------|------|------|
| $\Omega$는 유계 | 컴팩트 포함으로 좋은 성질 | 무한 영역에서는 Sobolev 매몰 약화 |
| $u \in L^p(\Omega)$부터 시작 | 기저 함수공간 필요 | 무한히 큰 함수는 제외 |
| $p \geq 1$ | Minkowski 부등식 성립 | $0 < p < 1$에서는 비볼록 |
| 약미분의 유일성 | a.e. 같은 함수는 동일 | 점 단위에서의 값은 의미 없음 |

## 📌 핵심 정리

1. **Sobolev 공간**: 약미분을 가진 $L^p$ 함수들의 집합
   $$W^{k,p}(\Omega) = \{u \in L^p : D^{\alpha}u \in L^p, |\alpha| \leq k\}$$

2. **완비성**: $(W^{k,p}, \|\cdot\|_{W^{k,p}})$는 Banach 공간

3. **Hilbert 구조**: $H^k = W^{k,2}$는 내적으로 Hilbert 공간

4. **위계 관계**: $H^s \subset H^t$ ($s > t$), $C^k \subset H^k$

5. **응용**: 정규화, PINN 수렴성, 유한요소법 오차 추정

## 🤔 생각해볼 문제

### 문제 1: Sobolev 공간 포함

$L^1([0,1]) \cap L^2([0,1])$의 함수가 항상 $W^{1,2}([0,1])$에 속할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

**아니오.** 반례:

$$u(x) = \begin{cases} 1/\sqrt{x} & 0 < x \leq 1 \\ 0 & x = 0 \end{cases}$$

- $\int_0^1 |u(x)| \, dx = \int_0^1 \frac{1}{\sqrt{x}} \, dx = 2$ (수렴!)
- $\int_0^1 u(x)^2 \, dx = \int_0^1 \frac{1}{x} \, dx = \infty$ (발산)

따라서 $u \in L^1$이지만 $u \notin L^2$.

더 조정해서: $u(x) = \log(x)$ on $(0, 1]$이면,
- $\int_0^1 \log^2(x) \, dx < \infty$ (수렴, 부분적분)
- 하지만 $\int_0^1 |\log(x)| \, dx = 1$ (수렴)
- 약미분: $u'(x) = 1/x \in L^2_{\text{loc}}$이지만 $L^2$에 전체에서 있나요?

실제로 $L^1 \cap L^2 \not\subset W^{1,2}$입니다. Sobolev 공간은 **도함수 조건**을 필요합니다.

</details>

### 문제 2: Poincaré 부등식

유계 영역 $\Omega$에서 $u \in W^{1,2}_0(\Omega)$ (경계에서 0)이면:
$$\|u\|_{L^2} \leq C \|\nabla u\|_{L^2}$$

이것이 왜 중요할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

Poincaré 부등식은 $W^{1,2}_0$에서 **|| ∇u ||_{L²}가 동치 노름**이 됨을 의미합니다.

$$\|u\|_{W^{1,2}} = \left(\|u\|_{L^2}^2 + \|\nabla u\|_{L^2}^2\right)^{1/2} \sim \|\nabla u\|_{L^2}$$

**중요성**:
1. **수치 해석**: 경계값 문제를 풀 때, $\|\nabla u\|$만 제어하면 전체 $W^{1,2}$ 노름이 제어됨
2. **PINN**: 경계 조건을 통합하면, 내부 잔차만 최소화해도 충분
3. **유한요소법**: Lax-Milgram 정리의 coercive 조건 = Poincaré 부등식

</details>

### 문제 3: 신경망의 Sobolev 정규성

ReLU 신경망 $u_\theta(x) = \sum_i \theta_i \text{ReLU}(w_i x + b_i)$가 $H^1(\mathbb{R})$에 속할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

**정답**: 아니오 (일반적으로).

ReLU는 $x=0$에서 미분불가능하므로:
- $u_\theta \in L^2$ (연속이므로 로컬 적분가능)
- $u'_\theta = \sum_i \theta_i w_i \text{ReLU}'(w_i x + b_i)$는 점프 불연속
- 약미분은 존재하지만: $u'_\theta \in L^2_{\text{loc}}$일 필요충분조건은?

실제로 ReLU 신경망의 도함수는 **piecewise constant** (조각별 상수)이므로:

$$\int_{\mathbb{R}} |u'_\theta|^2 \, dx < \infty$$

따라서 $u_\theta \in H^1_{\text{loc}}(\mathbb{R})$이지만, 전체 $\mathbb{R}$에서 $H^1$이 되려면 **컴팩트 지지** 또는 충분히 빠른 감소가 필요합니다.

**개선**: SIREN은 정현파를 사용하여 자동으로 높은 정규성을 달성합니다.

</details>

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./01-weak-derivative.md) | [📚 README](../README.md) | [다음 ▶](./03-poincare-sobolev-embedding.md) |

</div>
