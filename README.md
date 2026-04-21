<div align="center">

# 🔬 Functional Analysis Deep Dive

**"무한차원 벡터공간에서 벡터 놀이를 하는 것과, 왜 $L^2$는 힐베르트 공간이고 $L^1$, $L^\infty$는 아닌지 증명할 수 있는 것은 다르다"**

<br/>

> *"Kernel Method를 사용하는 것과, Mercer 정리로 $k(x, y) = \langle \phi(x), \phi(y) \rangle_\mathcal{H}$가 어떤 특성함수의 내적이 됨을 증명할 수 있는 것은 다르다.  
> Neural Tangent Kernel을 계산하는 것과, NTK가 RKHS의 재생핵이고 무한폭 한계에서 신경망이 RKHS 회귀와 동치임을 증명할 수 있는 것은 다르다."*

무한차원의 선형대수 — 함수를 벡터처럼 다루는 수학, 완비성(Completeness)의 심장부터  
Riesz 표현 정리, Moore-Aronszajn 정리, Representer 정리, NTK 이론까지  
**"왜 이렇게 작동하는가"** 라는 질문으로 Kernel·GP·NTK·Neural Operator의 이론적 기반을 끝까지 파헤칩니다

<br/>

[![GitHub](https://img.shields.io/badge/GitHub-iq--ai--lab-181717?style=flat-square&logo=github)](https://github.com/iq-ai-lab)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.26-013243?style=flat-square&logo=numpy&logoColor=white)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.11-8CAAE6?style=flat-square&logo=scipy&logoColor=white)](https://scipy.org/)
[![Docs](https://img.shields.io/badge/Docs-38개-blue?style=flat-square&logo=readthedocs&logoColor=white)](./README.md)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square&logo=opensourceinitiative&logoColor=white)](./LICENSE)

</div>

---

## 🎯 이 레포에 대하여

함수해석학에 관한 자료는 넘쳐납니다. 하지만 대부분은 **"어떤 공간이 존재한다"** 에서 멈춥니다.

| 일반 자료 | 이 레포 |
|----------|---------|
| "$L^2$는 Hilbert 공간입니다" | $L^2$만이 $L^p$ 중 내적 공간인 이유를 평행사변형 법칙 $\|x+y\|^2 + \|x-y\|^2 = 2(\|x\|^2+\|y\|^2)$의 역방향으로 증명하고, $L^1$·$L^\infty$에서 반례를 직접 구성 |
| "완비성이 중요합니다" | Cauchy 수열이 수렴하지 않을 때 해석학의 어떤 논증이 정확히 어디서 붕괴하는지, Riesz-Fischer 정리로 $L^p$ 완비성을 증명하고 그 한계를 Riesz 보조정리로 확인 |
| "Kernel Trick을 사용합니다" | Representer 정리를 Hilbert 공간 수직분해로 유도하고, Moore-Aronszajn 정리로 PD kernel → RKHS의 유일 존재를 구성적으로 증명, SVM 쌍대 문제가 이 형태가 됨을 라그랑주 쌍대화로 유도 |
| "Gaussian Process의 공분산 커널이 있습니다" | GP posterior mean이 kernel ridge regression과 동치인 이유를 RKHS 재생성질로 증명, GP 샘플 경로가 RKHS 원소가 아닌 이유를 0-1 법칙으로 설명 |
| "NTK로 신경망 훈련을 분석합니다" | 무한폭 극한에서 NTK $\Theta(x,y) = \lim\langle\nabla_\theta f(x), \nabla_\theta f(y)\rangle$가 결정론적 RKHS 재생핵으로 수렴하고, 훈련 동역학이 RKHS gradient flow와 동치임을 스케치 |
| 이론 나열 | NumPy/SciPy로 커널 행렬 스펙트럴 분해, RKHS 회귀, GP posterior, Mercer 고유함수를 직접 시각화·검증 |

---

## 📌 선행 레포 & 후속 레포

```
[Linear Algebra Deep Dive]  ──►  [Calculus & Optimization Deep Dive]  ──►  이 레포
  벡터공간, 내적, 고유값                거리공간, 수렴, Lipschitz               함수해석학
  스펙트럴 분해 — 필수                  완비성 개념 — 필수                      무한차원 선형대수
                                                                               │
                              [Probability Theory Deep Dive]  ──────────────►  │
                                측도론, L^p 공간 기초 — 필수                    │
                                                                               ▼
                                                              [Kernel Methods / GP / NTK 심화]
```

> ⚠️ **선행 학습**: 내적 공간·고유값 분해를 모른다면 [Linear Algebra Deep Dive](https://github.com/iq-ai-lab/linear-algebra-deep-dive)를, 거리공간·수렴·Lipschitz를 모른다면 [Calculus & Optimization Deep Dive](https://github.com/iq-ai-lab/calculus-optimization-deep-dive)를 먼저 학습하세요. $L^p$ 공간을 다루려면 [Probability Theory Deep Dive](https://github.com/iq-ai-lab/probability-theory-deep-dive)의 측도론 기초가 필요합니다.

---

## 🚀 빠른 시작

각 챕터의 첫 문서부터 바로 학습을 시작하세요!

[![Ch1](https://img.shields.io/badge/🔹_Ch1-거리공간과_완비성-4A90D9?style=for-the-badge)](./ch1-metric-banach/01-metric-space-completeness.md)
[![Ch2](https://img.shields.io/badge/🔹_Ch2-내적공간과_Hilbert_공간-4A90D9?style=for-the-badge)](./ch2-hilbert-geometry/01-inner-product-hilbert.md)
[![Ch3](https://img.shields.io/badge/🔹_Ch3-유계_선형_연산자-4A90D9?style=for-the-badge)](./ch3-bounded-operators-dual/01-bounded-linear-operators.md)
[![Ch4](https://img.shields.io/badge/🔹_Ch4-컴팩트_연산자-4A90D9?style=for-the-badge)](./ch4-compact-spectral/01-compact-operators.md)
[![Ch5](https://img.shields.io/badge/🔹_Ch5-RKHS와_재생성질-4A90D9?style=for-the-badge)](./ch5-rkhs-kernel/01-rkhs-reproducing-property.md)
[![Ch6](https://img.shields.io/badge/🔹_Ch6-약미분과_Sobolev_공간-4A90D9?style=for-the-badge)](./ch6-sobolev-variational/01-weak-derivative.md)
[![Ch7](https://img.shields.io/badge/🔹_Ch7-Universal_Approximation-4A90D9?style=for-the-badge)](./ch7-functional-analysis-ai/01-universal-approximation.md)

---

## 📚 전체 학습 지도

> 💡 각 챕터를 클릭하면 상세 문서 목록이 펼쳐집니다

<br/>

### 🔹 Chapter 1: 거리공간·노름공간·Banach 공간

> **핵심 질문:** "Cauchy 수열이 수렴하지 않으면 해석학의 어떤 논증이 붕괴하는가?" 유한차원에서는 당연했던 완비성이 무한차원에서 왜 비자명한 조건이 되는가? $L^p$ 공간은 왜 완비인가?

<details>
<summary><b>거리공간의 완비성부터 Riesz 보조정리까지 (5개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. 거리공간과 완비성](./ch1-metric-banach/01-metric-space-completeness.md) | 거리 공리 세 개 ($d \geq 0$, 대칭성, 삼각부등식), Cauchy 수열의 정의, 완비성의 핵심 역할을 Banach 고정점 정리(수축 사상 정리)로 예시, 유리수 $\mathbb{Q}$가 완비가 아닌 이유, 완비화(completion)의 구성 |
| [02. 노름공간과 Banach 공간](./ch1-metric-banach/02-normed-banach-space.md) | 노름 공리 세 개와 노름이 유도하는 거리, $(\mathbb{R}^n, \|\cdot\|_p)$가 항상 Banach임, 무한차원에서의 비자명성, 동치 노름 조건(유한차원에서는 모든 노름이 동치), 노름의 연속성 |
| [03. $L^p$ 공간 — 정의와 완비성](./ch1-metric-banach/03-lp-spaces-completeness.md) | $\|f\|_p = (\int\|f\|^p d\mu)^{1/p}$ 정의, Hölder 부등식 $\|fg\|_1 \leq \|f\|_p\|g\|_q$ 증명, Minkowski 부등식, Riesz-Fischer 정리($L^p$ 완비성 증명), 거의 어디서나(a.e.) 수렴과 $L^p$ 수렴의 관계 |
| [04. $\ell^p$ 수열공간과 $C([a,b])$ 연속함수공간](./ch1-metric-banach/04-sequence-continuous-spaces.md) | 수열공간 $\ell^p$의 완비성 직접 증명, sup-norm 하의 $C([a,b])$ 완비성 (균등 수렴의 폐쇄성), 점별 수렴 vs 균등 수렴의 차이, Weierstrass 근사 정리와 밀도(density)의 의미 |
| [05. Banach 공간의 유한차원 성질](./ch1-metric-banach/05-finite-dim-properties.md) | 유한차원에서 모든 노름이 동치임을 증명, 유한차원 폐·유계 집합의 컴팩트성, 무한차원 단위구가 컴팩트하지 않음을 Riesz 보조정리로 증명 — 무한차원의 근본적 차이점 |

</details>

<br/>

### 🔹 Chapter 2: Hilbert 공간의 기하학

> **핵심 질문:** $L^2$만이 $L^p$ 중 Hilbert 공간인 이유는 무엇인가? 수직투영이 "최선의 근사"인 이유를 어떻게 엄밀히 증명하는가? Riesz 표현 정리가 Kernel Method의 출발점이 되는 방식은?

<details>
<summary><b>내적 공간부터 Fourier 급수의 L² 수렴까지 (6개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. 내적공간과 Hilbert 공간](./ch2-hilbert-geometry/01-inner-product-hilbert.md) | 내적 공리 (양선형성, 켤레대칭, 양정치), Cauchy-Schwarz 부등식 $\|\langle x,y\rangle\| \leq \|x\|\|y\|$ 증명, 평행사변형 법칙이 내적 공간을 완전히 특징짓는 이유, Hilbert 공간 = 내적 공간 + 완비성 |
| [02. $L^2$가 왜 특별한가](./ch2-hilbert-geometry/02-why-l2-special.md) | $L^2$만이 $L^p$ 중 Hilbert 공간임을 평행사변형 법칙으로 증명, $L^1$·$L^\infty$의 반례 명시적 구성, 내적 $\langle f,g\rangle = \int fg\,d\mu$ 정의, MSE loss = $L^2$ 노름, 기대값으로서의 내적 해석 |
| [03. 수직투영(Orthogonal Projection)](./ch2-hilbert-geometry/03-orthogonal-projection.md) | 닫힌 볼록 집합에 대한 최근점의 존재·유일성 증명, 닫힌 부분공간 $M$에의 수직분해 $\mathcal{H} = M \oplus M^\perp$, 투영 연산자 $P_M$의 선형성·자기수반성·멱등성 증명, Gram-Schmidt의 무한차원 일반화 |
| [04. Riesz 표현 정리](./ch2-hilbert-geometry/04-riesz-representation.md) | 유계 선형범함수 $\varphi \in \mathcal{H}^*$는 유일한 $y \in \mathcal{H}$로 $\varphi(x) = \langle x,y\rangle$ 표현 (증명: $\ker\varphi$의 수직여공간 선택), $\mathcal{H}^* \cong \mathcal{H}$ 반선형 동형, RKHS 평가범함수의 표현으로의 연결 |
| [05. 정규직교기저와 Parseval 등식](./ch2-hilbert-geometry/05-orthonormal-basis-parseval.md) | Bessel 부등식 $\sum\|\langle x,e_n\rangle\|^2 \leq \|x\|^2$ 증명, 가분 Hilbert 공간의 가산 정규직교기저 존재(Zorn 보조정리), Parseval 등식 $\|x\|^2 = \sum\|\langle x,e_n\rangle\|^2$, 완전성(completeness)과 전체성(totality) |
| [06. Fourier 급수의 $L^2$ 수렴](./ch2-hilbert-geometry/06-fourier-l2-convergence.md) | $\{e^{inx}/\sqrt{2\pi}\}$가 $L^2([-\pi,\pi])$의 정규직교기저임을 증명, Fourier 급수의 $L^2$ 수렴 보장 (점별 수렴과의 차이), Dirichlet 핵·Fejér 핵, NumPy로 Fourier 부분합 수렴 시각화 |

</details>

<br/>

### 🔹 Chapter 3: 유계 선형 연산자와 쌍대공간

> **핵심 질문:** 연산자의 "유계성"과 "연속성"이 동치인 이유는 무엇인가? Hahn-Banach 정리가 볼록 최적화와 SVM에 어떻게 연결되는가? 약수렴이 노름수렴보다 중요한 이유는?

<details>
<summary><b>유계 선형 연산자부터 약수렴까지 (5개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. 유계 선형 연산자의 정의](./ch3-bounded-operators-dual/01-bounded-linear-operators.md) | $T: X \to Y$ 선형 연산자, $\|T\| = \sup_{\|x\|=1}\|Tx\|$ 연산자 노름, 연속성과 유계성의 동치 증명 (선형성이 핵심), 미분 연산자가 무한차원에서 비유계인 이유, Closed Graph 정리 |
| [02. 연산자 공간 $B(X,Y)$](./ch3-bounded-operators-dual/02-operator-space.md) | $Y$가 Banach이면 $B(X,Y)$도 Banach임을 증명, 연산자 노름의 곱셈 성질 $\|ST\| \leq \|S\|\|T\|$, 균등 유계 원리 (Uniform Boundedness Principle / Banach-Steinhaus), 열린 사상 정리 |
| [03. 쌍대공간(Dual Space) $X^*$](./ch3-bounded-operators-dual/03-dual-space.md) | 유계 선형범함수의 공간 $X^* = B(X, \mathbb{R})$, $X^*$는 항상 Banach, $L^p$의 쌍대가 $L^q$ ($1/p+1/q=1$)임을 Riesz 정리로 증명, 반사적(reflexive) 공간의 정의와 $L^p$ ($1<p<\infty$)의 반사성 |
| [04. Hahn-Banach 정리](./ch3-bounded-operators-dual/04-hahn-banach.md) | 부분공간의 유계 선형범함수를 전체 공간으로 노름 보존 확장 (Zorn 보조정리 사용), 기하학적 형태: 볼록 집합 분리 정리, SVM의 최대 마진 초평면이 Hahn-Banach의 기하학적 결론임을 연결 |
| [05. 약수렴과 약*수렴](./ch3-bounded-operators-dual/05-weak-convergence.md) | 노름수렴 vs 약수렴 ($x_n \rightharpoonup x \iff \varphi(x_n) \to \varphi(x)$ $\forall\varphi \in X^*$), Hilbert 공간에서 약수렴과 내적수렴의 관계, 약*수렴과 Banach-Alaoglu 정리, 최적화에서의 약수렴 중요성 |

</details>

<br/>

### 🔹 Chapter 4: 컴팩트·자기수반 연산자와 스펙트럴 정리

> **핵심 질문:** 유한차원의 대각화 정리를 무한차원으로 일반화하면 어떤 조건이 필요한가? 컴팩트 연산자의 고유값이 0으로 수렴한다는 것이 왜 Kernel Matrix의 빠른 스펙트럼 감소와 동일한가?

<details>
<summary><b>컴팩트 연산자부터 Fredholm 이론까지 (6개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. 컴팩트 연산자](./ch4-compact-spectral/01-compact-operators.md) | 유계 집합을 상대적 컴팩트 집합으로 보내는 연산자, 유한 랭크 연산자의 극한으로서의 특성, Hilbert-Schmidt 연산자 $\sum\|Te_n\|^2 < \infty$, 적분 연산자가 대표적 컴팩트 연산자인 이유, NumPy로 Gaussian 커널의 빠른 스펙트럼 감소 시각화 |
| [02. 수반 연산자(Adjoint)](./ch4-compact-spectral/02-adjoint-operator.md) | $\langle Tx,y\rangle = \langle x,T^*y\rangle$으로 정의되는 $T^*$의 유일성, $T^{**} = T$, $\|T\| = \|T^*\|$, $\|T^*T\| = \|T\|^2$, 행렬 전치·Hermitian의 무한차원 일반화, 정규 연산자 |
| [03. 자기수반 연산자(Self-adjoint)](./ch4-compact-spectral/03-self-adjoint-operators.md) | $T = T^*$ 조건, 실수 스펙트럼 증명, 양정치 연산자 $\langle Tx,x\rangle \geq 0$, 제곱근 연산자 $T^{1/2}$의 존재, Hilbert 공간의 대칭 행렬 대응물, kernel trick에서 Gram 행렬이 PSD인 이유 |
| [04. 스펙트럴 정리 — 컴팩트·자기수반](./ch4-compact-spectral/04-spectral-theorem-compact.md) | $T = \sum\lambda_n\langle\cdot,e_n\rangle e_n$ (가산 실수 고유값, 0 외에는 유한 다중도), 증명: Rayleigh 지수 최대화 → 첫 번째 고유쌍, 귀납으로 나머지, Mercer 정리와의 직접 연결, NumPy로 Gaussian 커널 고유함수 분해 |
| [05. 일반 스펙트럼 이론](./ch4-compact-spectral/05-general-spectrum-theory.md) | 스펙트럼 $\sigma(T) = \{\lambda : T-\lambda I \text{ 가역 아님}\}$, 점 스펙트럼·연속 스펙트럼·잔여 스펙트럼 분류, 스펙트럼 반경 $r(T) = \lim\|T^n\|^{1/n}$, 자기수반 연산자 스펙트럼의 실수성 |
| [06. Fredholm 이론과 적분방정식](./ch4-compact-spectral/06-fredholm-integral-equations.md) | 컴팩트 섭동 하의 Fredholm 지표 불변성, Fredholm 대안(alternative): $f = Kf + g$의 해가 없거나 유일하거나, 적분방정식과 Kernel Ridge Regression의 구조적 동치성 |

</details>

<br/>

### 🔹 Chapter 5: RKHS와 커널 방법

> **핵심 질문:** 재생핵 $k(x,y) = \langle k_x, k_y\rangle_\mathcal{H}$가 어떻게 무한차원 내적을 유한 계산으로 바꾸는가? Representer 정리는 왜 무한차원 최적화를 유한차원으로 축약하는가?

<details>
<summary><b>RKHS 정의부터 Gaussian Process까지 (7개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. RKHS의 정의와 재생성질](./ch5-rkhs-kernel/01-rkhs-reproducing-property.md) | $\mathcal{H}$가 함수공간, 평가범함수 $\delta_x: f \mapsto f(x)$가 유계, Riesz 표현 정리로 $k_x \in \mathcal{H}$ 존재하여 $f(x) = \langle f, k_x\rangle$, 재생핵 $k(x,y) = \langle k_x,k_y\rangle$, Sobolev 공간이 RKHS인 이유 |
| [02. Positive Definite Kernel](./ch5-rkhs-kernel/02-positive-definite-kernels.md) | $\sum\alpha_i\alpha_j k(x_i,x_j) \geq 0$ 정의, Gaussian $k(x,y)=\exp(-\|x-y\|^2/2\sigma^2)$의 PD성 Bochner 정리로 증명, Polynomial·Laplace·Matérn 커널, PD kernel의 합·곱 안정성 |
| [03. Moore-Aronszajn 정리](./ch5-rkhs-kernel/03-moore-aronszajn.md) | PD kernel $k$가 주어지면 유일한 RKHS $\mathcal{H}_k$가 존재함을 구성적으로 증명: $\text{span}\{k_x\}$를 내적 $\langle k_x, k_y\rangle = k(x,y)$로 완비화, 역방향(RKHS → PD kernel) 증명, NumPy로 유한차원 근사 |
| [04. Mercer 정리](./ch5-rkhs-kernel/04-mercer-theorem.md) | 컴팩트 집합 위의 연속 PD kernel은 $k(x,y) = \sum\lambda_n\phi_n(x)\phi_n(y)$, 고유함수 전개의 균등 수렴, feature map $\phi(x) = (\sqrt{\lambda_n}\phi_n(x))$의 존재와 Hilbert 공간 해석, Gaussian 커널의 Hermite 함수 전개 |
| [05. Representer 정리](./ch5-rkhs-kernel/05-representer-theorem.md) | 정규화된 경험적 리스크 최소화 $\min\sum L(y_i,f(x_i)) + \Omega(\|f\|_\mathcal{H})$의 최적해는 $f^* = \sum\alpha_i k(\cdot,x_i)$ 형태, Hilbert 공간 수직분해로 증명, Kernel Ridge Regression의 closed-form 유도, NumPy 검증 |
| [06. SVM의 쌍대 유도](./ch5-rkhs-kernel/06-svm-dual-derivation.md) | 라그랑주 쌍대화 + Representer 정리로 $\min\frac{1}{2}\alpha^T K\alpha - \mathbf{1}^T\alpha$ 형태 유도, KKT 조건에서 서포트 벡터의 의미, kernel trick의 수학적 정당성, `scikit-learn` SVM 결과와 직접 비교 검증 |
| [07. Gaussian Process와 RKHS](./ch5-rkhs-kernel/07-gaussian-process-rkhs.md) | GP의 공분산 커널이 RKHS 재생핵, posterior mean = kernel ridge regression (λ=σ²) 동치 증명, GP 샘플 경로가 RKHS 원소가 아님(Driscoll 정리, 0-1 법칙), Bayesian Optimization과 RKHS |

</details>

<br/>

### 🔹 Chapter 6: Sobolev 공간과 변분법

> **핵심 질문:** 고전 미분이 없는 함수에 "약미분"을 정의하면 어떤 이점이 생기는가? Sobolev 공간이 PINN·FEM의 이론적 기반이 되는 방식은? PDE의 약해가 존재한다는 것을 어떻게 보장하는가?

<details>
<summary><b>약미분부터 PDE 약해까지 (5개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. 약미분(Weak Derivative)](./ch6-sobolev-variational/01-weak-derivative.md) | $\int f g'\,dx = -\int f' g\,dx$ (테스트 함수 $g$에 대해)로 약미분 도입, 고전 미분과의 관계 및 차이, 약미분은 있지만 고전 미분이 없는 함수(예: $\|x\|$), 분포(distribution) 개념과의 연결 |
| [02. Sobolev 공간 $W^{k,p}$](./ch6-sobolev-variational/02-sobolev-spaces.md) | $\|f\|_{W^{k,p}} = \sum_{|\alpha|\leq k}\|D^\alpha f\|_p$ 정의, $W^{k,p}$가 Banach ($H^k = W^{k,2}$는 Hilbert), Sobolev 공간 위계 $H^s \subset H^t$ ($s>t$), Neural Network 근사이론의 기초 |
| [03. Poincaré 부등식과 Sobolev 매몰 정리](./ch6-sobolev-variational/03-poincare-sobolev-embedding.md) | Poincaré 부등식 $\|f\|_p \leq C\|\nabla f\|_p$ (경계 조건 하), Sobolev 매몰 정리 $W^{k,p}\hookrightarrow C^m$ (차원 조건 $k-n/p > m$), 컴팩트 매몰, FEM·PINN 수렴 분석의 핵심 도구 |
| [04. 변분법 기초](./ch6-sobolev-variational/04-calculus-of-variations.md) | 에너지 범함수 $E[f] = \int L(x,f,\nabla f)\,dx$ 최소화, Euler-Lagrange 방정식 $\frac{\partial L}{\partial f} - \nabla\cdot\frac{\partial L}{\partial(\nabla f)} = 0$ 유도, 직접법(Direct Method)으로 최소화원 존재성, 최적제어와 물리정보신경망의 기초 |
| [05. PDE의 약해(Weak Solution)](./ch6-sobolev-variational/05-weak-solution-pde.md) | Lax-Milgram 정리: 쌍선형 형식이 coercive·bounded이면 약해가 유일하게 존재, FEM의 이론적 근거, PINN loss가 이 틀에서 PDE 잔차를 최소화하는 방식, Sobolev 노름에서의 수렴 보장 |

</details>

<br/>

### 🔹 Chapter 7: 함수해석과 AI — 무한차원 학습 이론

> **핵심 질문:** Universal Approximation을 Stone-Weierstrass로 증명할 수 있는가? NTK가 왜 RKHS의 재생핵인가? Neural Operator는 어떤 의미에서 Banach 공간 사이 연산자를 근사하는가?

<details>
<summary><b>Universal Approximation부터 PINN Sobolev 수렴까지 (4개 문서)</b></summary>

<br/>

| 문서 | 핵심 정리·증명 |
|------|--------------|
| [01. Universal Approximation을 Functional Analysis로](./ch7-functional-analysis-ai/01-universal-approximation.md) | $C([0,1])$의 조밀성(denseness), ReLU 신경망이 $C(\mathbb{R}^n)$에서 조밀함을 Stone-Weierstrass 정리로 증명, 어떤 함수공간에서 "근사"를 논하느냐에 따라 결론이 달라지는 이유 ($L^2$ vs 균등수렴 vs Sobolev) |
| [02. Neural Tangent Kernel (NTK) 이론](./ch7-functional-analysis-ai/02-ntk-theory.md) | 무한폭 극한에서 NTK $\Theta(x,y) = \lim\langle\nabla_\theta f(x), \nabla_\theta f(y)\rangle$가 결정론적 커널로 수렴 (Jacot 2018 핵심 아이디어), NTK 하의 훈련 동역학 $\dot{f} = -\Theta(f-y)$가 RKHS gradient flow와 동치, NTK RKHS에서의 수렴 분석 |
| [03. Neural Operator와 함수공간 학습](./ch7-functional-analysis-ai/03-neural-operator.md) | Banach 공간 사이 연산자 $G: \mathcal{U} \to \mathcal{V}$의 범용 근사, Fourier Neural Operator(FNO): Fourier 기저에서 연산자 학습, DeepONet: Trunk-Branch 분해, Universal Operator Approximation 정리, PDE Solver로서의 응용 |
| [04. Implicit Neural Representation과 Sobolev 수렴](./ch7-functional-analysis-ai/04-implicit-neural-sobolev.md) | NeRF·SIREN이 함수를 Sobolev 노름으로 근사하는 의미, PINN의 Sobolev-loss가 PDE 잔차의 $H^{-1}$ 노름을 제어하는 방식, 수렴 보장을 위한 Sobolev 매몰 조건, 약해 이론과의 연결 |

</details>

---

## 💻 실험 환경

모든 챕터의 실험은 아래 환경에서 재현 가능합니다.

```bash
# requirements.txt
numpy==1.26.0
scipy==1.11.0
matplotlib==3.8.0
scikit-learn==1.3.0    # Kernel SVM, GP 비교
cvxpy==1.4.0           # SVM 쌍대 문제
jupyter==1.0.0
```

```bash
# 환경 설치
pip install numpy==1.26.0 scipy==1.11.0 matplotlib==3.8.0 \
            scikit-learn==1.3.0 cvxpy==1.4.0 jupyter==1.0.0

# 실험 노트북 실행
jupyter notebook
```

```python
# 대표 실험 예시 — Mercer 정리 시각화 (Gaussian 커널의 고유함수)
import numpy as np
import matplotlib.pyplot as plt

def gaussian_kernel(X, Y, sigma=1.0):
    d = np.sum((X[:, None] - Y[None, :]) ** 2, axis=-1)
    return np.exp(-d / (2 * sigma**2))

# [-3, 3] 균등 그리드
x = np.linspace(-3, 3, 200).reshape(-1, 1)
K = gaussian_kernel(x, x, sigma=1.0)

# 고유값 분해 (스펙트럴 정리의 실전)
eigvals, eigvecs = np.linalg.eigh(K)
eigvals, eigvecs = eigvals[::-1], eigvecs[:, ::-1]

fig, axes = plt.subplots(1, 2, figsize=(12, 4))

# 고유값 스펙트럼 — 컴팩트 연산자는 0으로 급속 수렴
axes[0].semilogy(eigvals[:50], 'o-', markersize=4)
axes[0].set_title('고유값 (log) — 컴팩트 연산자: 0으로 급속 수렴')
axes[0].set_xlabel('n'); axes[0].set_ylabel('λ_n')
axes[0].grid(True, alpha=0.3)

# 앞 5개 고유함수 = Mercer의 feature map 성분
for i in range(5):
    axes[1].plot(x.flatten(), eigvecs[:, i] * np.sqrt(abs(eigvals[i])),
                 label=f'√λ_{i+1} φ_{i+1}')
axes[1].set_title('Feature Map 성분 (Hermite 함수 유사)')
axes[1].legend(fontsize=8); axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Representer 정리 검증: Kernel Ridge Regression
def kernel_ridge(X_train, y_train, X_test, sigma=1.0, lam=0.01):
    K_train = gaussian_kernel(X_train, X_train, sigma)
    K_test  = gaussian_kernel(X_test,  X_train, sigma)
    alpha = np.linalg.solve(K_train + lam * np.eye(len(X_train)), y_train)
    return K_test @ alpha  # f* = Σ α_i k(·, x_i) 형태!
```

---

## 📖 각 문서 구성 방식

모든 문서는 동일한 구조로 작성됩니다.

| 섹션 | 설명 |
|------|------|
| 🎯 **핵심 질문** | 이 문서를 읽고 나면 답할 수 있는 질문 |
| 🔍 **왜 이 이론이 AI에서 중요한가** | Kernel SVM, GP, NTK, Neural Operator, PINN과의 구체적 연결 |
| 📐 **수학적 선행 조건** | Linear Algebra / Calculus / Probability 레포 참조 링크 포함 |
| 📖 **직관적 이해** | 유한차원과 무한차원의 차이를 물리적·기하학적 비유로 |
| ✏️ **엄밀한 정의** | 공리 수준의 정형적 정의 |
| 🔬 **정리와 증명** | Riesz, Mercer, Moore-Aronszajn, Spectral Theorem, Representer — "자명하다" 생략 없음 |
| 💻 **NumPy 구현 검증** | 커널 행렬, RKHS 회귀, SVM, GP posterior 직접 구현 |
| 🔗 **AI/ML 연결** | SVM, GP, NTK, Neural Operator, PINN 구체 사례 |
| ⚖️ **가정과 한계** | 가분성·컴팩트성·PD성이 깨질 때 어떤 이론이 붕괴하는가 |
| 📌 **핵심 정리** | 한 화면 요약 |
| 🤔 **생각해볼 문제** | 개념 심화 질문 + 해설 |

---

## 🗺️ 추천 학습 경로

<details>
<summary><b>🟢 "Kernel SVM을 쓰지만 왜 kernel trick이 성립하는지 설명 못한다" — RKHS 집중 (3일)</b></summary>

<br/>

```
Day 1  Ch2-04  Riesz 표현 정리 → 평가범함수가 내적으로 표현되는 이유
       Ch5-01  RKHS 재생성질 → f(x) = ⟨f, k_x⟩의 의미
Day 2  Ch5-02  Positive Definite Kernel → Gaussian, Polynomial 커널의 PD성 증명
       Ch5-03  Moore-Aronszajn 정리 → PD kernel에서 RKHS의 유일 구성
Day 3  Ch5-05  Representer 정리 → 최적해가 Σ α_i k(·, x_i) 형태인 이유
       Ch5-06  SVM 쌍대 유도 → 라그랑주 쌍대 + kernel trick의 수학적 완성
```

</details>

<details>
<summary><b>🟡 "L² loss를 쓰지만 왜 L²가 Hilbert 공간인지, 왜 중요한지 모른다" — Hilbert 집중 (1주)</b></summary>

<br/>

```
Day 1  Ch1-03  L^p 공간과 Riesz-Fischer 정리 → L^p 완비성 증명
Day 2  Ch2-01  내적공간과 Cauchy-Schwarz → 내적 공간의 기하학
       Ch2-02  L²가 왜 특별한가 → 평행사변형 법칙으로 L^1, L^∞ 반례 구성
Day 3  Ch2-03  수직투영 → H = M ⊕ M^⊥ 분해, 투영 연산자의 성질
Day 4  Ch2-04  Riesz 표현 정리 → H* ≅ H, RKHS 평가범함수와의 연결
Day 5  Ch2-05  Parseval 등식 → Fourier 계수의 에너지 보존
Day 6  Ch2-06  Fourier 급수의 L² 수렴 → 점별 수렴과의 차이
Day 7  Ch4-04  스펙트럴 정리 → 무한차원 대각화, Mercer 정리와의 연결
```

</details>

<details>
<summary><b>🔴 "Banach 공간부터 NTK까지 완전 정복한다" — 전체 정복 (7주)</b></summary>

<br/>

```
1주차  Chapter 1 전체 — 거리공간·Banach 공간
        → ℚ의 완비화, L^p Riesz-Fischer 직접 증명
        → Riesz 보조정리로 무한차원 단위구 비컴팩트 확인

2주차  Chapter 2 전체 — Hilbert 공간의 기하학
        → L¹/L∞에서 평행사변형 법칙 반례 NumPy로 시각화
        → Parseval 등식, Fourier 급수 L² 수렴 실험

3주차  Chapter 3 전체 — 유계 선형 연산자와 쌍대공간
        → 미분 연산자의 비유계성 실험
        → Hahn-Banach → SVM 마진 최대화 연결 이해

4주차  Chapter 4 전체 — 컴팩트·자기수반·스펙트럴
        → Gaussian 커널 행렬 고유값 0으로의 수렴 시각화
        → 스펙트럴 정리 → Mercer 정리 직접 연결

5주차  Chapter 5 전체 — RKHS와 커널 방법
        → Moore-Aronszajn 구성 NumPy 구현
        → Representer 정리 검증, SVM·KRR·GP posterior 비교

6주차  Chapter 6 전체 — Sobolev 공간과 변분법
        → 약미분 있는 함수 vs 고전 미분 있는 함수 비교
        → Euler-Lagrange 방정식 SymPy로 기호 유도

7주차  Chapter 7 전체 — AI 응용
        → Universal Approximation → Stone-Weierstrass 연결
        → NTK 커널 행렬 계산 실험 (유한폭 vs 이론값 비교)
```

</details>

---

## 🔗 연관 레포지토리

| 레포 | 주요 내용 | 연관 챕터 |
|------|----------|-----------|
| [linear-algebra-deep-dive](https://github.com/iq-ai-lab/linear-algebra-deep-dive) | 벡터공간, 내적, 고유값 분해, Spectral Theorem | Ch2(내적·Hilbert), Ch4(스펙트럴 정리) |
| [calculus-optimization-deep-dive](https://github.com/iq-ai-lab/calculus-optimization-deep-dive) | 거리공간, 수렴, Lipschitz, Lagrange | Ch1(완비성), Ch6(변분법·Lagrange) |
| [probability-theory-deep-dive](https://github.com/iq-ai-lab/probability-theory-deep-dive) | 측도론, $L^p$ 공간, 기댓값, 조건부 확률 | Ch1(L^p 완비성), Ch5(GP) |
| [convex-optimization-deep-dive](https://github.com/iq-ai-lab/convex-optimization-deep-dive) | 볼록 집합·함수, 쌍대성, KKT, Interior Point | Ch3(Hahn-Banach·분리정리), Ch5(SVM 쌍대) |

> 💡 이 레포는 **무한차원 선형대수로서의 함수해석학**에 집중합니다. Chapter 1~4는 순수 수학 레포로 학습 가능하며, Chapter 5~7은 Kernel Methods·Gaussian Process·딥러닝 이론 배경이 있을 때 연결이 더욱 깊어집니다.

---

## 📖 Reference

- **Functional Analysis** (Rudin) — 표준 고전: Banach·Hilbert 공간, 유계 연산자, 스펙트럴 이론
- **Real and Complex Analysis** (Rudin) — $L^p$ 공간의 완비성, Riesz-Fischer, Radon-Nikodym
- **Introduction to Functional Analysis** (Kreyszig) — 비교적 친절한 입문서, 풍부한 예제
- **Reproducing Kernel Hilbert Spaces in Probability and Statistics** (Berlinet & Thomas-Agnan) — RKHS 표준 교재
- **Learning with Kernels** (Schölkopf & Smola) — SVM·RKHS의 ML 표준
- **Gaussian Processes for Machine Learning** (Rasmussen & Williams) — GP 바이블, RKHS와의 연결
- **Partial Differential Equations** (Evans) — Sobolev 공간, 약해, 변분법의 표준
- **Neural Tangent Kernel: Convergence and Generalization in Neural Networks** (Jacot et al., NeurIPS 2018) — NTK 원전
- **Fourier Neural Operator for Parametric Partial Differential Equations** (Li et al., ICLR 2021) — Neural Operator 학습

---

<div align="center">

**⭐️ 도움이 되셨다면 Star를 눌러주세요!**

Made with ❤️ by [IQ AI Lab](https://github.com/iq-ai-lab)

<br/>

*"Kernel Method를 사용하는 것과, Representer 정리로 최적해가 $\sum\alpha_i k(\cdot, x_i)$ 형태가 됨을 유도할 수 있는 것은 다르다"*

</div>
