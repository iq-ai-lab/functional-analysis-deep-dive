# 4. 변분법 기초 — 함수공간에서의 최적화

## 🎯 핵심 질문

"함수 중에서 가장 작은 에너지를 갖는 함수는 무엇인가?" 고전 미적분에서 $f'(x) = 0$으로 극값을 찾듯이, 함수공간에서도 "가장 좋은 함수"를 찾을 수 있을까요?

## 🔍 왜 이 이론이 AI에서 중요한가

- **물리정보 신경망 (PINN)**: 손실함수 = 변분 문제. "신경망이 PDE를 만족하면서 경계조건도 만족하는 해"는 변분 에너지를 최소화하는 함수와 같습니다.
- **최적 제어**: 시스템을 제어하는 "최선의 방법"을 찾는 것 = 변분 문제
- **머신러닝의 수학적 기초**: 손실함수 최소화 = 변분법. 경사하강법은 변분 문제의 수치 해법입니다.

## 📐 수학적 선행 조건

- **Sobolev 공간**: $W^{k,p}$, $H^k$ 정의
- **함수형식**: 함수를 입력으로 받는 함수
- **약해**: PDE의 약해 개념
- **Fréchet 미분**: 함수공간에서의 미분

## 📖 직관적 이해

### 유한차원에서 무한차원으로

**유한차원 최적화**:
$$\min_{x \in \mathbb{R}^n} f(x)$$

극값 조건: $\nabla f(x) = 0$

**무한차원 (함수공간) 최적화**:
$$\min_{u \in H^1(\Omega)} J[u] = \int_{\Omega} \left(\frac{1}{2}|\nabla u|^2 - f u \right) dx$$

극값 조건: "Gâteaux 미분" $= 0$

### 예제: 최단 경로 문제

두 점 $(0,0)$과 $(1,1)$을 연결하는 곡선 $y(x)$ 중에서 길이가 최소인 것은?

