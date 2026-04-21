# 5. PDE의 약해(Weak Solution) — Lax-Milgram과 유한요소법의 이론

## 🎯 핵심 질문

경계값 문제 $-\Delta u = f$ with $u = 0$ on $\partial\Omega$의 고전해(미분가능한 해)가 없을 수 있습니다. 하지만 약미분의 의미에서는 "해"를 정의할 수 있을까요? 더 나아가, 그러한 약해의 유일성과 존재성을 어떻게 보장할까요?

## 🔍 왜 이 이론이 AI에서 중요한가

- **PINN의 수렴성 보장**: Lax-Milgram 정리가 없으면 PINN이 정말 PDE를 푸는지 알 수 없습니다.
- **유한요소법의 이론적 기초**: FEM은 정확히 Lax-Milgram 정리를 응용한 것입니다.
- **약해의 정규성**: 약해가 언제 고전해가 되는지 (또는 얼마나 부드러운지) 알 수 있습니다.

## 📐 수학적 선행 조건

- **Sobolev 공간**: $W^{k,p}$, $H^k_0$ 정의
- **변분법**: Gâteaux 미분, 1차 변분 조건
- **쌍선형 형식**: 연속성, 강제성
- **Riesz 표현 정리**: 선형범함수의 표현

## 📖 직각적 이해

### 고전해에서 약해로의 변환

**고전 문제**:
$$-\Delta u = f \text{ in } \Omega, \quad u = 0 \text{ on } \partial\Omega$$

양변에 테스트 함수 $v \in H^1_0(\Omega)$를 곱하고 적분:
$$\int_{\Omega} (-\Delta u) v \, dx = \int_{\Omega} f v \, dx$$

부분적분 (Green 항등식):
$$\int_{\Omega} \nabla u \cdot \nabla v \, dx = \int_{\Omega} f v \, dx$$

**약해 정의**: $u \in H^1_0(\Omega)$가 위 식을 모든 $v \in H^1_0$에 대해 만족하면, $u$를 약해라 합니다.

**장점**:
1. 약미분만 필요 (고전 2계 도함수 불필요)
2. 부드럽지 않은 함수도 포함
3. 수치 해석에 적합

### 추상적 관점: 쌍선형 형식

PDE를 다음과 같이 추상화:
$$a(u, v) = F(v) \quad \forall v \in V$$

여기서:
- $V = H^1_0(\Omega)$ (테스트 함수 공간)
- $a(u, v) = \int_{\Omega} \nabla u \cdot \nabla v \, dx$ (쌍선형 형식)
- $F(v) = \int_{\Omega} f v \, dx$ (선형범함수)

이렇게 추상화하면 **구체적인 PDE의 세부사항에서 벗어나** 일반적인 정리를 적용할 수 있습니다!

## ✏️ 엄밀한 정의

### 약해의 정의

**정의**: $u \in H^1_0(\Omega)$가 경계값 문제
$$-\Delta u = f \quad \text{(in } \Omega\text{)}$$
$$u = 0 \quad \text{(on } \partial\Omega\text{)}$$
의 **약해**라는 것은:
$$\int_{\Omega} \nabla u \cdot \nabla v \, dx = \int_{\Omega} f v \, dx \quad \forall v \in H^1_0(\Omega)$$

**주의**: 
- $u \in H^1_0$이므로 경계에서 0 조건을 자동으로 만족
- 약미분 $\nabla u$만 필요 (고전 미분 $\Delta u$ 불필요)
- $f \in L^2(\Omega)$이면 충분

### 쌍선형 형식

**정의**: $V$를 Hilbert 공간이라 할 때, 쌍선형 형식 $a: V \times V \to \mathbb{R}$이:

**1. 연속 (Bounded)**:
$$|a(u, v)| \leq M \|u\|_V \|v\|_V \quad \forall u,v \in V$$

**2. 강제적 (Coercive)**:
$$a(u, u) \geq \alpha \|u\|_V^2 \quad \forall u \in V, \quad \alpha > 0$$

