# 2023년 2학기 딥러닝 기말 프로젝트

본 프로젝트는 **인공지능 기반 수목 질병 진단**을 목표로 하며,  
딥러닝과 전이학습(Transfer Learning)을 활용하여 나무 질병을 조기 탐지하고 관리하는데 도움을 주고자 합니다.

## ! 데이터셋은 리포지토리에 포함되지 않음 !
데이터는 eclass 공지사항에서 별도로 다운로드, train과 test 모두 data 디렉토리에 복사하여 주세요\
(data/train, data/test가 되도록 구성)\
\
데이터셋은 서경원 교수님 연구실에서 라벨링한 것이므로,\
해당 리포지토리에서는 데이터에 대한 간략한 설명 이미지만을 포함합니다.

![](./data.jpg)

발표 자료 (완료!) : [https://www.miricanvas.com/v/12pa3cl](https://www.miricanvas.com/v/12q5093) \



## 📌 Introduction
### 왜 나무 질병을 연구해야 하는가?

1. **조기 진단 및 모니터링**  
   - 인공지능과 IoT를 활용한 조경수목 관리로 병해를 조기에 선별 및 예측 가능  
   - 실시간 모니터링을 통해 빠른 대응 가능  

2. **생태계 보전**  
   - 질병 확산을 막아 산림 생태계 보존에 기여  

3. **임업사업 지원**  
   - 임업 산업 적용 시 나무 생산성 향상 및 임산물 품질 개선  
   - 예: 감염된 나무를 신속히 식별·처리하여 **수확량 최적화**  


## 📂 추가 데이터셋: PlantVillage

본 프로젝트에서는 **Transfer Learning**을 위해 [PlantVillage 데이터셋](https://www.kaggle.com/datasets/emmarex/plantdisease)을 추가로 활용하였습니다.  

> ⚠️ **주의:** PlantVillage 데이터셋은 리포지토리에 포함되어 있지 않으므로 별도로 다운로드해야 합니다.

### 🔧 사용 방법
1. [PlantVillage Dataset](https://www.kaggle.com/datasets/emmarex/plantdisease) 다운로드  
2. 압축 해제 후 `PlantVillage/` 폴더를 `data/` 디렉토리에 위치  


## 🚀 How To Use

`main.py`는 총 **다섯 가지 주요 함수**로 구성됩니다:

### 1. `main_resnet()`  
- 느티나무 질병 데이터를 **K-Fold 학습**  
- 모델 저장: `checkpoint/ResNet_Nfold.pt`

### 2. `main_loadmodel()`  
- 저장된 `ResNet_Nfold.pt` 모델 불러오기  
- Test Set 평가 및 **Confusion Matrix** 이미지 저장  

### 3. `main_finetune()`  
- **ImageNet-21k pretrained Vision Transformer** 기반  
- 느티나무 질병 데이터로 **Fine-tuning** 진행  
- 모델 저장: `checkpoint/VisionTransformer_Nfold.pt`

### 4. `main_finetuneWithPlantVillage()`  
- ImageNet-21k pretrained Vision Transformer → PlantVillage 데이터셋 **1차 Fine-tuning**  
- 이후 느티나무 질병 데이터로 **2차 Fine-tuning**  
- 모델 저장:  
  - `checkpoint/VisionTransformer_PlantVillage.pt` (1차)  
  - `checkpoint/VisionTransformer_Nfold.pt` (2차)  

### 5. `main_load_finetunedmodel(use_plantvillage: bool)`  
- `use_plantvillage = False` → `checkpoint/finetune/VisionTransformer_Nfold.pt` 불러오기  
- `use_plantvillage = True` → `checkpoint/finetunePlantVillage/VisionTransformer_Nfold.pt` 불러오기  
- Test Set 평가 및 Confusion Matrix 저장  


## 🖥 실행 방법

### 1. 직접 실행
```bash
python main.py

main.py 하단의

```bash
if __name__=="__main__":
    # 실행할 함수 주석 해제 후 실행
    main_finetune()

### 모듈로 실행
```bash
from main import main_load_finetunedmodel

main_load_finetunedmodel(use_plantvillage=True)


