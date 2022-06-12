---
layout: post
---
<img src="/images/fulls/02.jpg" class="fit image">  
  
**Description**  
  
고객의 수익에 영향을 미칠 수 있는 사고를 즉시 감지하기 위해 실시간으로 고객의 비즈니스 지표를 모니터링하고 분석한다. 고객의 비즈니스 지표를 기반으로 이상 탐지(Anomaly Detection)를 하여 고객에게 위험을 전달할 수 있도록 하는 것이 이 과제의 목표이다.  
  
<img src="/images/fulls/data_info.jpg" class="fit image">  
   
데이터는 다음과 같이 이루어져 있다. timestamp(float), value(int), is_anomaly(boolean), predicted(float). 여기서 포커싱해야할 column은 value와 predicted이다. value는 해당 timestamp에 대해 어떠한 지표를 측정한 실수값이며, predicted는 해당 timestamp에 대한 미지의 예측 모델이 추론하여 내놓은 예측값이다. 이 미지의 모델(black box)은 정상 데이터에 대한 분포만 알고 있고, 그를 기반으로 추론을 진행하는 모델이다.  
  
점수는 F<sub>1</sub>-score의 평균으로 결정되며, 모든 값을 False로 판단하는 것만으로도 95%의 정확도를 얻을 수 있다. 하지만 이는 바람직하지 않다. 고객은 정상 데이터보다 비정상 데이터에 더 관심을 갖기 때문이다.
  
**Method**  
  