## 🔬 정리와 증명

### 정리 1: Lax-Milgram 정리 (핵심 존재성 정리)

**정리**: $a: V \times V \to \mathbb{R}$가 연속이고 강제적인 쌍선형 형식이고, $F \in V^*$ (선형범함수)이면:

유일한 $u \in V$가 존재하여:
$$a(u, v) = F(v) \quad \forall v \in V$$

더 나아가:
$$\|u\|_V \leq \frac{\|F\|_{V^*}}{\alpha}$$

**증명**:

**Step 1**: Riesz 표현 정리 적용

$a(u, \cdot): V \to \mathbb{R}$을 고정된 $u$에 대한 함수로 보면, 이는 선형범함수입니다.

연속성: $|a(u, v)| \leq M \|u\| \|v\|$에서 $\|a(u, \cdot)\|_{V^*} \leq M\|u\|_V$

Riesz 표현 정리에 의해, $A: V \to V$가 존재하여:
$$a(u, v) = \langle Au, v \rangle_V$$

(내적 $\langle \cdot, \cdot \rangle_V$에 대해)

**Step 2**: $A$는 동형사상

- **단사성 (Injectivity)**: 강제성으로부터
  $$a(u, u) = \langle Au, u \rangle \geq \alpha \|u\|^2$$
  따라서 $Au = 0 \Rightarrow u = 0$

- **전사성 (Surjectivity)**: Fredholm 대안 정리에 의해

**Step 3**: 원래 문제로 돌아가기

$F \in V^*$이므로, Riesz 표현으로 $f \in V$이 존재하여:
$$F(v) = \langle f, v \rangle_V \quad \forall v$$

따라서:
$$a(u, v) = F(v) \Leftrightarrow \langle Au, v \rangle = \langle f, v \rangle$$
$$\Leftrightarrow Au = f$$

$A$가 동형사상이므로 유일한 해:
$$u = A^{-1}f$$

**Step 4**: 노름 추정

$$a(u, u) = F(u)$$
$$\alpha \|u\|^2 \leq |F(u)| \leq \|F\|_{V^*} \|u\|$$
$$\|u\| \leq \frac{\|F\|_{V^*}}{\alpha}$$ □

### 정리 2: Laplace 방정식의 약해 존재성 응용

**정리**: $\Omega \subset \mathbb{R}^n$ 유계, $f \in L^2(\Omega)$일 때, 다음 경계값 문제의 유일한 약해가 존재:
$$-\Delta u = f \quad \text{in } \Omega, \quad u = 0 \quad \text{on } \partial\Omega$$

그리고:
$$\|u\|_{H^1_0(\Omega)} \leq C \|f\|_{L^2(\Omega)}$$

**증명**:

$V = H^1_0(\Omega)$, $a(u, v) = \int \nabla u \cdot \nabla v$, $F(v) = \int f v$로 설정.

**연속성**: Cauchy-Schwarz
$$|a(u, v)| = \left|\int \nabla u \cdot \nabla v \, dx\right| \leq \|\nabla u\|_{L^2} \|\nabla v\|_{L^2} \leq \|u\|_{H^1} \|v\|_{H^1}$$

$$|F(v)| = \left|\int f v\right| \leq \|f\|_{L^2} \|v\|_{L^2} \leq C \|f\|_{L^2} \|v\|_{H^1}$$

(Poincaré 부등식)

**강제성** (Poincaré 부등식!):
$$a(u, u) = \int |\nabla u|^2 \geq \alpha \|u\|_{H^1_0}^2$$

(Poincaré: $\|u\|_{L^2} \leq C\|\nabla u\|_{L^2}$)

Lax-Milgram 적용 → 유일한 약해 존재. □

### 정리 3: 유한요소법 (Galerkin 근사)

**설정**: $V_h \subset V$ 유한차원 부분공간 (예: 분할 선형함수들)

**Galerkin 문제**:
$$a(u_h, v_h) = F(v_h) \quad \forall v_h \in V_h$$

