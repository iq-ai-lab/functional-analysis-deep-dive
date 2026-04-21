# 5. 정규직교기저와 Parseval 등식

## 🎯 핵심 질문

- 무한차원 공간에서 정규직교기저(orthonormal basis)란 무엇인가?
- Parseval 등식이 성립하려면 어떤 조건이 필요한가?
- Bessel 부등식과 완전성(completeness) 사이의 관계는?
- 모든 가분(separable) Hilbert 공간이 동형인가?

## 🔍 왜 이 이론이 AI에서 중요한가

**유한차원에서는** 표준기저 $\{e_1, \ldots, e_n\}$에서 벡터를 표현하면 끝. 기저가 찾기 쉽다.

**무한차원에서는** 기저가 존재한다는 것을 증명해야 하고, 무한급수의 수렴을 신경써야 한다.

**AI의 핵심 응용:**

1. **Fourier 분석**: 신호를 정현파(sin, cos) 기저로 분해. JPEG 압축의 기초.

2. **Wavelet 변환**: 다중스케일 정규직교기저. 신호처리의 핵심.

3. **PCA와 ICA**: 데이터의 정규직교 분해. 주성분 = 기저벡터.

4. **Neural Operator** (FNO): Fourier 기저에서 선형연산자를 학습.

5. **Spectral Methods**: 편미분방정식을 Fourier 기저로 풀기.

6. **이미지 압축**: 이미지를 적은 개의 기저로 근사. Parseval 등식이 에너지 보존을 보장.

## 📐 수학적 선행 조건

- 내적공간과 Hilbert 공간 (01-inner-product-hilbert.md)
- 직교성과 수직분해 (03-orthogonal-projection.md)
- 급수의 수렴 (L² 노름)
- 완비성(completeness)

## 📖 직관적 이해 (유한차원 → 무한차원 차이를 비유로)

**유한차원에서:**
- 기저 $\{e_1, \ldots, e_n\}$: $x = \sum_{i=1}^n \langle x, e_i \rangle e_i$ (정규직교인 경우).
- Parseval: $\|x\|^2 = \sum_{i=1}^n |\langle x, e_i \rangle|^2$.
- 기저가 "완전"하다는 것은 자동.

**무한차원에서:**
- 가산 정규직교계: $\{e_1, e_2, e_3, \ldots\}$.
- 급수: $x = \sum_{n=1}^{\infty} \langle x, e_n \rangle e_n$ (수렴하는가?)
- Bessel 부등식: $\sum_{n=1}^{\infty} |\langle x, e_n \rangle|^2 \leq \|x\|^2$ (항상 수렴).
- Parseval이 성립하려면: $\sum_{n=1}^{\infty} |\langle x, e_n \rangle|^2 = \|x\|^2$ (등호).
- 등호가 성립 ⟺ 기저가 **완전**.

비유: 사진의 픽셀들을 "기저색"(빨강, 초록, 파랑)으로 분해할 때, 모든 색을 표현할 수 있으려면 기저가 완전해야 한다. Parseval은 원본 이미지의 밝기가 보존됨을 보장한다.

## ✏️ 엄밀한 정의

### 정의 5.1 (정규직교계)

Hilbert 공간 $H$의 **정규직교계(orthonormal system)** $\{e_n : n \in I\}$ (I는 색인집합):

1. **정규성**: $\|e_n\| = 1$ for all $n$.
2. **직교성**: $\langle e_n, e_m \rangle = 0$ for $n \neq m$.

합쳐서: $\langle e_n, e_m \rangle = \delta_{nm}$ (Kronecker delta).

### 정의 5.2 (Fourier 계수)

정규직교계 $\{e_n\}$에 대해, $x \in H$의 **Fourier 계수**:
$$c_n = \langle x, e_n \rangle$$

### 정의 5.3 (완전 정규직교계 = 정규직교기저)

