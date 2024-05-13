
<h1>주제: 스마트 공장을 위한 음향 이벤트 탐지 및 앱 개발</h1>

<h2>1. 예상성과</h2>
- (challenge)dcase2024 참여 및 수상<br>
- (논문) 음향 이벤트 탐지 관련 논문<br>
- (특허) 인공지능 음향 이벤트 탐지 방법<br>
- (어플리케이션)음향 이벤트 탐지 앱 개발

<h2>2. 필요기술</h2>
- 음향신호처리 및 비디오신호처리<br>
- Python/C/C++ 프로그래밍<br>
- 앱 개발 기술<br>
- DCASE2023 챌린지 분석 및 검증, 오픈소스 분석, 논문 분석<br>
- 스마트 공장에서의 기계 이상유무 판별을 위한 네트워크 설계 기술 습득<br>
- MPEG, ACoM 표준화 분석 및 적용방안 도출


<h2>3. 개발 배경 및 필요성</h2>
- 최근 딥러닝을 이용한 어플리케이션들은 영상으로만 한정하지 않고 음향을 부가적으로 활용하여 성능을 개선하고 있음<br>
- 스마트 공장에서 기계의 이상유무는 기계에서 나오는 소리를 이용하여 판별할 수 있으며, 음향분야 최고의 학회중 하나인 DCASE에서 매년 관련한  CHALLENGE를 개최하고 있음<br>
- 음향신호는 영상신호에 비해 상대적으로 저렴한 비용으로 모니터링 하거나 분석할 수 있음<br>

<h2>4. 개발요구사항</h2>
- 음향신호를 영상신호로 처리하는 기술 습득<br>
- DCASE2023에서 정의한 절차에 따른 챌린지 분석 및 검증, 오픈소스 분석, 논문 분석<br>
- DCASE2024 챌린지 참여<br>
- (2학기계획)스마트 공장을 위한 음향 이벤트 탐지 어플리케이션 개발

<h2>5. 관련 문헌 조사</h2>
<a href= "https://arxiv.org/abs/2305.07828"> [DCASE 2023 Challenge Task 2: First-Shot Unsupervised Anomalous Sound Detection for Machine Condition Monitoring 논문 요약 ] </a>

- Oversampling for Imbalance Compensation (불균형 보상을 위한 오버샘플링): 불균형한 데이터셋에서 발생하는 클래스 불균형을 보상하기 위해 사용된다. 소리 이벤트의 빈도가 적은 클래스에 대해 오버샘플링을 수행하여 데이터셋을 균형있게 만들어 모델의 학습을 돕는다.
- Synthetic Data Generation for Robust Detection (로버스트 감지를 위한 합성 데이터 생성): 합성 데이터를 생성하여 모델의 감지 능력을 향상시킨다. 이는 실제 데이터를 변형하거나 합성하여 다양한 환경에서 발생할 수 있는 소리 이벤트에 대한 모델의 감지 능력을 개선한다.
- Attribute ID Classification using Pre-trained Models (사전 학습 모델을 사용한 속성 ID 분류): 사전 학습된 모델을 이용하여 소리 이벤트의 속성을 식별하고 분류한. 이를 통해 소리 이벤트의 다양한 속성을 효과적으로 추출하고 분류할 수 있다.
- Python Librosa 라이브러리: [Librosa 설명](https://wikidocs.net/192879) , [Librosa 사용법](https://hyongdoc.tistory.com/404)
- TensorFlow : [TensorFlow 소개](https://www.tensorflow.org/learn?hl=ko&_gl=1*zjdww2*_up*MQ..*_ga*Mzc4MTA5NDY5LjE3MTE5NjY0MTE.*_ga_W0YLR4190T*MTcxMTk2NjQxMS4xLjAuMTcxMTk2NjQyMS4wLjAuMA..), [TensorFlow 사용법](https://www.tensorflow.org/tutorials?hl=ko&_gl=1*1vpzq5c*_up*MQ..*_ga*Mzc4MTA5NDY5LjE3MTE5NjY0MTE.*_ga_W0YLR4190T*MTcxMTk2NjQxMS4xLjAuMTcxMTk2NjQyMS4wLjAuMA..)
  
<h2>6. 연구개발계획</h2>
1차 (4.1 - 4.9) - Oversampling 기법에 대한 이해와 코드 구현 <br>
2차 (4.10 - 4.18) - Synthetic data generation을 위한 코드 작성 <br>
3차 (4.19 - 4.20) - Attribute ID classification을 사용한 속성 ID 분류 개발된 <br>
4차 (4.21 - 5.10) - 기술들을 토대로 모델 구현 시작 <br>
6차 (5.11 – 5.20) - 데이터셋을 활용한 모델 학습 및 성능 평가 <br> 
7차 (5.21 – 5.31) - 다양한 논문과 비교하며 모델 개선 방향 결정 후 추가적인 실험 및 평가 <br>
8차 (6.1 – 6.10) - DCASE 2024 Challenge 제출 및 2023 Challenge 점수 순위 비교 <br>
9차 (6.11 – 6.20) - 마감일 까지 피드백 및 재 제출, DCASE 2024 Challenge 최종점수 확인 <br>
10차 (6.21 – 6.30) - 최종 모델 완성 및 최적화 작업 <br>

<h2>7. 데이터 세트 다운로드 </h2>

- 개발 데이터세트
  - https://zenodo.org/record/7882613
- 추가 훈련 데이터세트
  - https://zenodo.org/record/7830345
- 평가 데이터세트
  - https://zenodo.org/record/7860847