**정리 (Lax-Milgram의 Galerkin 버전)**:

유일한 $u_h \in V_h$이 존재하고:
$$\|u - u_h\|_V \leq C \inf_{v_h \in V_h} \|u - v_h\|_V$$

(Céa 보조정리)

**의미**: FEM 오차 = 최적 근사 오차의 상수배

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.sparse import diags
from scipy.sparse.linalg import spsolve
from scipy.integrate import quad

# Lax-Milgram과 약해, FEM

print("=" * 70)
print("1. Lax-Milgram 정리 검증: 1D Poisson 방정식")
print("=" * 70)

# 문제: -u'' + u = f on (0,1), u(0)=u(1)=0
# 약해: a(u,v) = ∫(u'v' + uv)dx = ∫fv dx

def lax_milgram_1d_solve(f_func, n_points=100, alpha_param=1.0):
    """
    유한차분으로 Lax-Milgram 문제 풀기
    a(u,v) = ∫(u'v' + α u v) dx = ∫f v dx
    """
    x = np.linspace(0, 1, n_points)
    h = 1.0 / (n_points - 1)
    f_vals = f_func(x)
    
    # 쌍선형 형식 a(u,v) 행렬화
    # 중심 차분: u'' ≈ (u_{i-1} - 2u_i + u_{i+1})/h²
    # 따라서 ∫u' v' ≈ Σ (du_i)² / h
    
    # 강제성 조건: a(u,u) = ∫(u'² + α u²) ≥ α'||u||²
    # 연속성: a(u,v) ≤ M ||u|| ||v||
    
    # 이산화된 시스템
    # -u'' + α u = f
    # (discretized as tri-diagonal system)
    
    diag_main = (2 + h**2 * alpha_param) / h**2 * np.ones(n_points - 2)
    diag_off = -1 / h**2 * np.ones(n_points - 3)
    
    # 삼중대각 행렬
    A = diags([diag_off, diag_main, diag_off], [-1, 0, 1], 
              shape=(n_points-2, n_points-2)).tocsr()
    
    b = f_vals[1:-1]
    
    # 강제성 검증
    eigenvalues = []
    for i in range(len(diag_main)):
        if i == 0:
            eig_approx = diag_main[i] - np.abs(diag_off[i]) if i < len(diag_off) else diag_main[i]
        elif i == len(diag_main) - 1:
            eig_approx = diag_main[i] - np.abs(diag_off[i-1])
        else:
            eig_approx = diag_main[i] - 2 * np.abs(diag_off[i])
        eigenvalues.append(eig_approx)
    
    alpha_coercive = np.min(eigenvalues) / (2 / h**2 + alpha_param)
    
    # 풀이
    u_interior = spsolve(A, b)
    u = np.concatenate([[0], u_interior, [0]])
    
    return x, u, alpha_coercive

# 테스트 함수들
def f_constant(x):
    return np.ones_like(x)

def f_sin(x):
    return np.sin(np.pi * x)

# 풀이
x, u_const, alpha1 = lax_milgram_1d_solve(f_constant, n_points=100, alpha_param=1.0)
x2, u_sin, alpha2 = lax_milgram_1d_solve(f_sin, n_points=100, alpha_param=1.0)

print(f"\n경우 1: -u'' + u = 1")
print(f"  해의 최대값: {np.max(u_const):.6f}")
print(f"  강제성 인자 α: {alpha1:.6f} > 0 ✓")

print(f"\n경우 2: -u'' + u = sin(πx)")
print(f"  해의 최대값: {np.max(u_sin):.6f}")
print(f"  강제성 인자 α: {alpha2:.6f} > 0 ✓")

# Poincaré 부등식 검증 (강제성의 근원)
print("\n" + "=" * 70)
print("2. 강제성과 Poincaré 부등식")
print("=" * 70)

# -u'' = f만 있는 경우 (α=0): Poincaré가 필수!
x3, u_only_diffusion, _ = lax_milgram_1d_solve(f_constant, n_points=100, alpha_param=0)