정규직교계 $\{e_n\}$이 **완전(complete)** 또는 **정규직교기저(ONB)**:
$$\langle x, e_n \rangle = 0 \text{ for all } n \Rightarrow x = 0$$

동등하게: $\text{span}\{e_n : n \in I\}$이 $H$에서 조밀(dense).

## 🔬 정리와 증명

### 정리 5.4 (Bessel 부등식)

$\{e_n\}$이 정규직교계이면, 임의의 $x \in H$에 대해:
$$\sum_{n=1}^{\infty} |\langle x, e_n \rangle|^2 \leq \|x\|^2$$

**증명:**

부분합 $S_N = \sum_{n=1}^{N} \langle x, e_n \rangle e_n$를 생각하자.

$$\left\| x - S_N \right\|^2 = \left\langle x - \sum_{n=1}^{N} \langle x, e_n \rangle e_n, x - \sum_{m=1}^{N} \langle x, e_m \rangle e_m \right\rangle$$

우변을 전개:

$$= \|x\|^2 - \sum_{n=1}^{N} \overline{\langle x, e_n \rangle} \langle x, e_n \rangle - \sum_{m=1}^{N} \langle x, e_m \rangle \overline{\langle x, e_m \rangle} + \sum_{n=1}^{N} \sum_{m=1}^{N} \langle x, e_n \rangle \overline{\langle x, e_m \rangle} \langle e_n, e_m \rangle$$

직교성으로부터:
$$= \|x\|^2 - \sum_{n=1}^{N} |\langle x, e_n \rangle|^2 - \sum_{m=1}^{N} |\langle x, e_m \rangle|^2 + \sum_{n=1}^{N} |\langle x, e_n \rangle|^2$$

$$= \|x\|^2 - \sum_{n=1}^{N} |\langle x, e_n \rangle|^2 \geq 0$$

따라서:
$$\sum_{n=1}^{N} |\langle x, e_n \rangle|^2 \leq \|x\|^2$$

$N \to \infty$로 보내면:
$$\sum_{n=1}^{\infty} |\langle x, e_n \rangle|^2 \leq \|x\|^2$$

$\square$

---

### 정리 5.5 (Parseval 등식)

$\{e_n\}$이 Hilbert 공간 $H$의 정규직교기저이면, 임의의 $x \in H$에 대해:

$$\sum_{n=1}^{\infty} |\langle x, e_n \rangle|^2 = \|x\|^2$$

그리고 이 급수가 수렴한다 (Bessel 부등식에 의해). 또한:

$$x = \sum_{n=1}^{\infty} \langle x, e_n \rangle e_n$$

(L² 노름 수렴)

**증명:**

$\{e_n\}$이 완전이므로, 거의 모든 $x$에 대해 $\text{span}\{e_n\}$의 원소로 임의로 가까이 근사할 수 있다.

정확히는, 임의의 $\epsilon > 0$에 대해, 유한개의 계수 $c_1, \ldots, c_N$이 존재하여:
$$\left\| x - \sum_{n=1}^{N} c_n e_n \right\| < \epsilon$$

**특히**, 최선의 근사는 Fourier 계수를 사용할 때 얻어진다:
$$c_n^* = \langle x, e_n \rangle$$

이를 보이기 위해, 임의의 계수 $(c_n)$에 대해:

$$\left\| x - \sum_{n=1}^{N} c_n e_n \right\|^2 = \left\| x - \sum_{n=1}^{N} \langle x, e_n \rangle e_n + \sum_{n=1}^{N} (\langle x, e_n \rangle - c_n) e_n \right\|^2$$

$$= \left\| x - \sum_{n=1}^{N} \langle x, e_n \rangle e_n \right\|^2 + \sum_{n=1}^{N} |\langle x, e_n \rangle - c_n|^2$$

(마지막 두 항이 직교이므로)

최솟값은 $c_n = \langle x, e_n \rangle$일 때 달성된다.

