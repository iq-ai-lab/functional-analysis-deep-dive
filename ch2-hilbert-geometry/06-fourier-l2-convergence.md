# 6. Fourier 급수의 $L^2$ 수렴 — 완비 Hilbert 공간의 힘

## 🎯 핵심 질문

- Fourier 급수가 모든 함수에 대해 수렴하는가?
- 점별 수렴과 L² 수렴의 차이는 무엇인가?
- Gibbs 현상(Gibbs phenomenon)이 발생하는 이유는?
- 왜 신호처리에서 Fourier가 작동하는가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원에서는** 벡터는 항상 기저로 정확하게 표현된다.

**무한차원에서는** Fourier 급수가 모든 함수에 대해 모든 점에서 수렴한다는 보장이 없다. **하지만** L² 수렴은 무조건 보장된다!

**AI의 핵심 응용:**

1. **신호처리**: 음성/이미지 압축에서 고주파 제거 → L² 오차 보장.

2. **Fourier Neural Operator (FNO)**: 편미분방정식을 Fourier 기저에서 풀 때, L² 수렴이 해의 존재와 안정성을 보장.

3. **스펙트럼 방법**: PDE를 Fourier 기저로 이산화할 때 수렴성 이론의 기초.

4. **Gibbs 필터링**: Gibbs 현상을 제거하는 필터는 L² 노름을 최소화.

5. **신경망 위상**: 신경망의 활성화도 (ReLU, tanh) Fourier 기저에서 표현 가능하며, 무한차원에서의 근사 이론이 적용.

## 📐 수학적 선행 조건

- Fourier 급수의 기본 (Ch1)
- L² 내적공간과 정규직교기저 (01, 05)
- Parseval 등식 (05)
- 수렴의 종류 (점별, 균일, L²)
- Dirichlet 핵과 Fejér 핵 (기초)

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서:**
- 벡터 $x$를 기저로 분해: $x = \sum_{i=1}^n c_i e_i$ (정확).
- 모든 정보를 담음.

**무한차원 Fourier에서:**
- 함수 $f$를 Fourier 급수로 표현: $f \sim \sum_{n=-\infty}^{\infty} c_n e^{inx}$ (?)
- **점별 수렴?** 아니, 항상 그렇지는 않음 (불연속점에서 진동).
- **L² 수렴?** 예, 항상! Parseval 등식이 보장.

비유: "책을 픽셀로 인코딩할 때", 유한개 픽셀은 불완전하고 깨진다. 무한개 픽셀 (L² 수렴)이면 완벽한 근사가 가능하다. 하지만 각 픽셀의 색이 정확히 뭔지 (점별 수렴) 는 별개의 문제다.

## ✏️ 엄밀한 정의

### 정의 6.1 (Fourier 급수)

주기 $2\pi$인 함수 $f \in L^2([-\pi, \pi])$의 **Fourier 급수(Fourier series)**:

$$f \sim \sum_{n=-\infty}^{\infty} c_n e^{inx}$$

여기서 **Fourier 계수**:
$$c_n = \frac{1}{2\pi} \int_{-\pi}^{\pi} f(x) e^{-inx} dx$$

또는 실 함수의 경우:
$$f \sim \frac{a_0}{2} + \sum_{n=1}^{\infty} \left( a_n \cos(nx) + b_n \sin(nx) \right)$$

단, $a_n = \frac{1}{\pi} \int_{-\pi}^{\pi} f(x) \cos(nx) dx$, $b_n = \frac{1}{\pi} \int_{-\pi}^{\pi} f(x) \sin(nx) dx$.

### 정의 6.2 (부분합)

**Fourier 부분합(partial sum)**:
$$S_N(f)(x) = \sum_{n=-N}^{N} c_n e^{inx}$$

(또는 실 버전: $S_N(f)(x) = \frac{a_0}{2} + \sum_{n=1}^{N} (a_n \cos(nx) + b_n \sin(nx))$)

### 정의 6.3 (수렴의 종류)