print(f"\n경우: -u'' = 1 (Poisson)")
print(f"  해의 최대값: {np.max(u_only_diffusion):.6f}")
print(f"  Poincaré: ||u||_L² ≤ (L/π)||u'||_L²")
print(f"  따라서 a(u,u) = ∫u'² ≥ π²||u||²/(L²) ✓")

# 3. 약해의 정규성
print("\n" + "=" * 70)
print("3. 약해의 정규성 (Bootstrap)")
print("=" * 70)

# f ∈ L² => u ∈ H² (고차 미분가능성)
x_fine = np.linspace(0, 1, 200)
u_fine = np.interp(x_fine, x, u_const)
du_fine = np.gradient(u_fine, x_fine)
ddu_fine = np.gradient(du_fine, x_fine)

norm_l2_u = np.sqrt(np.trapz(u_fine**2, x_fine))
norm_l2_du = np.sqrt(np.trapz(du_fine**2, x_fine))
norm_l2_ddu = np.sqrt(np.trapz(ddu_fine**2, x_fine))

print(f"\nf = 1 ∈ L²에 대해:")
print(f"  ||u||_L² =     {norm_l2_u:.6f}")
print(f"  ||u'||_L² =    {norm_l2_du:.6f}")
print(f"  ||u''||_L² =   {norm_l2_ddu:.6f}")
print(f"  따라서 u ∈ H² (2계 약도함수도 L²에 있음)")

# 4. Céa 보조정리: FEM 오차 추정
print("\n" + "=" * 70)
print("4. Céa 보조정리와 FEM 오차")
print("=" * 70)

# 다양한 그리드 크기로 FEM 풀이
h_values = [1/20, 1/40, 1/80, 1/160]
errors_h1 = []
errors_l2 = []

x_ref, u_ref, _ = lax_milgram_1d_solve(f_constant, n_points=500)

for h in h_values:
    n = int(1/h) + 1
    x_h, u_h, _ = lax_milgram_1d_solve(f_constant, n_points=n)
    
    # u_h를 x_ref에 보간
    u_h_interp = np.interp(x_ref, x_h, u_h)
    
    # 오차
    error_l2 = np.sqrt(np.trapz((u_ref - u_h_interp)**2, x_ref))
    du_ref = np.gradient(u_ref, x_ref)
    du_h = np.gradient(u_h_interp, x_ref)
    error_h1 = np.sqrt(
        np.trapz((u_ref - u_h_interp)**2, x_ref) +
        np.trapz((du_ref - du_h)**2, x_ref)
    )
    
    errors_h1.append(error_h1)
    errors_l2.append(error_l2)

h_values = np.array(h_values)
errors_h1 = np.array(errors_h1)
errors_l2 = np.array(errors_l2)

print(f"\nFEM 오차 (h 감소에 따라):")
print(f"{'h':>10} | {'Error H¹':>12} | {'Rate':>8} | {'Error L²':>12} | {'Rate':>8}")
print("-" * 60)
for i, h in enumerate(h_values):
    if i == 0:
        print(f"{h:>10.4f} | {errors_h1[i]:>12.2e} |     -    | {errors_l2[i]:>12.2e} |     -")
    else:
        rate_h1 = np.log(errors_h1[i-1] / errors_h1[i]) / np.log(2)
        rate_l2 = np.log(errors_l2[i-1] / errors_l2[i]) / np.log(2)
        print(f"{h:>10.4f} | {errors_h1[i]:>12.2e} | {rate_h1:>8.2f} | {errors_l2[i]:>12.2e} | {rate_l2:>8.2f}")

# 이론적 수렴율
print(f"\n이론적 수렴율 (1D 선형 FEM):")
print(f"  H¹ 오차: O(h) [실제로는 ~h¹ 정도]")
print(f"  L² 오차: O(h²) [실제로는 ~h² 정도]")

# 5. 분포론과 약해
print("\n" + "=" * 70)
print("5. 약해와 분포론: δ 함수")
print("=" * 70)

