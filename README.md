## 시스템 UI
| 페이지1                                                               | 페이지2                                                                   | 페이지3                                                                   |
|-----------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------|
| ![image.png](https://portfolio.ywsung.ai.kr/static/images/10e785d8-095c-477e-bc24-7b3b69f88666.png) | ![image (2).png](https://portfolio.ywsung.ai.kr/static/images/26e53f09-4535-4c7c-8928-b0273d25f3fe.png) | ![image (1).png](https://portfolio.ywsung.ai.kr/static/images/546618ba-907f-48f9-bb10-4bf869e315bf.png) |


---

### 1. 프로젝트 개요
* **프로젝트명**: 산업장비 소리를 이용한 비정상 상황 구별 / 정상 데이터만을 이용한 비정상 상황 구별
* **개발 기간**: 2022.03.01 ~ 2022.12.15 
* **팀 구성**: Frontend/Backend : 1명, AI / 데이터관리 : 2명
* **프로젝트 목표**: 실전문제연구팀의 연구과제, 정상 데이터만으로 이루어진 데이터셋에서 데이터특징을 얻어, 실제 작업 환경에 적용하는 것이 목표임

***

### 2. 기술 스택
* **Frontend/Backend**: `PyQT`
* **AI**: `Tensorflow(Keras), Onnxruntime`
* **Data**: `Librosa`, `Pyserial`, `Parquet`
* **Tools**: `GitHub`


***

### 3. 아키텍처
| 아키텍처                                                     |
|-----------------------------------------------------------------------|
| ![211233816-a6648730-58a6-41d2-ad4c-5414f8995575.png](https://portfolio.ywsung.ai.kr/static/images/e8755ddf-e8d6-43e1-b610-4d67382925f5.png)|


***

### 4. 핵심 기능 및 구현 내용
* **실시간 데이터 수집 모듈**
    * **설명**: 
         * serial 통신을 이용해 유선 마이크와 연결하고, 16k sampling rate로 3초 단위로 데이터를 수집함. 
         * Qthread를 이용하여 PyQT와 병렬적으로 동작할 수 있게 함
    * **사유**:
         * CNC 작업 특성상 즉각적인 변화의 패턴보다 장기간의 패턴을 분석해야 하는 경우가 많아, 3초 단위로 데이터 수집하는 것으로 선택함
    * **효과**: 
         * 실시간으로 소리 데이터를 수집하고, 수집하는 딜레이를 1e-4초 만큼 줄임

* **데이터 전처리 및 경량화**
    * **설명**: 
         * Librosa를 이용하여 Mel-Spectrogram으로 변환하고, Parquet 파일로 변환하여 경량화를 진행함
    * **사유**: 
        * Mel-Spectrogram : CNC 전문 작업자는 소리를 통해 고장 여부를 확인한다는 것을 직업 인터뷰를 통해 얻음. 전문 작업자 관점에서 AI 모델이 판단할 수 있게끔 구성함
        * Parquet : Parquet 파일로 변환하여 저장 공간 효율성, 빠른 컬럼 프루닝을 가능하게 함
    * **효과**: 
         * Mel-Spectrogram으로 변환하여 384k 길이의 1d-array를 (513, 749) 사이즈의 2d-array로 경량화가 가능했음. Parquet 파일로 변환하여 원본 1.1mb 파일을 0.8mb로 압축할 수 있었음 

* **Autoencoder 기반 비정상 상황 구별 모델**
    * **설명**: 
         * Keras Dense Layer을 이용한 Unsupervised Autoencoder 모델
    * **사유**: 
        * Autoencoder : CNC 산업 장비 특성상 비정상 데이터를 만들어내는데 높은 비용이 들어, 정상 데이터만을 이용한 Unsupervised Autoencder 모델 사용
        * Dense Layer : CNN 기반 패턴 기반 모델을 사용할 수도 있으나, Dense Layer만을 이용하여 작업자들이 판단하는 근거인 특정 주파수의 dB 세기 위주의 모델을 구성하고 싶었음
    * **효과**: 
         * 성능 : 추론 속도는 약 0.05sec 임

| Confusion Matrix                                                      | AUC                                                                   |
|-----------------------------------------------------------------------|-----------------------------------------------------------------------|
| ![그림1.png](https://portfolio.ywsung.ai.kr/static/images/49215b96-5d92-4f49-bf39-1d47ca306cfe.png) | ![그림2.png](https://portfolio.ywsung.ai.kr/static/images/dc3f7c9b-4f7b-46c2-be1b-c36fdbcb54d0.png) |

***

### 5. 프로젝트 성과 및 회고 (Outcomes & Retrospective)
* **정량적 성과**: 
         * 논문 : 포스터 논문 1편
         * 수상 : 정보통신산업진흥원 등 4건

* **배운 점 (Lessons Learned)**: 
         * 첫번째 AI 프로젝트이고 전반적인 모듈 개발에 관여했어서 전반적인 AI 모델 개발 개요, AI 서비스 방식에 대한 맛보기가 가능했음.
* **아쉬운 점 및 개선 방향**: 
         * CNN 등 시간에 대한 패턴을 가져올 수 있는 모듈들을 추가해 추가적인 성능을 높이는 방향성을 진행해보고 싶음

***

### 6. 관련 링크 (Links)
* **GitHub Repository**: https://github.com/YEUNU/Autoencoder_Anomaly_Detection
* **블로그 리포팅**: https://portfolio.ywsung.ai.kr/post/36