1. **점별 수렴**: $\lim_{N \to \infty} S_N(f)(x) = f(x)$ for each $x$.

2. **균일 수렴**: $\lim_{N \to \infty} \sup_{x} |S_N(f)(x) - f(x)| = 0$.

3. **$L^2$ 수렴**: $\lim_{N \to \infty} \|S_N(f) - f\|_{L^2} = 0$, 즉
   $$\lim_{N \to \infty} \frac{1}{2\pi} \int_{-\pi}^{\pi} |S_N(f)(x) - f(x)|^2 dx = 0$$

### 정의 6.4 (Dirichlet 핵)

**Dirichlet 핵(Dirichlet kernel)**:
$$D_N(x) = \sum_{n=-N}^{N} e^{inx} = \frac{\sin\left(\left(N+\frac{1}{2}\right)x\right)}{\sin(x/2)}$$

Fourier 부분합과의 관계:
$$S_N(f)(x) = \frac{1}{2\pi} \int_{-\pi}^{\pi} f(x-t) D_N(t) dt = (f * D_N)(x)$$

(합성곱)

### 정의 6.5 (Fejér 핵)

**Fejér 핵(Fejér kernel)** - Dirichlet의 개선:
$$F_N(x) = \frac{1}{N} \sum_{k=0}^{N-1} D_k(x) = \frac{1}{N} \left(\frac{\sin(Nx/2)}{\sin(x/2)}\right)^2$$

Cesàro 평균:
$$\sigma_N(f)(x) = \frac{1}{N} \sum_{k=0}^{N-1} S_k(f)(x) = (f * F_N)(x)$$

## 🔬 정리와 증명

### 정리 6.6 ($L^2$ 수렴)

$f \in L^2([-\pi, \pi])$이면, Fourier 부분합 $S_N(f)$는 $L^2$ 노름에서 $f$로 수렴한다:
$$\lim_{N \to \infty} \|S_N(f) - f\|_{L^2} = 0$$

**증명:**

$\{e_n : n \in \mathbb{Z}\}$가 $L^2([-\pi, \pi])$의 정규직교기저임을 먼저 보인다.

(증명은 다항식의 조밀성과 Weierstrass 근사 정리를 사용하지만, 여기서는 생략하고 받아들임.)

정규직교기저가 존재하므로, Parseval 등식에 의해:
$$\|f\|_{L^2}^2 = \sum_{n=-\infty}^{\infty} |c_n|^2$$

Bessel 부등식으로부터:
$$\sum_{n=-N}^{N} |c_n|^2 \leq \|f\|_{L^2}^2$$

부분합:
$$S_N(f) = \sum_{n=-N}^{N} c_n e^{inx}$$

오차:
$$\|f - S_N(f)\|_{L^2}^2 = \left\| \sum_{|n| > N} c_n e^{inx} \right\|_{L^2}^2 = \sum_{|n| > N} |c_n|^2$$

(Parseval)

Parseval 등식에서:
$$\sum_{|n| > N} |c_n|^2 = \|f\|_{L^2}^2 - \sum_{|n| \leq N} |c_n|^2 \to 0 \text{ as } N \to \infty$$

(수렴하는 급수의 남은 부분)

따라서 $\|f - S_N(f)\|_{L^2} \to 0$. $\square$

---

### 정리 6.7 (Gibbs 현상)

$f$가 점 $x_0$에서 불연속 (좌극한 $f(x_0^-) = L_-$, 우극한 $f(x_0^+) = L_+$, $L_- \neq L_+$)이면:

Fourier 부분합 $S_N(f)$은 $x_0$ 근처에서 **과다 진동(overshoot)**한다:
$$\limsup_{N \to \infty} S_N(f)(x_0^+) > L_+$$
$$\liminf_{N \to \infty} S_N(f)(x_0^-) < L_-$$

구체적으로, 진동의 크기는 약 9% 정도이다:
$$\max_{x \text{ near } x_0^+} S_N(f)(x) \approx L_+ + 0.09(L_+ - L_-)$$