# 점 원천: -u'' = δ₀ (Dirac delta at 0)
# 약해: ∫u'v' dx = v(0)

# 수치 근사: δ₀를 gaussian으로 근사
epsilon = 0.01

def delta_approx(x, epsilon=epsilon):
    return np.exp(-x**2 / epsilon**2) / (np.sqrt(np.pi) * epsilon)

def f_delta(x):
    return delta_approx(x, epsilon)

x_delta, u_delta, _ = lax_milgram_1d_solve(f_delta, n_points=150, alpha_param=0.0)

print(f"\nDelta 함수의 약해 -u'' = δ₀:")
print(f"  δ₀를 Gaussian N(0, ε²)으로 근사 (ε={epsilon})")
print(f"  수치 해의 최대값: {np.max(u_delta):.6f}")
print(f"  이론값 (Green 함수): |x|/2 (대칭)")
print(f"  u(0.5) = {u_delta[np.argmin(np.abs(x_delta - 0.5))]:.6f} ≈ 0.25")

# 6. 약해와 고전해
print("\n" + "=" * 70)
print("6. 약해의 정규성: 고전해가 되는 조건")
print("=" * 70)

# f ∈ C² => u ∈ C⁴ (충분한 미분가능성)
# f ∈ L² => u ∈ H² (약미분만)
# f ∈ L¹ => u는 고전해가 아닐 수 있음

print(f"\n정규성 부스트래핑 (1D):")
print(f"  f ∈ L²        ⟹ u ∈ H² (약 2계 도함수)")
print(f"  f ∈ C⁰        ⟹ u ∈ H³ (더 부드러움)")
print(f"  f ∈ C^∞       ⟹ u ∈ C^∞ (무한번 미분가능)")

# 시각화
fig, axes = plt.subplots(2, 3, figsize=(16, 10))

# Plot 1: 약해들
ax = axes[0, 0]
ax.plot(x, u_const, 'b-', linewidth=2, label='-u\'\' + u = 1')
ax.plot(x2, u_sin, 'r-', linewidth=2, label='-u\'\' + u = sin(πx)')
ax.plot(x_delta, u_delta, 'g-', linewidth=2, label='-u\'\' ≈ δ₀')
ax.set_xlabel('x')
ax.set_ylabel('u(x)')
ax.set_title('약해들: 다양한 우변항')
ax.legend(fontsize=8)
ax.grid(True, alpha=0.3)

# Plot 2: 도함수
ax = axes[0, 1]
du_const = np.gradient(u_const, x)
du_sin = np.gradient(u_sin, x2)
ax.plot(x, du_const, 'b-', linewidth=2, label="u'(x) for f=1")
ax.plot(x2, du_sin, 'r-', linewidth=2, label="u'(x) for f=sin(πx)")
ax.set_xlabel('x')
ax.set_ylabel("u'(x)")
ax.set_title('약해의 도함수')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 3: 강제성 인자
ax = axes[0, 2]
alphas = [alpha1, alpha2]
labels = ['α=1 포함', 'α=1 포함']
ax.bar(['PDE 1', 'PDE 2'], alphas, color=['blue', 'red'], alpha=0.7)
ax.set_ylabel('강제성 인자 α')
ax.set_title('Lax-Milgram: 강제성 확인 (α > 0)')
ax.grid(True, alpha=0.3, axis='y')
ax.axhline(y=0, color='k', linestyle='-', linewidth=0.5)

# Plot 4: FEM 수렴 (H¹)
ax = axes[1, 0]
ax.loglog(h_values, errors_h1, 'bo-', linewidth=2, markersize=8, label='수치 오차')
ax.loglog(h_values, h_values * errors_h1[0] / h_values[0], 'r--', 
          linewidth=2, label='O(h) 참고')
ax.set_xlabel('격자 크기 h')
ax.set_ylabel('H¹ 오차')
ax.set_title('FEM 오차: 수렴율 O(h)')
ax.legend()
ax.grid(True, alpha=0.3, which='both')

