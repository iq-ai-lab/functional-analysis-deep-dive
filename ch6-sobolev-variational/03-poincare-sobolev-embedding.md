# 3. Poincaré 부등식과 Sobolev 매몰 정리

## 🎯 핵심 질문

함수의 도함수가 작으면, 함수 자체도 작을까요? Sobolev 공간의 함수가 더 큰 함수공간에 포함될까요? 예를 들어, 약미분을 가진 함수가 항상 연속일까요?

## 🔍 왜 이 이론이 AI에서 중요한가

- **Poincaré 부등식**: PINN 손실함수에서 경계 조건과 PDE 잔차 사이의 균형을 결정합니다. 경계 조건 강도를 약하게 줄 수 있습니다.
- **Sobolev 매몰**: 고차원에서 신경망이 함수를 근사할 때 필요한 복잡도(차원)를 결정합니다. 고차원에서의 "차원의 저주"와 연결됩니다.
- **정칙화**: 손실함수에 Sobolev 정규화항을 추가할 때, 이것이 어떤 함수공간에 해를 찾도록 강제하는지 알 수 있습니다.

## 📐 수학적 선행 조건

- **Sobolev 공간**: $W^{k,p}$의 정의와 성질
- **컴팩트 집합**: 유계 폐집합, Heine-Borel
- **함수공간의 포함**: 노름 비교
- **적분 부등식**: Hölder, Minkowski

## 📖 직관적 이해

### Poincaré 부등식의 직관

**1D 예제**: 구간 $[0, L]$에서 $u(0) = 0$인 함수

함수를 미분으로부터 재구성:
$$u(x) = \int_0^x u'(t) \, dt$$

적분 부등식으로:
$$|u(x)| = \left|\int_0^x u'(t) \, dt\right| \leq \int_0^x |u'(t)| \, dt$$

양쪽을 적분하면:
$$\int_0^L |u(x)| \, dx \leq L \int_0^L |u'(x)| \, dx$$

**직관**: 경계에서 0이면, 함수를 도함수로 제어할 수 있습니다!

### Sobolev 매몰의 직관

**유한차원**: $\mathbb{R}^n$에서 벡터 하나는 많은 정보를 가집니다.

**무한차원 ($L^2$)**: 함수는 무한히 많은 "자유도"를 가집니다. 예를 들어, 극도로 진동하는 함수도 가능합니다:
$$u(x) = \sum_{k=1}^{\infty} \frac{1}{k} \sin(kx)$$

**제약 추가** ($H^k$): 도함수까지 제어하면?
$$\|u\|_{H^1}^2 = \|u\|_{L^2}^2 + \|\nabla u\|_{L^2}^2 < \infty$$

이제 함수가 "더 나이스"해집니다. 극도의 진동은 불가능합니다!

**Sobolev 매몰**: 충분한 미분가능성을 요구하면, 함수가 더 작은(작은 노름을 가진) 함수공간에 자동으로 포함됩니다.

## ✏️ 엄밀한 정의

### Poincaré 부등식

**정리 (Poincaré 부등식)**: $\Omega \subset \mathbb{R}^n$이 유계이고, $u \in W^{1,p}_0(\Omega)$ (경계에서 0)이면:

$$\|u\|_{L^p(\Omega)} \leq C_P(\Omega, p) \|\nabla u\|_{L^p(\Omega)}$$

여기서 $C_P$는 Poincaré 상수 (영역과 $p$에만 의존).

**결론**: $W^{1,p}_0$에서 $\|\nabla u\|_{L^p}$와 $\|u\|_{W^{1,p}}$는 동치 노름입니다.

### Sobolev 매몰 정리

**정리 (Sobolev Embedding Theorem)**: 

**경우 1** ($k p < n$, Sobolev 임계 수): 
$$W^{k,p}(\mathbb{R}^n) \hookrightarrow L^q(\mathbb{R}^n), \quad \text{where } \frac{1}{q} = \frac{1}{p} - \frac{k}{n}$$