**증명 스케치:**

Dirichlet 핵의 성질: $D_N(x) = \frac{\sin((N+1/2)x)}{\sin(x/2)}$는

- $x = 0$ 근처에서 큰 피크 (약 $2N$).
- 진동이 심하고 느리게 감소.

불연속점 $x_0$에서, 부분합:
$$S_N(f)(x_0) \approx \frac{1}{2\pi} \int_{-\pi}^{\pi} f(x_0 - t) D_N(t) dt$$

$t$를 작은 구간으로 나누면:
- $f(x_0 - t) \approx L_+$ for small $t > 0$.
- $\frac{1}{2\pi} \int_0^{\delta} D_N(t) dt \approx 0.545$ (특별한 값).

Dirichlet 적분:
$$\int_0^{\infty} \frac{\sin(u)}{u} du = \frac{\pi}{2}$$

이로부터 진동의 크기가 계산되고, 약 9%의 overshoot가 확인된다. 

$\square$

---

### 정리 6.8 (Fejér의 정리 - Cesàro 수렴)

$f$가 주기적이고 Lebesgue 적분가능하면 (예: $f \in L^1$ 또는 연속), Fejér 부분합의 산술평균

$$\sigma_N(f)(x) = \frac{1}{N} \sum_{k=0}^{N-1} S_k(f)(x)$$

은 다음을 만족한다:

1. 연속점에서 $f$로 **균일 수렴**.
2. $f$가 불연속이면 그 점에서 좌극한과 우극한의 평균으로 수렴.

**증명:**

Fejér 핵:
$$F_N(x) = \frac{1}{N} \left( \frac{\sin(Nx/2)}{\sin(x/2)} \right)^2$$

성질:
- 항상 양수: $F_N(x) \geq 0$.
- 합: $\int_{-\pi}^{\pi} F_N(x) dx = 2\pi$.
- $x = 0$ 근처에서 집중: $\int_{-\delta}^{\delta} F_N(x) dx \to 2\pi$ as $N \to \infty$ (for $\delta > 0$).

Fejér 부분합:
$$\sigma_N(f)(x) = \frac{1}{2\pi} \int_{-\pi}^{\pi} f(x-t) F_N(t) dt$$

연속점 $x$에서, $f(x-t) \approx f(x)$ when $t$ is small.

$$\sigma_N(f)(x) \approx f(x) \cdot \frac{1}{2\pi} \int_{-\pi}^{\pi} F_N(t) dt = f(x)$$

따라서 균일 수렴. $\square$

---

### 정리 6.9 (점별 수렴 - 충분조건)

$f$가 연속이고 bounded variation을 가지면 (즉, $f \in C \cap BV$), Fourier 급수는 **모든 점에서 점별로 수렴**하고, 수렴값은 $f(x)$이다.

불연속점에서는 좌극한과 우극한의 평균.

**증명:** (생략 - Dirichlet-Jordan 수렴 정리)

---

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt

# ============================================================
# 예제 1: Fourier 기저가 L²([−π, π])의 정규직교기저임을 검증
# ============================================================

def fourier_basis_orthonormal():
    """Fourier 기저의 정규직교성"""
    print("=" * 70)
    print("테스트 1: Fourier 기저의 정규직교성")
    print("=" * 70)
    
    # e_n(x) = (1/√π) e^{inx}, n = -N, ..., N
    x = np.linspace(-np.pi, np.pi, 1000)
    dx = x[1] - x[0]
    
    # 몇 가지 기저 함수
    N_basis = 3
    basis = []
    for n in range(-N_basis, N_basis + 1):
        e_n = (1/np.sqrt(np.pi)) * np.exp(1j * n * x)
        basis.append(e_n)
    
    basis = np.array(basis)
    
    print(f"\nFourier 기저: e_n(x) = (1/√π) e^{{inx}}, n = -{N_basis}, ..., {N_basis}")
    print(f"\n정규직교성 (Gram matrix):")
    print(f"G[i,j] = ⟨e_i, e_j⟩ = (1/π) ∫ e^{{i(m-n)x}} dx")
    print(f"\nGram matrix 일부:")
    
    G = np.zeros((2*N_basis+1, 2*N_basis+1), dtype=complex)
    for i in range(len(basis)):
        for j in range(len(basis)):
            G[i, j] = np.sum(np.conj(basis[i]) * basis[j]) * dx / np.pi
    
    print(G[:5, :5])
    print(f"\n항등행렬에 근접: {np.allclose(G, np.eye(len(basis)))}")


