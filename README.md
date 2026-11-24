# 🍄 Mario World with Super Mushroom, Block & Kirby

> **A-Frame**과 **Three.js** 기반의 3D WebXR 인터랙티브 마리오 월드 프로젝트입니다.  
> 마리오 테마의 절차적 바닥 텍스처, 360° 파노라마 스카이돔, 정밀하게 보정된 3D GLB 모델과 2D 빌보드 스프라이트가 서로 다른 궤도에서 공전 및 자전하며 역동적인 3차원 공간을 연출합니다.

---

## 🎮 Live Demo
- **배포 주소**: [Vercel Deployment Link](https://mario-world.vercel.app) *(Vercel 배포 URL)*
- **GitHub 저장소**: [gtmobis/mario-world](https://github.com/gtmobis/mario-world)

---

## ✨ 주요 기능 (Key Features)

### 1. 🪐 다중 궤도 공전 & 자전 시스템 (Multi-orbital System)
카메라 Rig을 중심으로 4개의 서로 다른 반경과 궤도 경사각(Inclination), 공전 및 자전 속도를 가진 독립적인 궤도 시스템이 구현되어 있습니다:

| 궤도 | 대상 오브젝트 | 형태 | 반경 (Radius) | 높이 (Y) | 궤도 경사각 | 공전 주기 (Orbit) | 자전 주기 (Spin) |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **1st** | **Super Mushroom** | 3D GLB | 2.8m | +0.35m | 0° | 4.5초 | 2.2초 |
| **2nd** | **Mario Brick Block** | 3D GLB | 3.45m | +0.90m | -6° / 12° | 5.7초 | 3.2초 |
| **3rd** | **Kirby** | 2D Sprite | 4.2m | -0.10m | 14° | 7.0초 | 카메라 응시 (Billboard) |
| **4th** | **Question Block** | 2D Sprite | 5.6m | +0.35m | -10° | 10.2초 | 카메라 응시 (Billboard) |

### 2. 🧱 Canvas API를 이용한 절차적 바닥 텍스처 (`mario-ground`)
- 외부 이미지 로딩 지연 없이 HTML5 Canvas 2D API로 마리오 클래식 브릭의 주황/갈색 베이스, 음영 하이라이트 및 4개 코너 나사못(리벳) 디테일을 256×256 해상도로 실시간 생성합니다.
- `RepeatWrapping`을 적용하여 40×40 타일링으로 바닥 전체(100m×100m)에 선명하게 매핑됩니다.

### 3. 📦 3D GLB 모델 정규화 및 피벗 보정 (`Wrapper Group Pattern`)
- 3D 모델 파일 로드 시 원본 메쉬의 피벗 오프셋 및 제각각인 스케일 문제를 해결하기 위해 `THREE.Box3` 바운딩 박스를 계산하여 중심점(`0, 0, 0`)을 맞추는 **Wrapper Group 패턴**을 적용했습니다.
- 모든 메쉬에 양면 렌더링(`DoubleSide`) 및 sRGB 컬러 스페이스를 적용하여 모델이 뒤틀림 없이 자연스럽게 렌더링됩니다.

### 4. 🎯 카메라 시선 고정 빌보드 컴포넌트 (`always-face-camera`)
- 2D 스프라이트(커비, 물음표 블록)가 사용자의 카메라 위치를 매 프레임 실시간으로 추적(`lookAt`)하여 시점에 관계없이 언제나 정면을 바라보도록 구현했습니다.

### 5. ☀️ 입체적인 조명 & 부드러운 그림자 (PCF Soft Shadows)
- `Directional Light`(2048×2048 섀도우 맵)와 `PCFSoft` 그림자를 적용하여 오브젝트가 바닥에 부드러운 실시간 그림자를 드리웁니다.
- `Ambient Light`와 하늘-지면 색상을 자연스럽게 섞어주는 `Hemisphere Light`를 배치하여 화사하고 풍부한 야외 환경을 조성했습니다.

### 6. ⚡ CORS 제약 없는 하이브리드 데이터 로더
- 브라우저의 로컬 보안 정책(`file://`) 및 웹 호스팅 환경 모두에서 모델과 이미지가 즉시 로드될 수 있도록 Base64 인라인 데이터 스크립트(`*_data.js`)와 정적 에셋 로딩을 상호 보완적으로 지원합니다.

---

## 📁 프로젝트 파일 구조 (File Structure)

```text
├── index.html                  # 메인 3D WebXR 웹 페이지 (A-Frame 씬 및 컴포넌트)
├── README.md                   # 프로젝트 문서
│
├── 3D Models (.glb)
│   ├── mario_super_mushroom.glb # 3D 슈퍼 버섯 모델
│   ├── mario_block.glb          # 3D 마리오 브릭 블록 모델
│   └── model.glb                # 추가 3D 에셋 모델
│
├── Data Scripts (Base64 / Fallback)
│   ├── bg_data.js               # 배경 파노라마 텍스처 데이터
│   ├── kirby_data.js            # 커비 스프라이트 데이터
│   ├── question_block_data.js   # 물음표 블록 스프라이트 데이터
│   ├── mario_mushroom_data.js   # 슈퍼 버섯 모델 데이터
│   └── mario_block_data.js      # 마리오 블록 모델 데이터
│
└── Images & Textures
    ├── kirby.png                # 커비 스프라이트 원본
    ├── question_block.png       # 물음표 블록 스프라이트 원본
    ├── mushroom.png             # 버섯 텍스처 이미지
    └── 59b7def68edf9077af69bfe0b05053f9.jpg # 마리오 하늘 배경
```

---

## 🛠 사용 기술 (Tech Stack)

- **Framework**: [A-Frame](https://aframe.io/) `v1.6.0` (WebXR / Virtual Reality Framework)
- **Core 3D Engine**: [Three.js](https://threejs.org/)
- **Language**: HTML5, Vanilla JavaScript, Canvas 2D API
- **Deployment & Hosting**: [Vercel](https://vercel.com)
- **Version Control**: Git, GitHub, GitHub Desktop

---

## 🚀 로컬 실행 방법 (How to Run Locally)

1. 저장소를 클론(Clone)하거나 다운로드합니다:
   ```bash
   git clone https://github.com/gtmobis/mario-world.git
   ```
2. 프로젝트 폴더의 `index.html`을 브라우저(Chrome, Edge 등)로 열거나, VS Code의 **Live Server** 확장을 사용하여 실행합니다.
