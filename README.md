# TONO

  <img src="https://github.com/wina0901/tono_public/blob/main/TONO.png" width="800"/>

## AI 기반 온디바이스 사진 보정 앱

TONO는 단순 필터 앱이 아니라,
<br>
AI segmentation 기반으로 사람과 배경을 자동 분리하고
영역별 독립 보정을 수행하는 모바일 AI 사진 보정 프로젝트입니다.

핵심 목표는 다음과 같습니다.

- 온디바이스 AI segmentation
- 사람 / 배경 독립 보정
- Android 실제 기기 기반 성능 검증
- PyTorch → ONNX → TFLite deployment pipeline 구축
- 실제 모바일 환경에서 동작 가능한 AI 최적화

---

# Tech Stack

## Mobile
- React Native
- Expo SDK 54
- Expo Router
- Kotlin Native Module

## AI / Deployment
- PyTorch
- DeepLabV3
- MobileNetV3
- Custom Lightweight MODNet
- ONNX
- TensorFlow Lite
- onnx2tf

---

# AI Development Flow

TONO는 단순히 기존 segmentation 모델을 연결한 프로젝트가 아니라,
실제 모바일 AI deployment 전체 과정을 직접 구축한 프로젝트입니다.

전체 흐름:

Baseline Segmentation
<br>
→ Postprocess Experiment
<br>
→ Custom DeepLabV3 Training
<br>
→ Android Discrepancy Debugging
<br>
→ Lightweight MODNet Development
<br>
→ ONNX Export
<br>
→ TFLite Conversion
<br>
→ Android Native Integration
<br>
→ Mobile Evaluation
<br>
→ Fallback Architecture
<br>

---

# AI Version History

## v1 Baseline

기존 segmentation TFLite 모델 기반 baseline 구축.

문제:
- 머리카락 디테일 부족
- 경계 품질 부족
- 얇은 구조 표현 부족

### Result
- Mean IoU: 0.8424
- Mean Dice: 0.9085
- Mobile inference: 49.61ms

---

## v2 Postprocess

Morphology + feather 기반 후처리 실험.

결론:
- 시각 품질은 일부 개선
- 정량 성능 개선은 거의 없음

---

## v3 DeepLabV3 Custom Training

직접 segmentation 모델 학습 진행.

### Dataset
- Supervisely Person
- Train: 1852
- Val: 397
- Test: 398

### Model
- DeepLabV3 + MobileNetV3

### Result
- Mean IoU: 0.8946
- Mean Dice: 0.9410

---

## Android Discrepancy Debugging

초기 Android 결과가 PC 대비 크게 낮아지는 문제 발생.

원인:
- Android mask 저장 구조 문제
- Alpha channel 기반 저장 오류

해결:
- RGB grayscale 기반 저장 구조 수정

결과:
- Android metric 정상 복구

복구 후:
- Mean IoU: 0.8907
- Mean Dice: 0.9387

---

## v4 Lightweight MODNet

모바일 친화적 lightweight segmentation 모델 직접 구현.

### Key Features
- 약 270만 parameter
- ONNX/TFLite deployment
- Android native integration
- mobile inference 최적화

### PC Result
- Mean IoU: 0.9136
- Mean Dice: 0.9514

### Android Result
- Mean IoU: 0.8730
- Mean Dice: 0.9231
- Mobile inference: 약 44~87ms

---

# Quantitative Comparison

| Version | IoU | Dice | Mobile Inference |
|---|---|---|---|
| v1 | 0.8424 | 0.9085 | 49.61ms |
| v2 | 0.8424 | 0.9085 | 50.11ms |
| v3 | 0.8907 | 0.9387 | 250.22ms |
| v4 | 0.8730 | 0.9231 | 44~87ms |

---

# Deployment Pipeline

PyTorch
→ ONNX Export
→ TFLite Conversion
→ Android Native Integration
→ Mobile Evaluation

검증 항목:
- Numerical consistency validation
- Android TFLite evaluation
- 실제 기기 inference 측정
- fallback architecture 구축

---

# Key Engineering Experience

이 프로젝트를 통해 다음 경험을 확보했습니다.

- Computer Vision model training
- Segmentation architecture design
- Mobile AI optimization
- ONNX/TFLite deployment
- Numerical consistency verification

---

# Future Work

- Hair detail refinement
- False positive reduction
- Quantization optimization
- Multi-class segmentation
