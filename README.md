# Cartoon Rendering Study

> **2026-2 한신대학교 컴퓨터그래픽스 보고서**
> 「카툰 렌더링(Toon Rendering) 기술 분석 및 구현」을 위해 제작한 프로젝트입니다.

* **담당교수:** 류승택
* **구현 환경:** Unity / Shader Graph / Python / Jupyter Notebook
* **주제:** Toon Rendering / NPR (Non-Photorealistic Rendering)

---

## 1. 프로젝트 소개

본 프로젝트는 **3D 모델을 2D 애니메이션과 같은 스타일로 표현하는 카툰 렌더링(Toon Rendering)** 기술의 원리를 분석하고 직접 구현하기 위해 제작되었습니다.

기본적인 **Lambert 조명 모델**을 시작으로 다음과 같은 카툰 렌더링 기법을 단계적으로 학습하고 구현하였습니다.

* Lambert Shading
* Half-Lambert Shading
* Toon Shading
* Specular Highlight
* Ramp Texture Shading
* Outline Rendering

렌더링 수식과 명암 변화를 이해하기 위해 **Python / Jupyter Notebook**을 이용하여 시각화하였으며, 실제 렌더링은 **Unity URP / Shader Graph** 환경에서 구현하였습니다.

---

## 2. 주요 구현 내용

### Python / Jupyter Notebook

렌더링에 사용되는 수식과 조명 값의 변화를 이해하기 위해 Python을 이용하여 시각화하였습니다.

* Lambert Shading
* Half-Lambert Shading
* Toon Shading
* `ceil`, `floor`, `round`를 이용한 명암 단계화
* `step`, `smoothstep` 함수 비교
* RGB × Intensity를 이용한 색상 변화
* Gradient 및 Band Shading
* Blinn-Phong Specular
* Shininess 값에 따른 Specular Highlight 변화

---

### Unity / Shader Graph

Unity에서는 Shader Graph를 이용하여 Toon Shader를 직접 구현하고, 다양한 렌더링 방식을 비교하였습니다.

#### Diffuse Shading

기본적인 Lambert 조명 계산을 바탕으로 Half-Lambert 값을 생성합니다.

```text
N · L
  ↓
Saturate
  ↓
× 0.5
  ↓
+ 0.5
  ↓
Half-Lambert
```

Half-Lambert는 Lambert 조명의 음수 영역을 보정하여 전체 명암 범위를 보다 부드럽게 사용할 수 있도록 합니다.

---

#### Toon Shading

Half-Lambert 값을 일정한 단계로 양자화하여 카툰 스타일의 명암을 구현하였습니다.

```text
Toon = ceil(HalfLambert × n) / n
```

`n` 값을 조절하여 명암 단계의 수를 변경할 수 있으며, 이를 통해 부드러운 명암을 여러 단계의 **Band Shading** 형태로 변환할 수 있습니다.

---

#### Specular

Blinn-Phong 모델을 기반으로 Specular Highlight를 추가하여 카툰 스타일의 반사광을 표현하였습니다.

```text
H = normalize(L + V)

Specular = max(0, N · H)^n
```

여기서 `n` 값은 Shininess를 의미하며, 값이 커질수록 반사광의 범위가 좁고 선명하게 표현됩니다.

---

#### Ramp Texture

연속적인 조명 계산 결과를 **Ramp Texture의 UV 좌표**로 사용하여 밝기와 색상을 결정하는 방식을 실험하였습니다.

수식만으로 명암을 단계화하는 방식과 달리 Ramp Texture를 사용하면 원하는 명암 경계와 색상 변화를 텍스처를 통해 직접 제어할 수 있습니다.

---

#### Outline

**Backface Extrude** 방식을 이용하여 모델의 외곽선을 구현하였습니다.

```text
Mesh
 ↓
Vertex Normal 방향으로 확장
 ↓
Front Face 제거
 ↓
확장된 Back Face 렌더링
 ↓
Outline
```

주요 구현 과정은 다음과 같습니다.

* 모델의 정점을 Normal 방향으로 확장
* Front Face 제거
* 확장된 Back Face만 렌더링
* Outline 색상 조절
* Outline 두께 조절

---

## 3. 구현 흐름

본 프로젝트에서는 기본적인 조명 모델에서 비사실적 렌더링 표현으로 확장되는 과정을 단계적으로 구현하였습니다.

```text
Lambert
   ↓
Half-Lambert
   ↓
Intensity Quantization
   ↓
Toon Shading
   ↓
Specular
   ↓
Ramp Texture
   ↓
Outline
```

이를 통해 **사실적인 조명 계산이 카툰 스타일의 NPR(Non-Photorealistic Rendering) 표현으로 변환되는 과정**을 확인하였습니다.

---

## 4. 프로젝트 구성

```text
Toon-Rendering/
│
├─ Python/
│  └─ ToonRendering.ipynb
│
├─ Unity/
│  └─ ToonRenderingProject/
│
├─ Images/
│  └─ Result/
│
└─ README.md
```

> 실제 Repository 구성에 따라 폴더 및 파일명은 변경될 수 있습니다.

---

## 5. 개발 환경

### Unity

* Unity
* Universal Render Pipeline (URP)
* Shader Graph

### Python

* Python 3
* Jupyter Notebook
* NumPy
* Matplotlib

---

## 6. 프로젝트 목적

본 Repository는 **한신대학교 컴퓨터그래픽스 과목의 보고서 작성 및 기술 실습**을 위한 학습용 프로젝트입니다.

보고서에서 사용된 일부 그래프와 렌더링 결과는 본 Repository의 **Python 코드 및 Unity 프로젝트를 통해 직접 제작**하였습니다.

본 프로젝트를 통해 카툰 렌더링에서 사용되는 조명 모델과 명암 단계화 방식의 원리를 학습하고, 이를 실제 Shader 구현으로 연결하는 것을 목표로 하였습니다.

---

## 7. 참고 및 출처

프로젝트의 구현 및 분석 과정에서는 관련 논문, 공식 문서 및 기술 자료 등을 참고하였습니다.

세부 참고자료와 이미지 출처는 **컴퓨터그래픽스 보고서의 참고문헌 및 출처 항목**에 별도로 기재하였습니다.

---

## 8. 생성형 AI 활용

프로젝트 진행 과정에서 생성형 AI를 다음과 같은 용도로 활용하였습니다.

* 그래픽스 개념 및 수식 학습 보조
* Python 코드 작성 및 수정 보조
* Unity / Shader Graph 구현 과정의 질의응답
* 오류 원인 분석 및 구현 방식 검토

생성형 AI를 통해 생성되거나 제안된 내용은 **직접 실행 및 검증한 후 프로젝트에 반영**하였습니다.

---

### Hanshin University Computer Graphics · 2026-2

**Toon Rendering / NPR Study Project**