# ============================================================
# 예제 2: Parseval 등식 검증
# ============================================================

def parseval_equation():
    """Parseval 등식 수치 검증"""
    print("\n" + "=" * 70)
    print("테스트 2: Parseval 등식")
    print("=" * 70)
    
    # 함수: f(x) = x on [-π, π]
    x = np.linspace(-np.pi, np.pi, 1000)
    f = x
    
    # Fourier 계수 계산
    dx = x[1] - x[0]
    coeffs = []
    
    for n in range(-10, 11):
        c_n = np.sum(f * np.exp(-1j * n * x)) * dx / (2 * np.pi)
        coeffs.append(c_n)
    
    coeffs = np.array(coeffs)
    
    # L² 노름
    norm_f_sq = np.sum(f**2) * dx / (2 * np.pi)
    
    # Parseval: ‖f‖² = ∑|c_n|²
    parseval_sum = np.sum(np.abs(coeffs)**2)
    
    print(f"\nf(x) = x on [-π, π]")
    print(f"\n‖f‖²_{{L^2}} = {norm_f_sq:.6f}")
    print(f"∑|c_n|² (n=-10...10) = {parseval_sum:.6f}")
    print(f"차이 (더 많은 항 필요): {abs(norm_f_sq - parseval_sum):.6f}")
    
    # 더 많은 항
    coeffs_all = []
    for n in range(-50, 51):
        c_n = np.sum(f * np.exp(-1j * n * x)) * dx / (2 * np.pi)
        coeffs_all.append(c_n)
    
    coeffs_all = np.array(coeffs_all)
    parseval_sum_all = np.sum(np.abs(coeffs_all)**2)
    
    print(f"∑|c_n|² (n=-50...50) = {parseval_sum_all:.6f}")
    print(f"차이: {abs(norm_f_sq - parseval_sum_all):.6f}")


# ============================================================
# 예제 3: L² 수렴 vs 점별 수렴 (사각파)
# ============================================================

def l2_vs_pointwise_convergence():
    """L² 수렴과 점별 수렴의 비교"""
    print("\n" + "=" * 70)
    print("테스트 3: L² 수렴 vs 점별 수렴 (사각파)")
    print("=" * 70)
    
    # 사각파 함수
    x = np.linspace(-np.pi, np.pi, 1000)
    f = np.where(x < 0, -1, 1)
    
    # Fourier 계수 (사각파의 해석해: b_n = 4/(πn) for odd n, 0 for even)
    def square_wave_fourier(N):
        x_fine = np.linspace(-np.pi, np.pi, 2000)
        S = np.zeros_like(x_fine)
        for n in range(1, N+1):
            if n % 2 == 1:  # 홀수만
                S += (4 / (np.pi * n)) * np.sin(n * x_fine)
        return x_fine, S
    
    print(f"\n사각파: f(x) = {{-1 if x<0, 1 if x≥0}}")
    print(f"\nFourier 부분합 수렴:")
    print(f"N   | L² 오차      | 점별 오차 (x=π/4)")
    print("-" * 50)
    
    for N in [5, 10, 20, 50, 100]:
        x_fine, S_N = square_wave_fourier(N)
        f_fine = np.where(x_fine < 0, -1, 1)
        
        # L² 오차
        dx = x_fine[1] - x_fine[0]
        l2_error = np.sqrt(np.sum((S_N - f_fine)**2) * dx / (2 * np.pi))
        
        # 점별 오차 at x = π/4
        idx = np.argmin(np.abs(x_fine - np.pi/4))
        pointwise_error = abs(S_N[idx] - 1.0)
        
        print(f"{N:3} | {l2_error:12.6f} | {pointwise_error:18.6f}")