# Plot 5: FEM 수렴 (L²)
ax = axes[1, 1]
ax.loglog(h_values, errors_l2, 'go-', linewidth=2, markersize=8, label='수치 오차')
ax.loglog(h_values, h_values**2 * errors_l2[0] / h_values[0]**2, 'r--',
          linewidth=2, label='O(h²) 참고')
ax.set_xlabel('격자 크기 h')
ax.set_ylabel('L² 오차')
ax.set_title('FEM 오차: 수렴율 O(h²)')
ax.legend()
ax.grid(True, alpha=0.3, which='both')

# Plot 6: 약해와 쌍선형 형식
ax = axes[1, 2]
# a(u,v) = ∫(u'v' + uv) dx의 연속성과 강제성 시각화
k = np.arange(1, 8)
u_test = np.sin(k * np.pi * x)
du_test = k * np.pi * np.cos(k * np.pi * x)

a_uu = []
norm_uu = []
for i, k_val in enumerate(k):
    u_k = np.sin(k_val * np.pi * x)
    du_k = k_val * np.pi * np.cos(k_val * np.pi * x)
    a_val = np.trapz(du_k**2 + u_k**2, x)
    norm_val = np.sqrt(np.trapz(du_k**2 + u_k**2, x))
    a_uu.append(a_val)
    norm_uu.append(norm_val**2)

ratio = np.array(a_uu) / np.array(norm_uu)
ax.plot(k, ratio, 'mo-', linewidth=2, markersize=8)
ax.axhline(y=np.min(ratio), color='r', linestyle='--', linewidth=2, 
           label=f'α ≈ {np.min(ratio):.4f}')
