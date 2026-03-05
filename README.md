# Grounding DINO vs OWL-ViT

Open-Vocabulary Object Detection 모델인 **Grounding DINO**와 **OWL-ViT**를 LVIS Validation 데이터셋 기준으로 벤치마크 비교한 노트북입니다.

---

## 개요

텍스트 쿼리(자연어)로 객체를 탐지하는 두 Zero-shot Detection 모델의 **추론 속도**와 **탐지 정확도(IoU)** 를 동일한 조건에서 비교합니다.

| 모델 | 백본 | 특징 |
|------|------|------|
| [Grounding DINO](https://github.com/IDEA-Research/GroundingDINO) | Swin Transformer + DINO | 언어-비전 융합 기반 고정밀 탐지 |
| [OWL-ViT](https://huggingface.co/google/owlvit-base-patch32) | ViT (CLIP 기반) | 경량화, 빠른 추론 속도 |

---

## 실험 환경

- **데이터셋**: [LVIS v1 Validation](https://www.lvisdataset.org/) (50 샘플)
- **평가 지표**: Average IoU, Average Inference Time
- **신뢰도 임계값(Threshold)**: 0.2 (두 모델 공통 적용)
- **프레임워크**: PyTorch, Hugging Face Transformers, torchvision

---

## 평가 파이프라인

```
LVIS JSON 로드
    ↓
이미지별 카테고리 쿼리 생성
    ↓
┌──────────────┐     ┌──────────────┐
│ Grounding    │     │   OWL-ViT    │
│    DINO      │     │              │
│ (text: "A.B")│     │("a photo of")│
└──────┬───────┘     └──────┬───────┘
       ↓                    ↓
   IoU 계산  ←  GT bbox  →  IoU 계산
       ↓                    ↓
       └────────┬───────────┘
                ↓
         결과 시각화 (속도 / 정확도)
```