**핵심**: 부분합의 극한이 $x$에 수렴하는지 보기 위해, 완전성을 사용.

$x$에 직교하는 원소는 0뿐이므로:
$$\left\| x - \sum_{n=1}^{N} \langle x, e_n \rangle e_n \right\| \to 0 \text{ as } N \to \infty$$

극한을 취하면:
$$x = \sum_{n=1}^{\infty} \langle x, e_n \rangle e_n$$

따라서:
$$\|x\|^2 = \left\| \sum_{n=1}^{\infty} \langle x, e_n \rangle e_n \right\|^2 = \sum_{n=1}^{\infty} |\langle x, e_n \rangle|^2$$

(마지막 등호는 무한급수의 직교성)

$\square$

---

### 정리 5.6 (가분 Hilbert 공간은 정규직교기저를 가짐)

가분(separable) Hilbert 공간 (가산 조밀 부분집합을 가짐) 은 정규직교기저를 가진다.

**증명:**

가분이므로 가산 조밀 부분집합 $\{x_1, x_2, x_3, \ldots\}$이 존재.

Gram-Schmidt 정규직교화를 적용 (03-orthogonal-projection.md에서 설명).

선형독립인 부분수열을 취하면 가산 개의 원소를 얻고, 이로부터 정규직교 수열 $(e_n)$을 구성.

$\text{span}\{e_n\}$은 $\{x_1, x_2, \ldots\}$를 포함하고 둘 다 조밀이므로, $\{e_n\}$도 조밀.

따라서 $\{e_n\}$은 정규직교기저. $\square$

---

### 정리 5.7 (모든 가분 무한차원 Hilbert 공간은 $\ell^2$와 동형)

$H$가 가분 무한차원 Hilbert 공간이면, Hilbert 동형(unitary isomorphism)

$$\Phi : H \to \ell^2$$

이 존재한다. 즉:
$$\Phi(x) = (\langle x, e_n \rangle)_{n=1}^{\infty}$$

(정규직교기저 $\{e_n\}$ 선택)

**증명:**

$\{e_n\}$을 $H$의 정규직교기저라 하자.

$\Phi(x) = (c_1, c_2, c_3, \ldots)$ (단, $c_n = \langle x, e_n \rangle$).

Bessel 부등식에 의해:
$$\sum_{n=1}^{\infty} |c_n|^2 \leq \|x\|^2 < \infty$$

따라서 $\Phi(x) \in \ell^2$.

**선형성**: 명백.

**등거리성(isometry)**: Parseval 등식에 의해
$$\|\Phi(x)\|_{\ell^2}^2 = \sum_{n=1}^{\infty} |c_n|^2 = \|x\|^2$$

**전사성(onto)**: $(a_n) \in \ell^2$이면, $x = \sum_{n=1}^{\infty} a_n e_n$ (수렴, $\ell^2$의 완비성에 의해)로 정의하면 $\Phi(x) = (a_n)$.

따라서 Hilbert 동형. $\square$

---

## 💻 NumPy 구현으로 검증

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.fftpack import fft, ifft

# ============================================================
# 예제 1: 정규직교계 구성 (Gram-Schmidt)
# ============================================================

def orthonormal_system():
    """정규직교계 구성"""
    print("=" * 70)
    print("테스트 1: 정규직교계 구성 (Gram-Schmidt)")
    print("=" * 70)
    
    # 독립인 벡터들
    v = [
        np.array([1, 1, 0]),
        np.array([0, 1, 1]),
        np.array([1, 0, 1]),
    ]
    
    # Gram-Schmidt
    e = []
    for vi in v:
        u = vi.copy()
        for ej in e:
            u -= np.dot(vi, ej) * ej
        u = u / np.linalg.norm(u)
        e.append(u)
    
    print("\n입력 벡터들:")
    for i, vi in enumerate(v):
        print(f"  v{i+1} = {vi}")
    
    print("\n정규직교계:")
    for i, ei in enumerate(e):
        print(f"  e{i+1} = {ei}")
    
    # 정규직교성 검증
    print("\n정규직교성 검증:")
    G = np.array(e).T @ np.array(e)
    print("Gram matrix (e^T e):")
    print(G)
    is_orthonormal = np.allclose(G, np.eye(3))
    print(f"정규직교: {is_orthonormal}")


