---
layout: post
---
<img src="/images/fulls/02.jpg" class="fit image">  


## **Description**  


고객의 수익에 영향을 미칠 수 있는 사고를 즉시 감지하기 위해 실시간으로 고객의 비즈니스 지표를 모니터링하고 분석한다. 고객의 비즈니스 지표를 기반으로 이상 탐지(Anomaly Detection)를 하여 고객에게 위험을 전달할 수 있도록 하는 것이 이 과제의 목표이다.  


<img src="/images/fulls/data_info.jpg" class="fit image">  


데이터는 다음과 같이 이루어져 있다. timestamp(float), value(int), is_anomaly(boolean), predicted(float). 여기서 포커싱해야할 column은 value와 predicted이다. value는 해당 timestamp에 대해 어떠한 지표를 측정한 실수값이며, predicted는 해당 timestamp에 대한 미지의 예측 모델이 추론하여 내놓은 예측값이다. 이 미지의 모델(black box)은 정상 데이터에 대한 분포만 알고 있고, 그를 기반으로 추론을 진행하는 모델이다. 마지막으로, is_anomaly는 데이터가 정상/비정상 여부를 나타낸다. 정상 데이터는 False, 비정상 데이터는 True로 label되어 있다. 당연하게도, 우리는 train set으로 학습과 검증을 하고, test set으로 최종 평가를 진행하기 때문에 train set에는 각 샘플마다 True or False를 확인할 수 있지만(지도학습) test set는 is_anomaly column만이 존재하며 공란으로 되어있다. (우리가 예측해야 한다!)  


<img src="/images/fulls/train_consist.jpg" class="fit image">  


train set은 정상 샘플 15054개, 비정상 샘플 776개로 구성되어 있다. 점수는 test set에 대한 F<sub>1</sub>-score의 평균으로 결정되며, 모든 값을 False로 판단하는 것만으로도 95%의 정확도를 얻을 수 있다. 하지만 이는 바람직하지 않다. 고객은 정상 데이터보다 비정상 데이터에 더 관심을 갖기 때문이다.


## **Method**  


<img src="/images/fulls/data_corr.jpg" class="fit image">  


value와 is_anomaly column 간 상관관계가 가장 높음을 확인할 수 있다.  


<img src="/images/fulls/origin_data.jpg" class="fit image">  


좌측 그림은 시간에 따른 value, 우측 그림은 미지의 모델이 내놓은 예측값(given prediction)에 따른 value를 나타낸 것이다. 앞서 상관관계에서 확인했듯이, 시간에 대한 상관도는 매우 옅기 때문에 given prediction과 value에 중점을 두고 모델이 추론할 수 있도록 데이터 전처리를 진행하겠다. 먼저, 모델은 아래와 같은 Pipeline을 거친다.  
  
1. 데이터 전처리  
2. Support Vector Classifier를 사용한 train set 학습 및 분류한 validation set에 대한 추론  
3. fit된 SVC를 이용한 test set 추론  