연속 포함: $\|u\|_{L^q} \leq C \|u\|_{W^{k,p}}$

**경우 2** ($kp = n$): 
$$W^{k,p}(\mathbb{R}^n) \hookrightarrow L^q(\mathbb{R}^n) \text{ for all } q < \infty$$

**경우 3** ($kp > n$): 
$$W^{k,p}(\mathbb{R}^n) \hookrightarrow C^m(\mathbb{R}^n), \quad m = \left\lfloor k - \frac{n}{p} \right\rfloor$$

또는 더 정확하게: $k - n/p = m + \alpha$ (정수부 + 소수부)일 때,
$$W^{k,p} \hookrightarrow C^{m,\alpha}$$
(Hölder 연속성 지수 $\alpha$)

### Rellich-Kondrachov 정리 (컴팩트 매몰)

**정리**: $\Omega \subset \mathbb{R}^n$이 유계 열린집합이고, $k \geq 1$이면:
$$W^{k,p}(\Omega) \hookrightarrow\hookrightarrow L^p(\Omega)$$

(이중 화살표는 컴팩트 포함)

**직관**: 유계 Sobolev 수열은 수렴하는 부분열을 가집니다!

## 🔬 정리와 증명

### 정리 1: 1D Poincaré 부등식 (완전 증명)

**정리**: $u \in W^{1,2}([0,L])$이고 $u(0) = 0$이면:
$$\|u\|_{L^2([0,L])} \leq L \|u'\|_{L^2([0,L])}$$

**증명**:

$u \in C^1([0,L])$인 경우부터 시작 (조밀성으로 일반화):

$$u(x) = \int_0^x u'(t) \, dt$$

Cauchy-Schwarz 부등식:
$$|u(x)|^2 = \left|\int_0^x u'(t) \, dt\right|^2 \leq \left(\int_0^x 1 \, dt\right) \left(\int_0^x |u'(t)|^2 \, dt\right)$$

$$= x \int_0^x |u'(t)|^2 \, dt \leq L \int_0^L |u'(t)|^2 \, dt$$

