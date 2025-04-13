# Hand Bone Image Segmentation 대회

**대회 기간:** 2024.11.13 - 2024.11.28  
**주최:** Naver Connect & Upstage

## 팀원
- [김준원](https://github.com/KimJunWon98)
- [이준학](https://github.com/danlee0113)
- [김민환](https://github.com/alsghks1066)
- [전인석](https://github.com/inDseok)
- [신석준](https://github.com/SeokjunShin)


## 대회 소개

뼈는 인체의 구조와 기능에 매우 중요한 역할을 합니다. 특히 정확한 뼈 분할(Bone Segmentation)은 의료 진단과 치료 계획에 필수적입니다.

본 대회에서는 X-ray 이미지 기반의 **Hand Bone Segmentation**을 수행하며, 손가락, 손등, 팔을 포함한 **총 29개의 뼈 클래스를 분할하는 것이 주요 과제**입니다.


![image](https://github.com/user-attachments/assets/fe33f559-b68f-4db3-9aa9-228652154972)

## 평가 방법

### Dice Coefficient

Semantic Segmentation 분야에서 대표적으로 사용되는 평가 방법이며, 본 대회에서는 Test Set의 Dice 계수를 기준으로 성능을 평가합니다.

$$Dice = \frac{2|X \cap Y|}{|X| + |Y|}$$



## 리더보드
- Public Score
![image](https://github.com/user-attachments/assets/458b3b61-a1ed-4171-adf0-b51b38bb1cb0)
- Private Score
![image](https://github.com/user-attachments/assets/5fd45d4a-1594-4a2f-bc90-78170688adb9)


## 데이터셋 정보

- **Train set:** 이미지 800장 (라벨 제공)
- **Test set:** 이미지 288장 (라벨 미제공)
- **이미지 크기:** 2048 x 2048
- **클래스:** 손가락, 손등, 팔 포함 총 29개의 뼈 종류

![X-ray_of_normal_hand](https://github.com/user-attachments/assets/5cb5c224-cfb3-4c4f-843c-bdcf49b03feb)


## 개발 환경

- **GPU:** NVIDIA V100

## 협업 환경

- **Notion**을 통한 실험 관리 및 정보 공유
- **Wandb**를 이용한 실험 결과 시각화 및 모델 학습 모니터링


![image](https://github.com/user-attachments/assets/a6d68c64-da0e-4958-8cca-34da4cdec579)
![image](https://github.com/user-attachments/assets/6cf37168-f8fe-46ac-9b91-d248162b9580)

## 사용 모델

| Model        | 특징                                                                                                    | Library |
|--------------|---------------------------------------------------------------------------------------------------------|---------|
| U-Net++      | 다중 skip connection을 통해 다양한 스케일의 특징을 통합, 세부 분할 성능 향상                                 | SMP     |
| DeepLabV3+   | ASPP와 디코더 모듈을 활용하여 다중 스케일의 맥락 정보와 경계 표현을 강화                                  | SMP     |
| UPerNet      | FPN 기반으로 다양한 스케일의 특징을 결합하여 강력한 전역적 표현 생성                                      | SMP     |

## 데이터 증강 기법

### 기본 증강 기법

- **좌우 반전**
- **상하 반전**
- **90도 회전**
- **270도 회전**

### Albumentations 사용 증강 기법

| 기법                       | 세부 내용                                                                           | 적용 확률 (p) |
|----------------------------|-------------------------------------------------------------------------------------|---------------|
| Resize                     | 1536 x 1536 크기로 이미지 조정                                                        | -             |
| RandomBrightnessContrast   | 밝기와 대비를 무작위로 조정                                                          | 0.5           |
| CLAHE                      | 제한된 대비 적응형 히스토그램 균일화 (클립 한계 2.0, 타일 그리드 (8,8))                | 0.5           |
| Sharpen                    | 이미지 선명도 증가 (알파 0.2–0.5, 밝기 0.5–1.0)                                      | 0.5           |
| GaussianBlur               | 가우시안 블러 적용 (커널 크기: 1–3)                                                  | 0.5           |

### 보간 기법 (Interpolation)

- **Bilinear Interpolation:** 인접 픽셀 4개 기반 선형 보간
- **Bicubic Interpolation:** 주변 픽셀 16개 활용 3차 보간
- **Lanczos Interpolation:** Lanczos 커널을 사용한 sinc 함수 기반 보간

---

## 결론 및 주요 인사이트

- 데이터 증강이 성능 향상에 크게 기여함
- 협업 툴(Notion, Wandb)을 통한 효율적이고 체계적인 실험 관리가 중요함
- 시각화를 통해 모델의 강점과 약점을 명확하게 파악하는 것이 핵심임
