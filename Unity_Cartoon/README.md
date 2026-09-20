oon Rendering Study

2026-2 한신대학교 컴퓨터그래픽스 보고서
「카툰 렌더링(Toon Rendering) 기술 분석 및 구현」​을 위해 제작한 프로젝트입니다.

담당교수: 류승택
구현 환경: Unity / Shader Graph / Python / Jupyter Notebook
주제: Toon Rendering, NPR(Non-Photorealistic Rendering)
1. 프로젝트 소개

본 프로젝트는 3D 모델을 2D 애니메이션과 같은 형태로 표현하는 카툰 렌더링(Toon Rendering) 기술을 분석하고 직접 구현하기 위해 제작되었습니다.

기본적인 Lambert 조명 모델부터 Half-Lambert, 단계형 Toon Shading, Specular, Ramp Texture, Outline 등의 기법을 학습하고, Python을 이용한 수식 시각화와 Unity Shader Graph를 이용한 실제 렌더링을 진행하였습니다.

2. 주요 구현 내용
Python / Jupyter Notebook

렌더링에 사용되는 수식과 명암 변화를 이해하기 위해 Python으로 시각화하였습니다.

Lambert Shading
Half-Lambert Shading
Toon Shading
ceil, floor, round를 이용한 명암 단계화
step, smoothstep 함수 비교
RGB × Intensity를 이용한 색상 변화
Gradient 및 Band Shading
Blinn-Phong Specular
Shininess 값에 따른 Specular Highlight 변화
Unity / Shader Graph

Unity에서 직접 Toon Shader를 구현하고 다양한 렌더링 방식을 비교하였습니다.

Diffuse Shading

기본적인 Lambert 조명 계산을 기반으로 Half-Lambert 값을 생성합니다.

N · L
↓
Saturate
↓
× 0.5
↓
+ 0.5
↓
Half-Lambert
Toon Shading

Half-Lambert 값을 일정한 단계로 양자화하여 카툰 형태의 명암을 구현합니다.

Toon = ceil(HalfLambert × n) / n

n 값을 변경하여 명암 단계의 수를 조절할 수 있습니다.

Specular

Blinn-Phong 모델을 기반으로 Specular Highlight를 추가하여 카툰 스타일의 반사광을 표현하였습니다.

H = normalize(L + V)

Specular = max(0, N · H)^n
Outline

Backface Extrude 방식을 이용하여 모델 외곽선을 구현하였습니다.

모델의 정점을 Normal 방향으로 확장
Front Face를 제거
확장된 Back Face만 렌더링
Outline 색상 및 두께 조절
Ramp Texture

연속적인 조명 계산 결과를 Ramp Texture의 UV 좌표로 사용하여 명암과 색상을 결정하는 방식을 실험하였습니다.

3. 프로젝트 구성
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

※ 실제 Repository 구조에 따라 폴더명은 변경될 수 있습니다.

4. 구현 과정

본 프로젝트에서는 다음과 같은 순서로 카툰 렌더링을 구현하였습니다.

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

이를 통해 사실적인 조명 모델에서 비사실적 렌더링(NPR) 스타일의 표현으로 변화하는 과정을 단계적으로 확인하였습니다.

5. 개발 환경
Unity
Unity
Universal Render Pipeline (URP)
Shader Graph
Python
Python 3
Jupyter Notebook
NumPy
Matplotlib
6. 목적

본 Repository는 한신대학교 컴퓨터그래픽스 과목의 보고서 작성 및 기술 실습을 위한 학습용 프로젝트입니다.

보고서에서 사용된 일부 그래프와 구현 결과는 본 Repository의 Python 코드 및 Unity 프로젝트를 통해 제작하였습니다.

7. 참고

본 프로젝트의 구현 및 분석 과정에서는 관련 논문, 공식 문서, 기술 자료 등을 참고하였으며, 세부 출처는 보고서의 참고문헌에 별도로 기재하였습니다.

생성형 AI는 개념 학습, 코드 작성 보조 및 구현 과정의 질의응답 등에 활용하였으며, 생성된 내용은 직접 실행 및 검증 후 프로젝트에 반영하였습니다.