곡선의 길이:
$$J[y] = \int_0^1 \sqrt{1 + y'(x)^2} \, dx$$

극값: $y(x) = x$ (직선)

Euler-Lagrange 방정식을 이용하면 자동으로 얻어집니다!

### 현수선 (Catenary)

중력을 받는 일정한 밀도의 사슬이 만드는 형태 = 변분 문제

위치 에너지: $J[y] = \int \rho g y \sqrt{1 + y'^2} \, dx$

최소화 → Catenary 곡선 $y = \cosh(x)$

## ✏️ 엄밀한 정의

### 범함수 (Functional)

**정의**: 함수공간 $V$ (예: $H^1(\Omega)$)에서 실수로의 대응:
$$J: V \to \mathbb{R}, \quad u \mapsto J[u]$$

**예제**:
1. $J[u] = \int_{\Omega} u \, dx$ (함수의 적분)
2. $J[u] = \int_{\Omega} |\nabla u|^2 \, dx$ (에너지)
3. $J[u] = \max_{x \in \Omega} |u(x)|$ (최대값)

### Gâteaux 미분 (방향 도함수)

**정의**: $J: V \to \mathbb{R}$의 $u \in V$ 방향에서 $h \in V$ 방향의 Gâteaux 미분:

$$\delta J[u; h] = \lim_{t \to 0} \frac{J[u + th] - J[u]}{t}$$

이는 $u$ 방향에서 범함수를 "흔들었을 때" 얼마나 변하는가를 측정합니다.

### 1계 변분 조건

**정의**: $u$가 $J$를 최소화하면:
$$\delta J[u; h] = 0 \quad \forall h \in V$$

즉, 어느 방향으로든 흔들어도 변화가 없어야 합니다.

### Fréchet 미분

**정의**: $J$가 $u$에서 Fréchet 미분가능하면:
$$J[u + h] = J[u] + DJ[u](h) + o(\|h\|)$$

여기서 $DJ[u]: V \to \mathbb{R}$는 선형범함수이고, $o(\|h\|)/\|h\| \to 0$.

## 🔬 정리와 증명

### 정리 1: Euler-Lagrange 방정식 유도

**설정**: 라그랑주 함수 $L(x, u, p) = L(x, u, \nabla u)$에 대해,
$$J[u] = \int_{\Omega} L(x, u(x), \nabla u(x)) \, dx$$

**정리**: $u$가 극값이면 (Euler-Lagrange 방정식):
$$\frac{\partial L}{\partial u} - \nabla \cdot \frac{\partial L}{\partial \nabla u} = 0$$

또는 1D 경우:
$$\frac{\partial L}{\partial u} - \frac{d}{dx} \frac{\partial L}{\partial u'} = 0$$

**증명** (1D):

$u$가 극값이면, 모든 변분 $h \in C_c^{\infty}$에 대해:
$$\delta J[u; h] = 0$$

계산:
$$\delta J[u; h] = \lim_{t \to 0} \frac{1}{t} \left[J[u + th] - J[u]\right]$$

$$= \lim_{t \to 0} \frac{1}{t} \int_a^b [L(u+th, u'+th') - L(u, u')] \, dx$$

Taylor 전개:
$$= \int_a^b \left[\frac{\partial L}{\partial u} h + \frac{\partial L}{\partial u'} h' \right] dx$$

부분적분 (경계항은 $h(a)=h(b)=0$이므로 소거):
$$= \int_a^b \left[\frac{\partial L}{\partial u} - \frac{d}{dx}\frac{\partial L}{\partial u'} \right] h \, dx = 0$$

이것이 모든 $h$에 대해 성립하려면:
$$\frac{\partial L}{\partial u} - \frac{d}{dx}\frac{\partial L}{\partial u'} = 0$$ □

### 정리 2: 직접법 (Direct Method) — 해의 존재성

**설정**: $V$를 Banach 공간 (또는 Hilbert 공간), $J: V \to \mathbb{R} \cup \{\infty\}$ 범함수

**가정**:
1. $J$는 **약 하반연속** (weakly lower semicontinuous): $u_n \rightharpoonup u$ (약수렴) $\Rightarrow$ $J[u] \leq \liminf J[u_n]$
2. $J$는 **강제** (coercive): $\|u\| \to \infty \Rightarrow J[u] \to \infty$

**결론**: $\min_{u \in V} J[u]$가 달성되고, 극소화원이 존재합니다.

**증명**:

**Step 1**: 하한 존재
$$m := \inf_{u \in V} J[u] > -\infty$$

(강제성으로부터 바운드됨)

**Step 2**: 극소화 수열 구성

$m < \infty$이므로, 수열 $\{u_n\}$이 존재하여:
$$J[u_n] \to m \quad \text{as } n \to \infty$$

**Step 3**: 유계성과 약수렴

강제성으로부터:
$$\|u_n\| \leq C \quad \text{(어떤 상수 } C\text{)}$$

Banach 공간의 약수렴 성질에 의해, 부분열 $u_{n_k} \rightharpoonup u \in V$

**Step 4**: 하한연속성으로부터 최소값 달성

약 하한연속성:
$$J[u] \leq \liminf_{k \to \infty} J[u_{n_k}] = m$$

그런데 $J[u] \geq m$ (하한의 정의)이므로:
$$J[u] = m$$ □

### 정리 3: 1차 필요조건과 Euler-Lagrange

**정리**: $J$가 $u$에서 극소화되고, Fréchet 미분가능하면:
$$DJ[u](h) = 0 \quad \forall h \in V$$

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import minimize, LinearConstraint
from scipy.integrate import odeint, solve_bvp
from scipy.sparse import diags
from scipy.sparse.linalg import spsolve

# 변분법의 수치 검증과 응용

print("=" * 70)
print("1. 최단 경로 문제: Brachistochrone (변분법)")
print("=" * 70)

# 문제: 수직면에서 점 (0,0)에서 (L, H)로 중력 하에 떨어지는 
#       최단 시간 곡선은?
# 풀이: Cycloid (사이클로이드)

def arc_length_functional(y, x):
    """곡선의 호장: ∫√(1 + y'²) dx"""
    dy = np.gradient(y, x)
    return np.trapz(np.sqrt(1 + dy**2), x)

# 여러 경로 비교
x = np.linspace(0, 1, 100)

# 경로 1: 직선 y = x
y_straight = x
length_straight = arc_length_functional(y_straight, x)

# 경로 2: 이차 포물선 y = x^2
y_parabola = x**2
length_parabola = arc_length_functional(y_parabola, x)

# 경로 3: 정현파 y = sin(πx/2)
y_sine = np.sin(np.pi * x / 2)
length_sine = arc_length_functional(y_sine, x)

print(f"\n호장 비교 [(0,0)에서 (1,1)로]:")
print(f"  직선:     {length_straight:.6f}")
print(f"  포물선:   {length_parabola:.6f}")
print(f"  정현파:   {length_sine:.6f}")
print(f"  이론값:   {np.sqrt(2):.6f} (최단)")

# Euler-Lagrange로 검증: 직선이 최단
# L = √(1 + y'²)
# ∂L/∂y = 0
# ∂L/∂y' = y'/√(1+y'²)
# d/dx(∂L/∂y') - ∂L/∂y = 0  =>  y'' = 0  => y = ax+b

# 2. 에너지 최소화
print("\n" + "=" * 70)
print("2. 에너지 최소화: Dirichlet 문제")
print("=" * 70)

# 문제: min_u ∫(1/2(u'²) - f·u)dx, u(0)=u(1)=0
# Euler-Lagrange: -u'' = f
# (Poisson 방정식!)

def energy_functional_1d(u, x, f):
    """J[u] = ∫(1/2 u'² - f·u) dx"""
    du = np.gradient(u, x)
    return 0.5 * np.trapz(du**2, x) - np.trapz(f * u, x)

# 유한차분으로 수치 해 계산
def solve_poisson_fd(L, N, f_func):
    """
    -u'' = f on [0,L], u(0)=u(L)=0
    유한차분 방법
    """
    x = np.linspace(0, L, N)
    h = L / (N - 1)
    f_vals = f_func(x)
    
    # 삼중대각 행렬 (경계조건 포함)
    # -u_{i-1} + 2u_i - u_{i+1} = h²f_i
    diag_main = 2 * np.ones(N-2) / h**2
    diag_off = -np.ones(N-3) / h**2
    
    A = diags([diag_off, diag_main, diag_off], [-1, 0, 1], shape=(N-2, N-2))
    b = f_vals[1:-1]
    
    u_interior = spsolve(A.tocsr(), b)
    u = np.concatenate([[0], u_interior, [0]])
    
    return x, u

# 테스트 함수들
def f1(x):
    return np.ones_like(x)  # f = 1

def f2(x):
    return np.sin(np.pi * x)  # f = sin(πx)

# 수치 해
x1, u1 = solve_poisson_fd(1, 100, f1)
x2, u2 = solve_poisson_fd(1, 100, f2)

# 에너지 계산
J1 = energy_functional_1d(u1, x1, f1(x1))
J2 = energy_functional_1d(u2, x2, f2(x2))

print(f"\n경우 1: f(x) = 1")
print(f"  에너지 J[u] = {J1:.8f}")
print(f"  해의 최대값: {np.max(u1):.6f}")

print(f"\n경우 2: f(x) = sin(πx)")
print(f"  에너지 J[u] = {J2:.8f}")
print(f"  해의 최대값: {np.max(u2):.6f}")

# Euler-Lagrange 검증: -u'' = f
du1 = np.gradient(u1, x1)
ddu1 = np.gradient(du1, x1)
residual1 = -ddu1 - f1(x1)
error1_fd = np.sqrt(np.trapz(residual1**2, x1))

print(f"\nEuler-Lagrange 방정식 검증:")
print(f"  ||−u'' − f||_L² = {error1_fd:.2e}")

# 3. 정칙화된 최적화
print("\n" + "=" * 70)
print("3. Tikhonov 정칙화: 변분 관점")
print("=" * 70)

# 노이즈가 있는 데이터에 피팅
np.random.seed(42)
x_data = np.linspace(0, 1, 30)
u_true = np.sin(np.pi * x_data)
u_noisy = u_true + 0.1 * np.random.randn(len(x_data))

# 정칙화 없음
def loss_data(u):
    return np.mean((u - u_noisy)**2)

# 정칙화 포함: J[u] = ||u - u_data||² + λ||u'||²
def loss_regularized(u, x, lam):
    du = np.gradient(u, x)
    dx = x[1] - x[0]
    data_fit = np.mean((u - u_noisy)**2)
    smoothness = lam * np.trapz(du**2, x)
    return data_fit + smoothness

# 다양한 λ에 대해
lambdas = [0, 0.001, 0.01, 0.1, 1.0]
x_fine = np.linspace(0, 1, 100)

results = {}
for lam in lambdas:
    def obj_func(u):
        return loss_regularized(u, x_fine, lam)
    
    # 초기값
    u_init = np.interp(x_fine, x_data, u_noisy)
    
    # 최소화
    from scipy.optimize import minimize
    result = minimize(obj_func, u_init, method='L-BFGS-B')
    results[lam] = result.x

print(f"\nTikhonov 정칙화 (다양한 λ):")
for lam in lambdas:
    u_opt = results[lam]
    loss = loss_regularized(u_opt, x_fine, lam)
    smoothness = np.trapz(np.gradient(u_opt, x_fine)**2, x_fine)
    print(f"  λ = {lam:.4f}: J[u] = {loss:.6f}, 평활도 = {smoothness:.6f}")

# 4. 직접법: Galerkin 근사
print("\n" + "=" * 70)
print("4. 직접법과 Galerkin 근사")
print("=" * 70)

# 문제: min ∫(1/2(u'²) + u² - 2u) dx, u(0)=u(1)=0
# 정확한 해: -u'' + u = 1  =>  u(x) = 1 - cosh(x)/cosh(1)

def energy_galerkin(c, basis_funcs, basis_derivs, x, f):
    """
    u_N = Σ c_i φ_i (Galerkin 근사)
    J[u_N] 계산
    """
    u = np.sum([c[i] * basis_funcs[i](x) for i in range(len(c))], axis=0)
    du = np.sum([c[i] * basis_derivs[i](x) for i in range(len(c))], axis=0)
    
    energy_kin = 0.5 * np.trapz(du**2, x)
    energy_pot = 0.5 * np.trapz(u**2, x)
    work = np.trapz(f * u, x)
    
    return energy_kin + energy_pot - work

# 기저 함수: sin basis (자동으로 경계 조건 만족)
x_galerkin = np.linspace(0, 1, 200)
f_galerkin = np.ones_like(x_galerkin)

def basis_sin(n):
    return lambda x: np.sin(n * np.pi * x)

def basis_sin_deriv(n):
    return lambda x: n * np.pi * np.cos(n * np.pi * x)

# 다양한 basis 개수
n_basis_list = [1, 2, 3, 5, 10]
galerkin_results = {}

for N in n_basis_list:
    basis_funcs = [basis_sin(n) for n in range(1, N+1)]
    basis_derivs = [basis_sin_deriv(n) for n in range(1, N+1)]
    
    def obj(c):
        return energy_galerkin(c, basis_funcs, basis_derivs, x_galerkin, f_galerkin)
    
    from scipy.optimize import minimize
    result = minimize(obj, np.zeros(N), method='BFGS')
    u_approx = np.sum([result.x[i] * basis_funcs[i](x_galerkin) 
                       for i in range(N)], axis=0)
    
    energy = obj(result.x)
    galerkin_results[N] = {'coeffs': result.x, 'energy': energy, 'u': u_approx}
    
    print(f"  N = {N} (basis 개수): J[u_N] = {energy:.8f}")

# 수렴 검증
print(f"\nGalerkin 근사의 수렴:")
for N in n_basis_list:
    print(f"  에너지 감소: {galerkin_results[N]['energy']:.8f}")

# 시각화
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# Plot 1: 호장 비교
ax = axes[0, 0]
ax.plot(x, y_straight, 'r-', linewidth=2, label=f'직선 (L={length_straight:.4f})')
ax.plot(x, y_parabola, 'g-', linewidth=2, label=f'포물선 (L={length_parabola:.4f})')
ax.plot(x, y_sine, 'b-', linewidth=2, label=f'정현파 (L={length_sine:.4f})')
ax.set_xlabel('x')
ax.set_ylabel('y(x)')
ax.set_title('최단 경로: 직선이 최적')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 2: Poisson 방정식 해
ax = axes[0, 1]
ax.plot(x1, u1, 'b-', linewidth=2, label='f(x)=1')
ax.plot(x2, u2, 'r-', linewidth=2, label='f(x)=sin(πx)')
ax.set_xlabel('x')
ax.set_ylabel('u(x)')
ax.set_title('에너지 최소화 (−u\'\'=f, u(0)=u(1)=0)')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 3: 정칙화 효과
ax = axes[1, 0]
ax.scatter(x_data, u_noisy, s=50, alpha=0.6, label='노이즈 데이터')
ax.plot(x_fine, np.sin(np.pi * x_fine), 'k--', linewidth=2, label='참 함수')
for lam in [0, 0.01, 0.1]:
    ax.plot(x_fine, results[lam], label=f'λ={lam}')
ax.set_xlabel('x')
ax.set_ylabel('u(x)')
ax.set_title('Tikhonov 정칙화의 효과')
ax.legend(fontsize=8)
ax.grid(True, alpha=0.3)

# Plot 4: Galerkin 수렴
ax = axes[1, 1]
energies = [galerkin_results[N]['energy'] for N in n_basis_list]
ax.semilogy(n_basis_list, energies, 'bo-', linewidth=2, markersize=8)
ax.set_xlabel('기저 함수 개수 N')
ax.set_ylabel('에너지 J[u_N]')
ax.set_title('Galerkin 근사의 수렴')
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/calculus_variations.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/calculus_variations.png")

# PINN 손실함수와의 연결
print("\n" + "=" * 70)
print("5. PINN과 변분법의 연결")
print("=" * 70)

# PDE: -u'' + u = f, u(0)=u(1)=0
# 약해: a(u,v) = ∫(u'v' + uv)dx = ∫fv dx
# PINN 손실: ||u_θ' + u_θ - f||² + BC 항

def pinn_loss(u, du, x, f):
    """PINN 손실 함수"""
    # PDE 잔차
    ddu = np.gradient(du, x)
    pde_residual = ddu + u - f
    
    # BC 잔차 (경계값)
    bc_residual_0 = u[0]
    bc_residual_1 = u[-1]
    
    return (np.trapz(pde_residual**2, x) + 
            bc_residual_0**2 + bc_residual_1**2)

print(f"\nPINN 손실 = 변분 문제의 에너지")
u_opt = galerkin_results[5]['u']
du_opt = np.gradient(u_opt, x_galerkin)
loss_pinn = pinn_loss(u_opt, du_opt, x_galerkin, f_galerkin)
print(f"  PINN 손실: {loss_pinn:.2e}")

plt.show()
```

## 🔗 AI/ML 연결

### 1. PINN의 손실함수 = 변분 에너지

PDE: $-\Delta u = f$ with BC: $u|_{\partial\Omega} = 0$

**변분 형식**: 
$$J[u] = \frac{1}{2}\int_{\Omega} |\nabla u|^2 \, dx - \int_{\Omega} f u \, dx$$

**PINN 손실**:
$$\mathcal{L}_{\text{PINN}} = \int_{\Omega} (Au_{\theta} - f)^2 \, dx + \int_{\partial\Omega} u_{\theta}^2 \, dx$$

두 식이 **같은 문제를 다르게 표현**한 것입니다!

### 2. 경사하강법 = 변분법의 수치 해법

경사하강법 반복:
$$u^{(k+1)} = u^{(k)} - \alpha \nabla J[u^{(k)}]$$

이는 정확히 **가장 가파른 하강 방향**을 따르는 변분 최소화입니다.

### 3. 정칙화 = Sobolev 노름 제약

손실함수: $\mathcal{L} = \|u_{\theta} - u_{\text{data}}\|^2 + \lambda \|u_{\theta}\|_{H^k}^2$

- $\lambda = 0$: 데이터만 맞춤 (과적합)
- $\lambda > 0$: 매끄러움도 강요 (정칙화)
- 이는 **Tikhonov 정칙화**와 동일

## ⚖️ 가정과 한계

| 가정 | 설명 | 한계 |
|------|------|------|
| $J$ 볼록 | 유일한 극소값 | 비볼록 $J$는 국소 극소값 다수 |
| 약 하반연속 | 극소화 수열 수렴 | 비연속 범함수는 해 없을 수 있음 |
| 강제성 | 해가 유한 에너지 | 무한 에너지 해는 제외 |

## 📌 핵심 정리

1. **범함수**: 함수를 입력으로 받아 실수를 출력하는 함수
   $$J: V \to \mathbb{R}$$

2. **Gâteaux 미분**: 방향 도함수
   $$\delta J[u; h] = \lim_{t \to 0} \frac{J[u+th] - J[u]}{t}$$

3. **극값 조건**: 1차 변분이 0
   $$\delta J[u; h] = 0 \quad \forall h$$

4. **Euler-Lagrange**: 극값의 필요조건
   $$\frac{\partial L}{\partial u} - \nabla \cdot \frac{\partial L}{\partial \nabla u} = 0$$

5. **직접법**: 극소화 수열의 약수렴으로 해 존재
   - 약 하한연속 + 강제성 → 최소값 존재

## 🤔 생각해볼 문제

### 문제 1: Euler-Lagrange 방정식

라그랑주 함수 $L(x, u, p) = \frac{1}{2}p^2 + u^2 - xu$에 대해, Euler-Lagrange 방정식을 유도하세요.

<details>
<summary>💡 풀이 (클릭)</summary>

주어진: $L(x, u, p) = \frac{1}{2}p^2 + u^2 - xu$

계산:
- $\frac{\partial L}{\partial u} = 2u - x$
- $\frac{\partial L}{\partial p} = p$
- $\frac{d}{dx}\left(\frac{\partial L}{\partial p}\right) = \frac{du'}{dx}$

Euler-Lagrange:
$$\frac{\partial L}{\partial u} - \frac{d}{dx}\frac{\partial L}{\partial p} = 0$$

$$2u - x - u'' = 0$$

$$u'' - 2u = -x$$

이는 2계 선형 미분방정식입니다. (비동차 상수계수 ODE)

</details>

### 문제 2: 직접법의 조건

왜 $J[u] \to \infty$ as $\|u\| \to \infty$ (강제성)이 필요할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

**반례** (강제성 없음):

$V = C^0([0,1])$ (연속함수), $J[u] = \int_0^1 (u'^2 - 2u) \, dx$

(여기서 경계조건 무시하고)

$u_n(x) = n$ (상수함수)이면:
- $J[u_n] = 0 - 2n \cdot 1 = -2n \to -\infty$

따라서 극소값이 존재하지 않습니다!

**강제성의 역할**:

$J[u] \geq \alpha \|u\|^p - C$ (어떤 $\alpha > 0, p > 1, C$)

이면 $\|u_n\| \leq M$ (유계) → 약수렴 부분열 존재 → 극소값 존재

</details>

### 문제 3: PINN과 변분법

PINN 손실 $\mathcal{L}_{\text{PINN}} = \|Au_\theta - f\|^2 + \lambda \|u_\theta|_{\partial\Omega}\|^2$이

변분 에너지로 어떻게 표현될까요?

<details>
<summary>💡 풀이 (클릭)</summary>

PDE: $Au = f$ (예: $-\Delta u = f$)

약해:
$$a(u, v) = \int_{\Omega} (∇u \cdot ∇v) \, dx = \int_{\Omega} f v \, dx$$

변분 에너지:
$$J[u] = \frac{1}{2} a(u, u) - \int_{\Omega} f u \, dx = \frac{1}{2}\int_{\Omega} |∇u|^2 \, dx - \int_{\Omega} f u \, dx$$

극값 조건:
$$δJ[u; h] = \int_{\Omega} (∇u \cdot ∇h) \, dx - \int_{\Omega} f h \, dx = 0$$

부분적분:
$$a(u, h) = (f, h) ⟹ Au = f$$

**결론**: 변분 에너지를 최소화 = PDE를 풀기

PINN은 정확히 이 변분 에너지를 신경망으로 최소화하는 것입니다!

</details>

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./03-poincare-sobolev-embedding.md) | [📚 README](../README.md) | [다음 ▶](./05-weak-solution-pde.md) |

</div>