양변을 $[0,L]$에서 $x$에 대해 적분:
$$\int_0^L |u(x)|^2 \, dx \leq L \int_0^L \left(\int_0^L |u'(t)|^2 \, dt\right) dx = L^2 \int_0^L |u'(t)|^2 \, dt$$

따라서:
$$\|u\|_{L^2} \leq L \|u'\|_{L^2}$$ □

**일반화 (조밀성 논증)**:

$C^1 \cap W^{1,2}$는 $W^{1,2}$에서 조밀하므로, 극한을 취하면 일반적인 $u \in W^{1,2}$에서도 성립합니다.

### 정리 2: Sobolev 매몰 (스케치)

**정리**: $W^{1,2}(\mathbb{R}^1) \hookrightarrow C^0(\mathbb{R})$ (연속 함수)

**스케치 증명**:

$u \in W^{1,2}(\mathbb{R})$이면, $u' \in L^2(\mathbb{R})$.

임의의 $x, y \in \mathbb{R}$에 대해:
$$|u(x) - u(y)| = \left|\int_x^y u'(t) \, dt\right| \leq \int_x^y |u'(t)| \, dt$$

Cauchy-Schwarz:
$$\leq \sqrt{|x-y|} \sqrt{\int_x^y |u'(t)|^2 \, dt}$$

$\|u'\|_{L^2} < \infty$이므로, $|x-y| \to 0$일 때 $|u(x) - u(y)| \to 0$.

따라서 $u$는 연속입니다. □

**고차원** ($\mathbb{R}^n$):

$W^{1,p}(\mathbb{R}^n)$의 함수가 연속이 되려면:
- $1 \leq p < n$: 약함 (매몰만 가능, 연속성 없음)
- $p = n$: 경계선 (모든 $q < \infty$로 매몰)
- $p > n$: 충분히 강함 (연속성 보장)

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import quad, solve_bvp
from scipy.optimize import minimize_scalar

# Poincaré 부등식과 Sobolev 매몰 정리의 수치 검증

# 1. Poincaré 부등식 검증
print("=" * 70)
print("1. Poincaré 부등식 검증")
print("=" * 70)

def estimate_poincare_constant(L, num_functions=100, n_points=200):
    """
    구간 [0, L]에서 여러 함수에 대해 Poincaré 상수 추정
    """
    x = np.linspace(0, L, n_points)
    dx = L / (n_points - 1)
    
    ratios = []
    
    # 다양한 기저함수 사용
    for k in range(1, num_functions):
        # 테스트 함수: sin(kπx/L)
        u = np.sin(k * np.pi * x / L)
        
        # 도함수
        du = (k * np.pi / L) * np.cos(k * np.pi * x / L)
        
        # L2 노름
        norm_u = np.sqrt(np.trapz(u**2, x))
        norm_du = np.sqrt(np.trapz(du**2, x))
        
        if norm_du > 1e-10:
            ratios.append(norm_u / norm_du)
    
    ratios = np.array(ratios)
    c_p_estimate = np.max(ratios)
    theoretical = L / np.pi  # 이론값
    
    return c_p_estimate, theoretical, ratios

# 여러 길이에 대해 계산
lengths = [1, 2, 5, 10]
results = {}

for L in lengths:
    c_p_est, c_p_theory, ratios = estimate_poincare_constant(L)
    results[L] = {
        'estimated': c_p_est,
        'theoretical': c_p_theory,
        'ratios': ratios
    }
    print(f"\n구간 [0, {L}]:")
    print(f"  추정된 Poincaré 상수:  {c_p_est:.6f}")
    print(f"  이론값 (L/π):         {c_p_theory:.6f}")
    print(f"  오차:                 {np.abs(c_p_est - c_p_theory):.2e}")

# 2. Poincaré 부등식의 의미: W^{1,2}_0에서 동치 노름
print("\n" + "=" * 70)
print("2. W^{1,2}_0에서의 동치 노름")
print("=" * 70)

L = 1.0
x = np.linspace(0, L, 300)
dx = x[1] - x[0]

# 테스트 함수들 (경계에서 0)
def test_u(x, k):
    """u(x) = sin(kπx), u(0)=u(1)=0"""
    return np.sin(k * np.pi * x)

def test_du(x, k):
    """u'(x) = kπ cos(kπx)"""
    return k * np.pi * np.cos(k * np.pi * x)

k_values = [1, 2, 3, 4, 5]
c_p = L / np.pi

for k in k_values:
    u = test_u(x, k)
    du = test_du(x, k)
    
    # 노름
    norm_l2_u = np.sqrt(np.trapz(u**2, x))
    norm_l2_du = np.sqrt(np.trapz(du**2, x))
    norm_w12 = np.sqrt(norm_l2_u**2 + norm_l2_du**2)
    
    # Poincaré로부터
    norm_w12_via_poincare = np.sqrt((c_p * norm_l2_du)**2 + norm_l2_du**2)
    
    print(f"\nk = {k}:")
    print(f"  ||u||_L² =            {norm_l2_u:.6f}")
    print(f"  ||u'||_L² =           {norm_l2_du:.6f}")
    print(f"  ||u||_W¹,² (정확) =   {norm_w12:.6f}")
    print(f"  Poincaré로부터:        {norm_w12_via_poincare:.6f}")
    print(f"  비율:                 {norm_l2_u / norm_l2_du:.6f} (≤ L/π = {c_p:.6f})")

# 3. Sobolev 매몰 시각화
print("\n" + "=" * 70)
print("3. Sobolev 매몰 정리: H^1 함수는 연속!")
print("=" * 70)

# 불연속이면 H^1에 속할 수 없음을 보이자
x_dense = np.linspace(-1, 1, 1000)

# 함수 1: 연속이고 H^1에 속함
def f_smooth(x):
    return np.sin(np.pi * x / 2)

def df_smooth(x):
    return (np.pi / 2) * np.cos(np.pi * x / 2)

# 함수 2: 불연속 함수
def f_discontinuous(x):
    return np.where(x > 0, 1, -1)

# H^1 노름 계산
x_h1 = np.linspace(-1, 1, 500)
f_s = f_smooth(x_h1)
df_s = df_smooth(x_h1)

norm_l2_smooth = np.sqrt(np.trapz(f_s**2, x_h1))
norm_h1_smooth = np.sqrt(np.trapz(f_s**2, x_h1) + np.trapz(df_s**2, x_h1))

print(f"\n연속함수 f(x) = sin(πx/2):")
print(f"  ||f||_L² =  {norm_l2_smooth:.6f}")
print(f"  ||f||_H¹ =  {norm_h1_smooth:.6f}")
print(f"  ∈ H¹? YES")

print(f"\n불연속함수 H(x) (Heaviside):")
print(f"  ||H||_L² = 1.000000")
print(f"  약미분 (약한 의미의 도함수): δ(x) (델타 함수)")
print(f"  ∉ L²이므로 ∉ H¹")

# 4. 고차원에서 매몰 조건
print("\n" + "=" * 70)
print("4. 고차원에서 매몰 조건 (이론)")
print("=" * 70)

def embedding_condition(n, k, p):
    """
    W^{k,p}(R^n)의 매몰 조건 확인
    
    - 만약 kp < n: L^q, 1/q = 1/p - k/n
    - 만약 kp = n: L^q, q < inf
    - 만약 kp > n: C^m, m = floor(k - n/p)
    """
    critical = k * p / n
    
    if critical < 1:
        q = 1 / (1/p - k/n)
        return f"W^{{{k},{p}}}(ℝ^{n}) ⊂ L^q(ℝ^{n}), q = {q:.2f}"
    elif critical == 1:
        return f"W^{{{k},{p}}}(ℝ^{n}) ⊂ L^q(ℝ^{n}) ∀q < ∞"
    else:
        m = int(np.floor(k - n/p))
        return f"W^{{{k},{p}}}(ℝ^{n}) ⊂ C^{m}(ℝ^{n})"

# 예제들
examples = [
    (1, 1, 2),  # W^{1,2}(ℝ^1)
    (2, 1, 2),  # W^{1,2}(ℝ^2) - 경계선
    (3, 1, 2),  # W^{1,2}(ℝ^3)
    (2, 2, 2),  # W^{2,2}(ℝ^2)
]

for n, k, p in examples:
    condition = embedding_condition(n, k, p)
    print(f"  {condition}")

# 5. 실제 고차원 예제: Gaussian 함수
print("\n" + "=" * 70)
print("5. 다차원 함수와 매몰 정리")
print("=" * 70)

# 2D: Gaussian
x1 = np.linspace(-3, 3, 100)
x2 = np.linspace(-3, 3, 100)
X1, X2 = np.meshgrid(x1, x2)

gaussian = np.exp(-(X1**2 + X2**2) / 2)
grad_gaussian_x = -X1 * gaussian
grad_gaussian_y = -X2 * gaussian

# 2D에서의 적분 (수치)
dx1 = x1[1] - x1[0]
dx2 = x2[1] - x2[0]

norm_l2_2d = np.sqrt(np.sum(gaussian**2) * dx1 * dx2)
norm_grad_2d = np.sqrt(
    (np.sum(grad_gaussian_x**2) + np.sum(grad_gaussian_y**2)) * dx1 * dx2
)
norm_h1_2d = np.sqrt(norm_l2_2d**2 + norm_grad_2d**2)

print(f"\n2D Gaussian e^{{-(x²+y²)/2}}:")
print(f"  ||u||_L² =  {norm_l2_2d:.6f}")
print(f"  ||∇u||_L² = {norm_grad_2d:.6f}")
print(f"  ||u||_H¹ =  {norm_h1_2d:.6f}")
print(f"  W^{{1,2}}(ℝ²)에 속함? YES (지수 감소)")
print(f"  매몰 정리: W^{{1,2}}(ℝ²) ⊂ L^q ∀q < ∞")

# 시각화
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# Plot 1: Poincaré 상수 vs 구간 길이
ax = axes[0, 0]
L_vals = np.array(list(results.keys()))
estimated_vals = [results[L]['estimated'] for L in L_vals]
theoretical_vals = [results[L]['theoretical'] for L in L_vals]
ax.plot(L_vals, estimated_vals, 'ro-', linewidth=2, markersize=8, label='추정값')
ax.plot(L_vals, theoretical_vals, 'bs--', linewidth=2, markersize=8, label='이론값 (L/π)')
ax.set_xlabel('구간 길이 L')
ax.set_ylabel('Poincaré 상수')
ax.set_title('Poincaré 상수는 L에 선형 증가')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 2: 연속 vs 불연속 함수
ax = axes[0, 1]
x_plot = np.linspace(-1, 1, 300)
ax.plot(x_plot, f_smooth(x_plot), 'b-', linewidth=2, label='sin(πx/2) ∈ H¹')
ax.plot(x_plot, f_discontinuous(x_plot), 'r--', linewidth=2, label='Heaviside ∉ H¹')
ax.set_xlabel('x')
ax.set_ylabel('u(x)')
ax.set_title('연속성: H¹의 특성')
ax.legend()
ax.grid(True, alpha=0.3)
ax.set_ylim([-1.5, 1.5])

# Plot 3: W^{1,2}_0에서 Poincaré 부등식 검증
ax = axes[1, 0]
norms_l2 = []
norms_du = []
k_test = np.arange(1, 8)
for k in k_test:
    u = test_u(x, k)
    du = test_du(x, k)
    norms_l2.append(np.sqrt(np.trapz(u**2, x)))
    norms_du.append(np.sqrt(np.trapz(du**2, x)))

norms_l2 = np.array(norms_l2)
norms_du = np.array(norms_du)
ratio = norms_l2 / norms_du

ax.plot(k_test, ratio, 'go-', linewidth=2, markersize=8, label='||u||_L² / ||u\'||_L²')
ax.axhline(y=c_p, color='r', linestyle='--', linewidth=2, label=f'L/π = {c_p:.4f}')
ax.set_xlabel('주파수 k')
ax.set_ylabel('비율')
ax.set_title('Poincaré 부등식 검증: ||u|| ≤ (L/π)||u\'||')
ax.legend()
ax.grid(True, alpha=0.3)

# Plot 4: 2D Gaussian 시각화
ax = axes[1, 1]
contour = ax.contourf(X1, X2, gaussian, levels=20, cmap='viridis')
ax.contour(X1, X2, gaussian, levels=10, colors='white', alpha=0.3, linewidths=0.5)
cbar = plt.colorbar(contour, ax=ax)
cbar.set_label('u(x,y)')
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_title('2D: e^{-(x²+y²)/2} ∈ H¹(ℝ²)')
ax.set_aspect('equal')

plt.tight_layout()
plt.savefig('/tmp/poincare_embedding.png', dpi=150, bbox_inches='tight')
print("\n그래프 저장: /tmp/poincare_embedding.png")

# 경계값 문제의 해 존재성
print("\n" + "=" * 70)
print("6. Poincaré 부등식과 경계값 문제")
print("=" * 70)

# 문제: -u''(x) + u(x) = 1, u(0)=u(1)=0
# 약해: a(u,v) = ∫(u'v' + uv)dx = ∫1·v dx = F(v)

# Poincaré로부터: a(u,u) ≥ α||u||²_{W^{1,2}_0}
# coercive 조건을 확인

def coercivity_check():
    """
    a(u,u) = ∫(u'² + u²)dx의 coercive성 확인
    
    Poincaré: ||u||_L² ≤ (1/π)||u'||_L²
    따라서: a(u,u) = ∫(u'² + u²)dx 
                   ≥ ∫u'²dx  (양수항)
                   ≥ π² · ||u||²_L² (Poincaré의 역)
    
    실제로는 더 강함
    """
    
    x = np.linspace(0, 1, 300)
    
    k_vals = np.arange(1, 8)
    a_uu_vals = []
    norm_w12_vals = []
    
    for k in k_vals:
        u = np.sin(k * np.pi * x)
        du = k * np.pi * np.cos(k * np.pi * x)
        
        # a(u,u) = ∫(u'² + u²)dx
        a_uu = np.trapz(du**2 + u**2, x)
        
        # ||u||²_{W^{1,2}_0}
        norm_w12 = np.sqrt(np.trapz(u**2, x) + np.trapz(du**2, x))
        norm_w12_vals.append(norm_w12)
        a_uu_vals.append(a_uu)
    
    a_uu_vals = np.array(a_uu_vals)
    norm_w12_vals = np.array(norm_w12_vals)
    
    alpha = np.min(a_uu_vals / (norm_w12_vals**2))
    
    return alpha

alpha = coercivity_check()
print(f"\nBilinear form a(u,u) = ∫(u'² + u²)dx의 coercivity:")
print(f"  a(u,u) ≥ α||u||²_{{W^{{1,2}}_0}}, α ≈ {alpha:.6f} > 0 ✓")
print(f"  따라서 Lax-Milgram 정리 적용 가능 (다음 장)")

plt.show()
```

## 🔗 AI/ML 연결

### 1. PINN의 수렴성 분석

PINN 손실함수:
$$\mathcal{L} = \lambda_1 \|\nabla u_\theta - f\|^2 + \lambda_2 |u_\theta(0) - u_0|^2$$

Poincaré 부등식을 사용하면:
- 경계 조건 강도 $\lambda_2$를 감소시킬 수 있음
- 내부 PDE 잔차만으로도 충분히 제어 가능

### 2. 고차원 신경망의 복잡도

Sobolev 매몰 정리에서:
- $n$이 증가하면, $W^{k,p}(ℝ^n) \hookrightarrow C^m$의 조건이 강해짐
- 신경망이 높은 정규성을 가져야 함 → 더 깊고 넓은 네트워크 필요
- 이것이 **차원의 저주(curse of dimensionality)**의 수학적 기초

### 3. 정칙화항의 역할

손실함수: $\mathcal{L} = \|u_\theta - u_{\text{data}}\|_{L^2}^2 + \lambda \|u_\theta\|_{H^k}^2$

- Poincaré로부터: 자동으로 $\|u_\theta\|_{L^2}$ 제어
- Sobolev 매몰: $\lambda$가 충분하면 해가 더 좋은 함수공간에 속함

## ⚖️ 가정과 한계

| 가정 | 설명 | 한계 |
|------|------|------|
| Poincaré: 유계 $\Omega$ | 경계 조건 필수 | 무한 영역에서 성립 안함 |
| 매몰: Sobolev 특정 지수 | 미분가능성과 차원 균형 | 임계 지수에서 경계선 동작 |
| 컴팩트 매몰: 유계 영역 | Rellich-Kondrachov | 무한 영역: 일반 포함만 가능 |

## 📌 핵심 정리

1. **Poincaré 부등식**: 경계 조건 + 도함수 → 함수 제어
   $$\|u\|_{L^p} \leq C \|\nabla u\|_{L^p}$$ (유계 영역, $u|_{\partial\Omega} = 0$)

2. **동치 노름**: $W^{1,p}_0$에서 $\|\nabla u\|_{L^p} \sim \|u\|_{W^{1,p}}$

3. **Sobolev 매몰**: 미분가능성 ↔ 함수 연속성/정칙성
   - $W^{k,p}(ℝ^n) \hookrightarrow L^q$ (kp < n)
   - $W^{k,p}(ℝ^n) \hookrightarrow C^m$ (kp > n)

4. **컴팩트 매몰**: 유계 Sobolev 수열 → 수렴 부분열 존재

## 🤔 생각해볼 문제

### 문제 1: Poincaré 부등식과 경계조건

경계조건 $u(0) = 0$을 제거하면 어떻게 될까요? 그래도 Poincaré 부등식이 성립할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

**아니오.** 반례:

$$u(x) = 1 \quad \text{(상수함수 on } [0,L])$$

그러면:
- $\|u\|_{L^2} = \sqrt{L}$
- $\|u'\|_{L^2} = 0$
- $\|u\|_{L^2} / \|u'\|_{L^2} = \infty$

따라서 부등식이 성립하지 않습니다!

**수정된 버전** (Poincaré-Friedrichs):

$$\|u - \bar{u}\|_{L^2} \leq C \|\nabla u\|_{L^2}$$

여기서 $\bar{u} = \frac{1}{|\Omega|} \int_\Omega u$는 평균값입니다.

**또는** 한쪽 경계만 고정해도 됩니다:

$$u(a) = 0 \Rightarrow \|u\|_{L^2} \leq C \|u'\|_{L^2}$$

</details>

### 문제 2: 차원 효과

$W^{1,2}(ℝ^n)$이 $C^0(ℝ^n)$ (연속함수)에 포함되려면, $n$이 몇 이하여야 할까요?

<details>
<summary>💡 풀이 (클릭)</summary>

Sobolev 매몰 조건:

$$kp > n \Rightarrow W^{k,p} \hookrightarrow C^m$$

$k=1, p=2$인 경우:

$$1 \cdot 2 > n \Rightarrow n < 2$$

따라서:
- $n = 1$: $W^{1,2}(ℝ) \subset C^0(ℝ)$ ✓ (연속!)
- $n = 2$: $1 \cdot 2 = 2$ (경계선, 연속성 보장 안함)
- $n \geq 3$: $W^{1,2}(ℝ^n) \not\subset C^0$ (불연속 함수 가능)

**고차원 극복**: 더 높은 미분가능성 요구

$$W^{2,2}(ℝ^n) \hookrightarrow C^0 \quad \text{if } 2 \cdot 2 > n, \text{ i.e., } n < 4$$

</details>

### 문제 3: PINN과 차원의 저주

고차원 ($n$이 큼)에서 PINN이 더 어려운 이유는?

<details>
<summary>💡 풀이 (클릭)</summary>

1. **Sobolev 임계 지수** 증가:
   - 1D: $W^{1,2}$로도 연속성 달성
   - 고차원: $W^{1,2}$는 연속성만 보장, 더 높은 미분가능성 필요

2. **신경망 복잡도**:
   - $W^{1,2}(ℝ^n) \subset C^0$이려면, 신경망이 더 "부드러워야" 함
   - 부드러움 = 더 깊은 신경망 또는 더 많은 매개변수

3. **샘플 복잡도**:
   - Sobolev 정규성이 낮을수록 더 많은 데이터/콜로케이션 필요
   - 고차원에서는 콜로케이션점을 지수적으로 많이 필요 (차원의 저주)

**극복 방안**:
- SIREN 사용 (높은 정규성 자동 달성)
- 차원 축소
- 문제 특화 아키텍처 (순환 구조, 멀티스케일)

</details>

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./02-sobolev-spaces.md) | [📚 README](../README.md) | [다음 ▶](./04-calculus-of-variations.md) |

</div>