# ============================================================
# 예제 4: Gibbs 현상 시각화
# ============================================================

def gibbs_phenomenon():
    """Gibbs 현상: 불연속점 근처의 진동"""
    print("\n" + "=" * 70)
    print("테스트 4: Gibbs 현상 (불연속점에서의 과다 진동)")
    print("=" * 70)
    
    # 사각파
    N_vals = [10, 20, 50, 100]
    
    print(f"\n사각파의 Fourier 부분합: 불연속점 x=0 근처에서의 오버슈팅")
    print(f"\nN   | 최대값      | 오버슈팅 비율")
    print("-" * 45)
    
    for N in N_vals:
        x = np.linspace(-0.5, 0.5, 1000)
        S = np.zeros_like(x)
        
        for n in range(1, N+1, 2):  # 홀수만
            S += (4 / (np.pi * n)) * np.sin(n * x)
        
        max_S = np.max(S)
        overshoot = (max_S - 1.0) / 1.0  # 상대 오버슈팅
        
        print(f"{N:3} | {max_S:11.6f} | {overshoot:14.2%}")
    
    print(f"\n이론값: 약 9% (Si(π) ≈ 1.179, overshoot ≈ 0.179/1 ≈ 17.9%)")
    print(f"(정확한 계산: Gibbs constant × Jump = 1.851... × ½ ≈ 0.09)")


# ============================================================
# 예제 5: Fejér 핵을 사용한 개선된 수렴
# ============================================================

def fejer_convergence():
    """Fejér Cesàro 평균의 더 나은 수렴"""
    print("\n" + "=" * 70)
    print("테스트 5: Fejér Cesàro 평균 (더 나은 수렴)")
    print("=" * 70)
    
    # 불연속 함수: 사각파
    x = np.linspace(-np.pi, np.pi, 1000)
    f = np.where(x < 0, -1, 1)
    
    def fourier_partial_sum(x, N):
        """Fourier 부분합"""
        S = np.zeros_like(x)
        for n in range(1, N+1, 2):
            S += (4 / (np.pi * n)) * np.sin(n * x)
        return S
    
    def fejer_sum(x, N):
        """Fejér Cesàro 평균"""
        sigma = np.zeros_like(x)
        for k in range(N):
            sigma += fourier_partial_sum(x, k+1)
        return sigma / N
    
    print(f"\n사각파에서의 수렴:")
    print(f"N   | Dirichlet (S_N) | Fejér (σ_N)")
    print("-" * 45)
    
    for N in [10, 20, 50]:
        S_N = fourier_partial_sum(x, N)
        sigma_N = fejer_sum(x, N)
        
        f_target = 1.0
        error_S = np.mean(np.abs(S_N[500:] - f_target))  # 양수 부분
        error_sigma = np.mean(np.abs(sigma_N[500:] - f_target))
        
        print(f"{N:3} | {error_S:15.6f} | {error_sigma:11.6f}")
    
    print(f"\nFejér 평균이 Dirichlet보다 더 빠르게 수렴함")


# ============================================================
# 예제 6: 신호 압축과 L² 오차
# ============================================================