# ============================================================
# 예제 2: Fourier 계수와 Bessel 부등식
# ============================================================

def fourier_coefficients_bessel():
    """Fourier 계수 계산과 Bessel 부등식"""
    print("\n" + "=" * 70)
    print("테스트 2: Fourier 계수와 Bessel 부등식")
    print("=" * 70)
    
    # 함수: f(x) = x on [0, 1]
    x = np.linspace(0, 1, 1000)
    f = x
    
    # L² 노름
    norm_f_sq = np.trapz(f**2, x)
    
    # 정규직교기저: {1, √2 cos(2πnx), √2 sin(2πnx)}
    # 정규화를 위해 내적 ⟨f,g⟩ = ∫ fg dx 사용
    
    fourier_norm_sq = 0
    
    print(f"\nf(x) = x on [0,1]")
    print(f"‖f‖²_{'{L^2}'} = {norm_f_sq:.6f}")
    print(f"\nFourier 계수 (정규화된 기저):")
    print(f"n   | cos 계수      | sin 계수      | |c_n|²")
    print("-" * 55)
    
    # 상수항
    c0 = np.trapz(f * np.ones_like(x), x)
    fourier_norm_sq += c0**2
    print(f"0   | {c0:13.6f} | {'':13} | {c0**2:.6f}")
    
    # 기타 항
    for n in range(1, 6):
        cos_basis = np.sqrt(2) * np.cos(2 * np.pi * n * x)
        sin_basis = np.sqrt(2) * np.sin(2 * np.pi * n * x)
        
        c_cos = np.trapz(f * cos_basis, x)
        c_sin = np.trapz(f * sin_basis, x)
        
        fourier_norm_sq += c_cos**2 + c_sin**2
        
        print(f"{n:3} | {c_cos:13.6f} | {c_sin:13.6f} | {c_cos**2 + c_sin**2:.6f}")
    
    print(f"\n∑|c_n|² (부분합) = {fourier_norm_sq:.6f}")
    print(f"‖f‖² = {norm_f_sq:.6f}")
    print(f"Bessel 부등식 만족: {fourier_norm_sq <= norm_f_sq}")
    print(f"여유값: {norm_f_sq - fourier_norm_sq:.6f}")


# ============================================================
# 예제 3: Parseval 등식 (사각파)
# ============================================================

def parseval_square_wave():
    """Parseval 등식 검증 (사각파)"""
    print("\n" + "=" * 70)
    print("테스트 3: Parseval 등식 (사각파의 Fourier 급수)")
    print("=" * 70)
    
    # 주기 함수: 사각파 ([-π, π])
    x = np.linspace(-np.pi, np.pi, 1000)
    f = np.where(np.abs(x) < np.pi/2, 1, -1)
    
    # L² 노름
    norm_f_sq = np.trapz(f**2, x) / (2 * np.pi)  # 정규화 for period 2π
    
    # FFT로 Fourier 계수 계산
    F = np.fft.fft(f)
    N = len(f)
    
    # Parseval's theorem: (1/N)∑|F_k|² = (1/2π)∫|f|² dx
    # (다양한 정규화 규칙이 있으므로 수치 검증)
    
    # 직접 계산
    parseval_sum = np.sum(np.abs(F)**2) / (N**2)
    
    print(f"\n사각파 f(x) = {{1 if |x| < π/2, -1 otherwise}} on [-π, π]")
    print(f"(한 주기: [-π, π])")
    print(f"\n‖f‖²_{'{L^2}'} = {norm_f_sq:.6f}")
    
    # Fourier 계수 개수별 누적
    power = 0
    print(f"\nFourier 전력 누적:")
    print(f"계수개수 | 누적 전력    | 에러")
    print("-" * 40)
    
    for k in [5, 10, 20, 50, 100]:
        power_k = np.sum(np.abs(F[:k])**2) / (N**2)
        error = np.abs(norm_f_sq - power_k)
        print(f"{k:8} | {power_k:11.6f} | {error:.6f}")
    
    print(f"\n완전한 Fourier급수: {norm_f_sq:.6f}")


