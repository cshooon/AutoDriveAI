# 2DBB Object‐Detection Pipeline (YOLOv5)

> 🏆 2023 인공지능 자율주행 알고리즘 챌린지 – 장려상

🔗 공모전 페이지: https://challenge2023.gcontest.co.kr/template/m/12709

## 프로젝트 소개

본 프로젝트의 목표는  
도로 환경에서 차량, 보행자, 신호등 등 주변 객체를  
빠르고 정확하게 인식하는 것입니다.

2D Bounding Box(2DBB) 데이터셋을 활용하여  
YOLOv5 모델을 Fine-Tuning 했으며,  
총 9종의 객체를 탐지하고 시각화했습니다.

자율주행 환경에서의 객체 인식을 가정한 프로젝트입니다.

## 데이터셋 요약

| 항목 | 내용 |
|||
| 이미지 수 | 100,000 |
| 클래스 수 | 10 (이 중 9개 객체 탐지) |
| 데이터 분할 | Train : Val : Test = 8 : 1 : 1 |
| 포맷 | COCO JSON (bbox) |

### 클래스 구성

- car  
- truck  
- bus  
- special_vehicle  
- motorcycle  
- bicycle  
- pedestrian  
- traffic_sign  
- traffic_light  
- none (평가 제외)

※ 실제 탐지 대상은 9개 클래스이며,  
`none` 클래스는 평가에서 제외되었습니다.



## 데이터 구조

```

2DBB/
├── training/
│   ├── images/
│   └── labels/
├── validation/
│   ├── images/
│   └── labels/
└── test/
├── images/
└── labels/

```

## 전체 파이프라인

### Dataset & DataLoader

- 이미지 로드 및 전처리  
- JSON 라벨 파싱 및 bbox 좌표 보정  
- YOLO 형식(x_center, y_center, w, h) 변환  
- 가변 bbox 개수를 batch 단위로 처리

## YOLOv5 커스터마이징

데이터셋에 맞게 YOLOv5를 수정하여 사용했습니다.

주요 변경 사항:

- 클래스 수 조정 (80 → 10)  
- 고정 앵커 사용  
- 학습 시 gradient 유지  
- Loss 가중치 재조정  
- 한글 클래스 시각화 지원

Pretrained backbone은 유지하고  
Detection head만 데이터셋에 맞게 수정했습니다.

## 학습 설정

- Optimizer: AdamW  
- Learning rate: Cosine scheduler  
- Epochs: 35  
- Mixed Precision 학습 적용  
- GPU 메모리 사용량 약 6GB

## 성능

| Metric | Validation | Test |
|--|--|--|
| Precision | 0.71 | 0.68 |
| Recall | 0.45 | 0.30 |
| mAP@0.5 | 0.56 | 0.49 |
| mAP@0.5:0.95 | 0.32 | 0.29 |

※ 전체 데이터 중 10,000장 기준 실험 결과입니다.

## 탐지 결과 예시

<img src="https://github.com/user-attachments/assets/adf308a8-e87f-48de-8f21-d3880867cdb4" width="49%">
<img src="https://github.com/user-attachments/assets/4185d699-346a-4fe5-8703-f1d99861cda2" width="49%">

## 수상 내역

| 구분 | 상금 | 수상 팀 |
|--|--|--|
| 최우수상 | 300만원 | 1팀 |
| 우수상 | 200만원 | 1팀 |
| 장려상 | 100만원 | 2팀 |

본 프로젝트는 장려상을 수상했습니다.

## 사용 기술

- PyTorch  
- YOLOv5  
- COCO format dataset  
- Mixed Precision Training  

## 참고
* [YOLOv5 ‑ Ultralytics](https://github.com/ultralytics/yolov5)
* [공모전 데이터 가이드](https://challenge2023.gcontest.co.kr/template/m/frame/downloadlist/12709?q=617)