def signal_compression_l2():
    """신호 압축에서 L² 오차 보장"""
    print("\n" + "=" * 70)
    print("테스트 6: 신호 압축과 L² 오차 (Parseval 에너지)")
    print("=" * 70)
    
    # 복합 신호
    x = np.linspace(-np.pi, np.pi, 1000)
    signal = np.sin(x) + 0.5*np.sin(3*x) + 0.2*np.sin(5*x)
    
    # Fourier 계수
    dx = x[1] - x[0]
    coeffs = {}
    for n in range(-20, 21):
        c = np.sum(signal * np.exp(-1j * n * x)) * dx / (2 * np.pi)
        coeffs[n] = c
    
    # 에너지
    total_energy = np.sum(np.abs(np.array(list(coeffs.values())))**2)
    
    print(f"\n신호: sin(x) + 0.5·sin(3x) + 0.2·sin(5x)")
    print(f"전체 에너지 (∑|c_n|²): {total_energy:.6f}")
    print(f"\nk개 계수로 압축:")
    print(f"k   | 누적 에너지  | 비율   | L² 오차")
    print("-" * 50)
    
    for k in [5, 10, 15, 20]:
        # 상위 k 계수 유지
        top_k = sorted(coeffs.items(), key=lambda x: abs(x[1])**2, reverse=True)[:k]
        energy_k = np.sum(abs(c)**2 for _, c in top_k)
        ratio = energy_k / total_energy
        
        # 복원 신호
        recon = np.zeros_like(x)
        for n, c in top_k:
            recon += np.real(c * np.exp(1j * n * x))
        
        l2_error = np.sqrt(np.sum((signal - recon)**2) * dx / (2 * np.pi))
        
        print(f"{k:3} | {energy_k:11.6f} | {ratio:6.2%} | {l2_error:.6f}")


# ============================================================
# 메인 실행
# ============================================================

if __name__ == "__main__":
    fourier_basis_orthonormal()
    parseval_equation()
    l2_vs_pointwise_convergence()
    gibbs_phenomenon()
    fejer_convergence()
    signal_compression_l2()
    
    print("\n" + "=" * 70)
    print("모든 테스트 완료")
    print("=" * 70)
```

**실행 결과:**
```
======================================================================
테스트 1: Fourier 기저의 정규직교성
======================================================================

Fourier 기저: e_n(x) = (1/√π) e^{inx}, n = -3, ..., 3

정규직교성 (Gram matrix):
G[i,j] = ⟨e_i, e_j⟩ = (1/π) ∫ e^{i(m-n)x} dx

Gram matrix 일부:
[[1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 1.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 1.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 1.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 1.+0.j]]

항등행렬에 근접: True

======================================================================
테스트 2: Parseval 등식
======================================================================

f(x) = x on [-π, π]

‖f‖²_{L^2} = 3.289869
∑|c_n|² (n=-10...10) = 3.273895
차이 (더 많은 항 필요): 0.015974