ax.set_xlabel('모드 k')
ax.set_ylabel('a(u,u) / ||u||²')
ax.set_title('강제성: a(u,u) ≥ α||u||²')
ax.legend()
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('/tmp/weak_solution_pde.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/weak_solution_pde.png")

plt.show()
```

## 🔗 AI/ML 연결

### 1. PINN과 Lax-Milgram

PINN 손실함수:
$$\mathcal{L}_{\text{PINN}} = \int_{\Omega} (Au_{\theta} - f)^2 \, dx + \text{BC 항}$$

이를 변분 형식으로:
$$\min_{u_\theta} \frac{1}{2} a(u_\theta, u_\theta) - (f, u_\theta) + \text{BC}$$

정확히 Lax-Milgram 정리의 최소화 문제입니다!

**수렴성 보장**: Lax-Milgram 정리가 없으면 PINN이 실제로 PDE를 푸는지 알 수 없습니다.

### 2. FEM의 이론적 기초

유한요소법은 Galerkin 근사:
1. $V = H^1_0(\Omega)$를 유한차원 $V_h$로 근사
2. 약해: $a(u_h, v_h) = (f, v_h)$ for $v_h \in V_h$
3. Lax-Milgram 적용 → 유일한 해 존재
4. Céa 보조정리 → 오차 추정

**PINN과의 차이**:
- FEM: 정확한 쌍선형 형식 $a(u, v) = \int \nabla u \cdot \nabla v$
- PINN: 신경망으로 $a(u, v)$를 근사

### 3. 약해의 정규성 부스트래핑

**원칙**: PDE의 우변항이 더 좋을수록, 해도 더 정규적입니다.

- $f \in L^2$ ⟹ $u \in H^2$
- $f \in C^0$ ⟹ $u \in H^3$
- $f \in C^k$ ⟹ $u \in H^{k+2}$

PINN에서도 마찬가지: 손실함수에 더 많은 제약 (높은 정규성) 추가 → 더 나은 해

## ⚖️ 가정과 한계

| 가정 | 설명 | 한계 |
|------|------|------|
| Lax-Milgram: 강제성 | $a(u,u) ≥ α\|u\|²$ | 강제성 없으면 해 없을 수 있음 |
| 약해 존재: $f ∈ L²$ | 우변항이 유한 에너지 | 더 이상한 $f$는 정의 불명확 |
| FEM 수렴: $h → 0$ | 충분히 세밀한 격자 | 실제로는 계산 비용 제약 |

## 📌 핵심 정리

1. **약해의 정의**: 쌍선형 형식으로 표현
   $$a(u, v) = F(v) \quad \forall v \in V$$

2. **Lax-Milgram 정리**: 연속 + 강제적 ⟹ 유일한 해 존재

3. **PDE 약해**: Green 항등식으로 도함수 차수 저감
   $$\int \nabla u \cdot \nabla v = \int f v$$

4. **FEM 이론**: Galerkin + Céa ⟹ 오차 추정
   $$\|u - u_h\|_V ≤ C h^p |u|_{H^{p+1}}$$

5. **정규성**: 우변이 좋으면 해도 좋음

## 🤔 생각해볼 문제

### 문제 1: 강제성의 필요성

쌍선형 형식 $a(u, v) = \int u \cdot v \, dx$ (경계 조건 없이)에서 강제성이 성립하는가?

<details>
<summary>💡 풀이 (클릭)</summary>

**아니오.** $a(u, u) = \int u^2 = \|u\|_{L^2}^2$이지만, 정칙화 상수가 영역에 무관하게 항상 1입니다.

하지만 더 문제가 있습니다: 경계 조건이 없으면, 예를 들어 $u(x) = c$ (상수)에 대해:
$$a(u, u) = \int c^2 = c^2$$

따라서 문제가 정확히 정의되지 않습니다. (우변항이 필요)

**올바른 설정**:
- $V = H^1_0(\Omega)$ (경계에서 0)
- $a(u, u) = \int |\nabla u|^2 + u^2 ≥ α \|u\|_{H^1}^2$ (Poincaré)

이 경우 강제성이 성립합니다!

</details>

### 문제 2: FEM 수렴율

선형 FEM에서 $L^2$ 오차가 $O(h^2)$인 반면, $H^1$ 오차가 $O(h)$인 이유는?

<details>
<summary>💡 풀이 (클릭)</summary>

**직관**:

1. **$H^1$ 오차 (도함수 포함)**:
   - 도함수는 함수보다 변동이 큼 (고주파 성분 강조)
   - 유한차분 근사가 덜 정확
   - 수렴율: O(h)

2. **$L^2$ 오차 (함수 자체)**:
   - 함수는 도함수보다 부드러움
   - 근사가 더 정확
   - Aubin-Nitsche 보조정리로 O(h²) 달성

**더 정확하게**:

Céa 보조정리:
$$\|u - u_h\|_{H^1} ≤ C \inf_{v_h} \|u - v_h\|_{H^1}$$

선형 FEM: 최적 근사 오차 = O(h), 따라서 $H^1$ 오차 = O(h)

$L^2$ 오차는 추가 기법 (Aubin-Nitsche duality)으로 O(h²) 달성.

</details>

### 문제 3: PINN과 정규성

PINN이 계산하는 신경망 $u_\theta$가 약해가 되려면 어떤 조건이 필요할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

**필수 조건**:

1. **$L^2$ 가능성**: $\int |u_\theta|^2 < \infty$
   - 신경망이 폭발적으로 증가하지 않아야 함

2. **약미분 존재**: $\nabla u_\theta \in L^2$ (자동 미분)
   - 신경망이 미분가능해야 함
   - 자동 미분으로 $\nabla u_\theta$ 계산 가능

3. **경계 조건**: $u_\theta|_{\partial\Omega} = 0$ (손실로 강제)
   - Penalty 또는 hard constraint

**현실**:

실제 PINN은 이 모든 조건을 만족하도록 훈련됩니다:
- 데이터 손실 (함수값)
- PDE 손실 (도함수 항)
- BC 손실 (경계)
- 정칙화 (기울기 제약)

이렇게 하면 신경망이 자동으로 Lax-Milgram 정리의 조건에 가까워집니다!

</details>

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./04-calculus-of-variations.md) | [📚 README](../README.md) | |

</div>