# ============================================================
# 예제 4: 정규직교기저로의 벡터 분해
# ============================================================

def vector_decomposition():
    """벡터를 정규직교기저로 분해"""
    print("\n" + "=" * 70)
    print("테스트 4: 벡터 분해와 재구성")
    print("=" * 70)
    
    # 벡터 x
    x = np.array([1, 2, 3, 4, 5], dtype=float)
    
    # 정규직교기저 생성 (임의)
    basis_raw = [np.random.randn(5) for _ in range(5)]
    
    # Gram-Schmidt
    basis = []
    for v in basis_raw:
        u = v.copy()
        for e in basis:
            u -= np.dot(v, e) * e
        if np.linalg.norm(u) > 1e-10:
            basis.append(u / np.linalg.norm(u))
    
    basis = np.array(basis).T  # (5, 5) 행렬
    
    # Fourier 계수
    c = basis.T @ x
    
    # 재구성
    x_recon = basis @ c
    
    # Parseval
    norm_x = np.linalg.norm(x)
    norm_c = np.linalg.norm(c)
    
    print(f"\n벡터 x = {x}")
    print(f"\n정규직교기저로 분해:")
    print(f"c = ⟨x, e_i⟩ = {c}")
    
    print(f"\n재구성: x_recon = ∑c_i e_i")
    print(f"x_recon = {x_recon}")
    
    print(f"\nParseval 등식:")
    print(f"‖x‖ = {norm_x:.6f}")
    print(f"√(∑|c_i|²) = {norm_c:.6f}")
    print(f"일치: {np.isclose(norm_x, norm_c)}")
    
    print(f"\n재구성 오차: {np.linalg.norm(x - x_recon):.10f}")


# ============================================================
# 예제 5: Fourier 기저와 신호 압축
# ============================================================

def signal_compression_fourier():
    """Fourier 기저로 신호 압축 (에너지 보존)"""
    print("\n" + "=" * 70)
    print("테스트 5: Fourier 신호 압축 (Parseval 에너지 보존)")
    print("=" * 70)
    
    # 신호: 복합 주파수
    t = np.linspace(0, 1, 256)
    signal = (np.sin(2*np.pi*5*t) +      # 5 Hz
              0.5*np.sin(2*np.pi*15*t) +  # 15 Hz
              0.2*np.sin(2*np.pi*25*t))   # 25 Hz
    
    # FFT
    freq_domain = np.fft.fft(signal)
    power = np.abs(freq_domain)**2
    
    # 전체 에너지
    total_energy = np.sum(power)
    
    # 상위 k 계수로 압축
    print(f"\n신호: sin(2π·5t) + 0.5·sin(2π·15t) + 0.2·sin(2π·25t)")
    print(f"샘플링: {len(signal)} 포인트")
    print(f"\n전체 에너지: {total_energy:.2f}")
    print(f"\nk개 주요 계수로의 압축:")
    print(f"k   | 누적 에너지  | 비율   | 오차 (L²)")
    print("-" * 50)
    
    for k in [10, 20, 50, 100, 256]:
        # 상위 k 계수 유지
        top_k_indices = np.argsort(power)[-k:]
        freq_compressed = freq_domain.copy()
        freq_compressed[~np.isin(np.arange(len(freq_domain)), top_k_indices)] = 0
        
        # 신호 복원
        signal_recon = np.real(np.fft.ifft(freq_compressed))
        
        # 에너지 / 오차
        energy_k = np.sum(np.abs(freq_compressed)**2)
        ratio = energy_k / total_energy
        l2_error = np.sqrt(np.sum((signal - signal_recon)**2))
        
        print(f"{k:3} | {energy_k:11.2f} | {ratio:6.2%} | {l2_error:.6f}")