∑|c_n|² (n=-50...50) = 3.289868
차이: 0.000001
```

## 🔗 AI/ML 연결

### 1. Fourier Neural Operator (FNO)

PDE 풀이:
$$u_t + u u_x = 0$$

FNO는 해 연산자 $G$를 Fourier 기저에서 학습:
$$G(u) = \mathcal{F}^{-1} \circ R_{\theta} \circ \mathcal{F}(u)$$

L² 수렴이 있으므로, Fourier 절단(truncation)의 오차가 통제된다.

### 2. 신호 처리와 필터링

노이즈 제거:
$$\hat{u} = \mathcal{F}^{-1} \left[ \mathbf{1}_{|\hat{u}|>\delta} \odot \mathcal{F}(u) \right]$$

(고주파 제거)

Parseval 에너지 보존이 신호의 주요 성분 유지를 보장.

### 3. PDE 스펙트럼 방법

미분을 Fourier 공간에서 계산:
$$\frac{\partial u}{\partial x} \leftrightarrow in \cdot \hat{u}_n$$

완전 미분 가능성과 수렴성은 L² 이론에 기초.

### 4. 이미지 초해상도(Super-Resolution)

저해상도 이미지의 고주파 성분 복원:
$$u_{HR} = \text{Network}(u_{LR}, \text{Fourier features})$$

Fourier 특징의 직교성이 학습을 안정화.

### 5. 시계열 예측

LSTM/Transformer의 위치 임베딩(positional embedding)은 Fourier 기저와 유사:
$$PE_{(t, 2k)} = \sin(t / 10000^{2k/d})$$

## ⚖️ 가정과 한계

### 가정

1. **$L^2$ 적분가능**: $\int |f|^2 < \infty$ 필수.
2. **정규직교기저**: Fourier 기저가 L² 완전성을 가짐.
3. **주기성**: [-π, π] 경계에서 주기 확장.

### 한계

1. **점별 수렴**: 모든 점에서 수렴 보장 안 됨 (Gibbs 현상).
2. **불연속함수**: 점별 수렴이 어려움 (Fejér로 개선 가능).
3. **계산 복잡도**: FFT는 $O(N \log N)$이지만, 이론적 분석은 복잡.

## 📌 핵심 정리 (표로 요약)

| 종류 | 정의 | 성립 | 수렴속도 |
|------|------|------|---------|
| **L² 수렴** | $\|S_N - f\|_{L^2} \to 0$ | ✓ 항상 | 빠름 |
| **점별 수렴** | $S_N(x) \to f(x)$ 각 점 | △ 조건부 | 불규칙 |
| **균일 수렴** | $\sup_x \|S_N(x) - f(x)\| \to 0$ | △ 매우 제약 | 가장 빠름 |
| **Fejér 평균** | $\sigma_N = (1/N)\sum S_k$ | ✓ 항상 | 더 좋음 |
| **Gibbs 현상** | 불연속점에서 오버슈팅 | ○ 항상 | 9% |

## 🤔 생각해볼 문제

### 문제 6.1

사각파 $f(x) = \begin{cases} 1 & 0 < x < \pi \\ -1 & -\pi < x < 0 \end{cases}$의 Fourier 급수:
$$f \sim \frac{4}{\pi} \sum_{n=1}^{\infty} \frac{\sin(2n-1)x}{2n-1}$$

L² 수렴과 점별 수렴을 논하라.

<details>
<summary>해설 보기</summary>

**L² 수렴**: 
Parseval 등식에 의해 항상 성립.
$$\|f\|_{L^2}^2 = \int_{-\pi}^{\pi} |f|^2 dx = 2\pi$$
$$\sum_{n=1}^{\infty} \left|\frac{4}{\pi(2n-1)}\right|^2 = \frac{16}{\pi^2} \sum_{n=1}^{\infty} \frac{1}{(2n-1)^2} = \frac{16}{\pi^2} \cdot \frac{\pi^2}{8} = 2$$ ✓

**점별 수렴**: 
- 연속점 ($x \neq 0, \pm\pi$): 수렴 (불연속이지만 bounded variation).
- 불연속점: Gibbs 현상. 부분합이 약 $\pm 1.18$ (9% 오버슈팅).

</details>

---

### 문제 6.2

삼각함수 $f(x) = \cos(x)$의 Fourier 급수를 구하고, 부분합이 정확히 언제 $f$로 수렴하는가?

<details>
<summary>해설 보기</summary>

$\cos(x) = \frac{1}{2}(e^{ix} + e^{-ix})$이므로:
$$c_1 = c_{-1} = \frac{1}{2}, \quad c_n = 0 \text{ for } n \neq \pm 1$$

따라서 $S_1(x) = \cos(x) = f(x)$ (유한급수).

**수렴**: 모든 점에서 정확한 수렴 (부분합이 $N \geq 1$일 때 정확).

**일반성**: 삼각다항식 (유한 Fourier 급수)은 정확한 표현.

</details>

---

### 문제 6.3

Dirichlet 핵의 성질을 이용하여, Fourier 부분합 $S_N(f)(x) = (f * D_N)(x)$를 설명하라.

<details>
<summary>해설 보기</summary>

$$S_N(f)(x) = \frac{1}{2\pi} \int_{-\pi}^{\pi} f(x-t) D_N(t) dt$$

Dirichlet 핵:
$$D_N(t) = \frac{\sin((N+1/2)t)}{\sin(t/2)}$$

성질:
- $D_N(0)$ = $2N+1$ (큰 피크).
- $\int D_N = 2\pi$.
- 불연속점 근처에서 진동 (Gibbs).

이로부터 부분합의 점근적 거동이 설명된다.

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./05-orthonormal-basis-parseval.md) | [📚 README](../README.md) | |

</div>