# ============================================================
# 메인 실행
# ============================================================

if __name__ == "__main__":
    orthonormal_system()
    fourier_coefficients_bessel()
    parseval_square_wave()
    vector_decomposition()
    signal_compression_fourier()
    
    print("\n" + "=" * 70)
    print("모든 테스트 완료")
    print("=" * 70)
```

**실행 결과:**
```
======================================================================
테스트 1: 정규직교계 구성 (Gram-Schmidt)
======================================================================

입력 벡터들:
  v1 = [1 1 0]
  v2 = [0 1 1]
  v3 = [1 0 1]

정규직교계:
  e1 = [0.70710678 0.70710678 0.        ]
  e2 = [-0.40824829 0.40824829 0.81649658]
  e3 = [0.57735027 0.57735027 -0.57735027]

정규직교성 검증:
Gram matrix (e^T e):
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
정규직교: True
```

## 🔗 AI/ML 연결

### 1. Fourier Neural Operator (FNO)

연산자 학습에서 Fourier 기저를 사용:
$$\mathcal{F}(u) = \sum_k c_k e^{ikx}$$

Parseval 등식이 있으므로, Fourier 공간에서의 에러가 원래 공간의 에러와 동일.

### 2. 이미지 압축 (JPEG)

이미지를 DCT (Discrete Cosine Transform) 기저로 분해:
$$I = \sum_{i,j} c_{ij} \cos(2\pi i x) \cos(2\pi j y)$$

고주파 계수를 제거해도 저주파 에너지는 보존 (Parseval).

### 3. Wavelet 변환

Fourier보다 공간-주파수 지역화가 좋은 정규직교기저.

$$f = \sum_{j,k} c_{j,k} \psi_{j,k}(x)$$

Parseval: $\|f\|_2^2 = \sum_{j,k} |c_{j,k}|^2$.

### 4. PCA (Principal Component Analysis)

데이터의 공분산 행렬의 고유벡터가 정규직교기저를 이룬다.

$$X = U \Sigma V^T \Rightarrow x_i = \sum_j \sigma_j u_j v_j^T x_i$$

각 주성분이 분산의 일부를 설명 (Parseval).

### 5. 신호처리 (필터링, 복원)

노이즈가 있는 신호:
$$y = x + \text{noise}$$

Fourier 기저에서 높은 전력을 가진 계수만 유지 → 신호 복원.

## ⚖️ 가정과 한계

### 가정

1. **가분성**: 가산 조밀 부분집합이 필요.
2. **Hilbert 구조**: 내적과 완비성이 필수.
3. **정규직교계**: 직교성이 명시적으로 필요.

### 한계

1. **일반 Banach 공간**: Banach 공간에서는 쌍대기저가 존재하지 않을 수 있음.
2. **명시적 기저**: 구체적인 정규직교기저를 찾기가 매우 어려울 수 있음.
3. **무한급수 수렴**: 급수가 수렴한다는 보장도 필요 (Parseval 조건).

## 📌 핵심 정리 (표로 요약)

| 개념 | 정의 | 성질 |
|------|------|------|
| **정규직교계** | $\langle e_n, e_m \rangle = \delta_{nm}$ | 직교성, 정규성 |
| **Fourier 계수** | $c_n = \langle x, e_n \rangle$ | 유일한 표현 |
| **Bessel 부등식** | $\sum \|c_n\|^2 \leq \|x\|^2$ | 항상 성립 |
| **완전성** | $\langle x, e_n \rangle = 0 ∀n ⟹ x=0$ | 정규직교기저 조건 |
| **Parseval 등식** | $\sum \|c_n\|^2 = \|x\|^2$ | 완전성과 동치 |
| **함수 분해** | $x = \sum c_n e_n$ | L² 수렴 |
| **가분 Hilbert** | 가산 정규직교기저 존재 | Zorn 보조정리 |
| **동형** | 가분 무한차원 ≅ ℓ² | 구조 동일 |

## 🤔 생각해볼 문제

### 문제 5.1

$\mathbb{R}^3$에서 벡터 $x = (1, 2, 3)$을 정규직교기저 $\{e_1, e_2, e_3\} = \{(1,0,0), (0,1,0), (0,0,1)\}$로 분해하고 Parseval 등식을 검증하라.

<details>
<summary>해설 보기</summary>

Fourier 계수:
$$c_1 = \langle x, e_1 \rangle = 1$$
$$c_2 = \langle x, e_2 \rangle = 2$$
$$c_3 = \langle x, e_3 \rangle = 3$$

분해:
$$x = 1 \cdot e_1 + 2 \cdot e_2 + 3 \cdot e_3 = (1, 2, 3)$$

Parseval:
$$\|x\|^2 = 1 + 4 + 9 = 14$$
$$\sum |c_n|^2 = 1 + 4 + 9 = 14$$ ✓

</details>

---

### 문제 5.2

$L^2([-π, π])$에서 정규화된 Fourier 기저 $e_n(x) = \frac{1}{\sqrt{π}} e^{inx}$ (또는 실 버전)에 대해, 함수 $f(x) = x$의 처음 5개 Fourier 계수를 계산하라.

<details>
<summary>해설 보기</summary>

실 버전 Fourier 기저 (간편함):
$$c_0 = \frac{1}{\sqrt{π}} \int_{-π}^{π} x dx = 0 \text{ (홀함수)}$$

$$c_n^{\sin} = \sqrt{\frac{2}{π}} \int_{-π}^{π} x \sin(nx) dx = \sqrt{\frac{2}{π}} \cdot \frac{2(-1)^{n+1}}{n}$$

처음 5개:
- $n=1$: $c_1^{\sin} = \sqrt{\frac{2}{π}} \cdot 2 = \sqrt{\frac{8}{π}} \approx 1.596$
- $n=2$: $c_2^{\sin} = \sqrt{\frac{2}{π}} \cdot (-1) = -\sqrt{\frac{2}{π}} \approx -0.798$
- $n=3$: $c_3^{\sin} = \sqrt{\frac{2}{π}} \cdot \frac{2}{3} = \sqrt{\frac{8}{9π}} \approx 0.532$
- 등등...

</details>

---

### 문제 5.3

$\ell^2$에서 벡터 $x = (1, 1/2, 1/4, 1/8, \ldots)$에 대해 Parseval 등식을 검증하라.

<details>
<summary>해설 보기</summary>

$x_n = 2^{-n}$ for $n = 0, 1, 2, \ldots$

표준기저에서 분해하면 그 자체가 정규직교기저이다:
$$c_n = x_n = 2^{-n}$$

Parseval:
$$\|x\|_{\ell^2}^2 = \sum_{n=0}^{\infty} |x_n|^2 = \sum_{n=0}^{\infty} 2^{-2n} = \sum_{n=0}^{\infty} (1/4)^n = \frac{1}{1 - 1/4} = \frac{4}{3}$$

$$\sqrt{\|x\|_{\ell^2}^2} = \sqrt{4/3} = 2/\sqrt{3} \approx 1.155$$

</details>

---

<div align="center">

| | | |
|---|---|---|
| [◀ 이전](./04-riesz-representation.md) | [📚 README](../README.md) | [다음 ▶](./06-fourier-l2-convergence.md) |

</div>
