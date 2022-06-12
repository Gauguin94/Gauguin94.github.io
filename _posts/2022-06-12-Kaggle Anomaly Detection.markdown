---
layout: post
---
<img src="/images/fulls/02.jpg" class="fit image">  


## **대회 개요**  


고객의 수익에 영향을 미칠 수 있는 사고를 즉시 감지하기 위해 실시간으로 고객의 비즈니스 지표를 모니터링하고 분석한다. 고객의 비즈니스 지표를 기반으로 이상 탐지(Anomaly Detection)를 하여 고객에게 위험을 전달할 수 있도록 하는 것이 이 과제의 목표이다.  


<img src="/images/fulls/data_info.jpg" class="fit image">  


데이터는 다음과 같이 이루어져 있다. timestamp(float), value(int), is_anomaly(boolean), predicted(float). 여기서 포커싱해야할 column은 value와 predicted이다. value는 해당 timestamp에 대해 어떠한 지표를 측정한 실수값이며, predicted는 해당 timestamp에 대한 미지의 예측 모델이 추론하여 내놓은 예측값이다. 미지의 모델(black box)은 정상 데이터에 대한 분포만 알고 있고, 그를 기반으로 추론을 진행하는 모델이다. 마지막으로, is_anomaly는 데이터가 정상/비정상 여부를 나타낸다. 정상 데이터는 False, 비정상 데이터는 True로 label되어 있다. 당연하게도, 우리는 train set으로 학습과 검증을 하고, test set으로 최종 평가를 진행하기 때문에 train set에는 각 샘플마다 True or False를 확인할 수 있지만(지도학습) test set는 is_anomaly column만이 존재하며 공란으로 되어있다. (우리가 예측해야 한다!)  


<img src="/images/fulls/train_consist.jpg" class="fit image">  


train set은 정상 샘플 15,054개, 비정상 샘플 776개로 구성되어 있다. 점수는 test set에 대한 F1-score의 평균으로 결정되며, 모든 값을 False로 판단하는 것만으로도 95%의 정확도를 얻을 수 있다. 하지만 이는 바람직하지 않다. 고객은 정상 데이터보다 비정상 데이터에 더 관심을 갖기 때문이다.


## **무지성 도전**  


<img src="/images/fulls/data_corr.jpg" class="fit image">  


value와 is_anomaly column 간 상관관계가 가장 높음을 확인할 수 있다.  


<img src="/images/fulls/origin_data.jpg" class="fit image">  


좌측 그림은 시간에 따른 value, 우측 그림은 미지의 모델이 내놓은 예측값(given prediction)에 따른 value를 나타낸 것이다. 붉은색 점은 비정상, 파란색 점은 정상을 나타낸다. 앞서 상관관계에서 확인했듯이, 시간에 대한 상관도는 매우 옅기 때문에 given prediction과 value에 중점을 두고 모델이 추론할 수 있도록 데이터 전처리를 진행하겠다. 먼저, 모델은 아래와 같은 Pipeline을 거친다.  
  
1. 데이터 전처리  
2. Support Vector Classifier를 사용한 train set 학습 및 StratifiedKFold를 사용한 validation set 추론 (학습 및 검증)
3. fit된 SVC를 이용한 test set 추론  


<img src="/images/fulls/datapr_origin.jpg" class="fit image">  


가장 먼저 생각할 수 있는 것은 value와 predicted의 데이터를 집어넣어 학습하는 것이다. 학습 데이터를 (data["predicted"], data["value"])꼴로 바꾸고 str(boolean)로 이루어져 있는 is_anomaly를 0, 1로 바꿔준다. 그리고 가우시안 커널과 가중치를 적용한 SVC를 사용할 것이기 때문에 미리 weight를 초기화한다. 왜 가중치를 적용한 SVC를 사용하냐? 지도학습의 특성에 의해, 적어도 train set의 정상/비정상 비율을 알고 있기 때문에 효율적인 학습을 위해 이와 같이 진행한다.  


<img src="/images/fulls/origin_fit.jpg" class="fit image">  


ROC 곡선 아래의 영역이 0.933으로 언뜻 보기에 높은 값이 산출되었다. 하지만 높은 수치는 곧 과대적합(overfitting)이라는 신호가 될 수 있기 때문에 벌써 좋아하기는 이르다.  


<img src="/images/fulls/origin_graph.JPG" class="fit image">
<img src="/images/fulls/origin_result.jpg" class="fit image">

좌측 그림은 추론을 거치지 않은, 주최자가 배포한 test set을 given prediction에 따른 value로 표현한 그래프이며 우측 그림은 모델의 추론 결과를 나타낸 것이다. 또한 총 3960개의 샘플로 이루어진 test set에 대하여 모델은 정상 샘플 3,923개, 비정상 샘플 37개라는 결과를 도출하였다. 결과를 주최측에 제출해보자. 0.98484로 20위 이하로 랭크될 수 있었다.**(끝난 competition이기 때문에 현재는 랭크가 안된다.)** 하지만 그저 20위 정도를 하려고, 혹은 그냥 아무 생각도 없이 데이터를 있는 그대로 집어 넣어 결과만 도출하는 것은 의미가 없다. 그리하여 어떻게 데이터 전처리를 해볼까 궁리해본 결과...  
  
  
## **지성 도전**  
  

<img src="/images/fulls/mv.jpg" class="fit image">


이동평균이란 무엇인가? 이동평균은 말 그대로 일정 length(보통 window라고 표현.)를 기준으로 전체 구간에서 특정 구간을 이동하며 window 크기만큼의 구간의 평균을 구하는 것이다. 이는 주식에서 시세 동향선을 나타내거나, 구간을 smoothing할 때 쓰일 수 있다. 이 구간이 time이라고 생각한다면, 연속적인 시간이 고려된 평균이라고 할 수 있을 것이다. (window 크기만큼의 시간 동안 구간의 평균) 앞서 우리는 시간과 is_anomaly 간 상관도가 낮은 것을 확인하였지만, time sequence 데이터이기 때문에 시간을 완전히 배제하는 것은 옳지 않다고 생각한다. 시간 영역에서의 이상치란, 시간의 흐름에 따라 임펄스마냥 갑자기 훅! 튀어버리는 값일 수 있다. 또한 주식으로 다른 경우의 예를 들어보면, 5년 동안 100원이었던 주가가 갑자기 10,000원이 되었을 때, "어? 이거 이상한데?"라고 생각할 수 있지만 10,000인 상태로 주가가 오래도록 지속된다면 우리는 이상하다고 생각하지 않을 것이다. **그리고** given prediction은 정상 데이터의 분포를 알고 있는 미지의 모델의 예측값이라고 했기 때문에 정상 샘플들에 대한 given prediction과 비정상 샘플들에 대한 given prediction에는 확연한 차이가 있을 것이다. 그리하여 필자는 given prediction("predicted" column에 해당하는 값들.)에 대해 window 11의 이동평균을 적용하겠다. a라는 값이 있다면 그 a라는 값과(+1), a라는 값 이전 5개의 값(+5), 그리고 a라는 값 이후의 5개의 값(+5 = 11)에 대한 이동평균을 구하고자 window의 크기를 11로 선정하였다. (기준이 되는 샘플 1개와 전후값 10개의 평균.)
  

<img src="/images/fulls/duplicate.jpg" class="fit image">  
  

위의 그림은 True인 value들과 False인 value들 간 중복되는 개수를 구한 것이다. 735개의 중복되는 샘플들이 존재한다. 이는 SVC의 학습을 조금 어렵게 할 것이다. 왜? 같은 20이라는 값을 두고 어떤 경우는 정상, 어떤 경우는 비정상이라고 할 것이기 때문에 이 중복되는 값들을 확실하게 분류해줄 수 있다면 모델의 학습에 크게 도움이 될 것이다. 
  
  
<img src="/images/fulls/datapr_code.jpg" class="fit image">  
<img src="/images/fulls/datapr_renew.jpg" class="fit image">  
  

중복되는 값들을 확실하게 분류할 수 있는 방법으로 "거리"를 떠올렸다. 각각의 샘플에 대하여, 원점으로부터 떨어진 거리((value, distance)의 좌표를 갖는.)로 표현한다면, 단 하나의 값도 중복되지 않는다. 이러한 사견들을 취합하여 각각의 샘플을 (원점으로부터의 거리, 크기 11의 이동평균)의 좌표 형태로 변환하였다. 좌측 그림은 앞서 첨부하였던 미지의 모델이 내놓은 예측값(given prediction)에 따른 value 그래프이다. 이번 변환을 통한 그래프(우측)와 비교하기 위하여 다시 첨부하였다. 완벽하진 않지만, 비정상 샘플들이 일직선에 가까운 배치임을 확인할 수 있다. 사람이 보기에도 학습하기에 더 쉬워보이기 때문에 이전에 행했던 **"무지성 도전"**보다 조금 더 나은 결과를 가져올 것이라고 생각된다.  
  

<img src="/images/fulls/rocauc.jpg" class="fit image">  
<img src="/images/fulls/final_graph.jpg" class="fit image">
  
  
ROC 곡선 아래의 영역이 0.915로 이전보다는 낮지만 0.90을 넘는 값이므로 준수한 수치라고 할 수 있다. 또한 이전 모델에 비해 추론의 결과를 나타낸 그래프에서의 각 벡터들이 좀 더 분류하기 쉽도록 퍼져 있음을 확인할 수 있다.  
  
  
## **얻은 것**
<img src="/images/fulls/second_score.jpg" class="fit image">  
  
  
제출 후 획득 점수 또한 2위권에 랭크될만큼 훌륭한 결과를 도출하였다. 뿌듯함도 뿌듯함이지만, 평소에 이런 대회를 접했을 때 가장 먼저 드는 생각은 "어떤 훌륭한 SOTA 모델을 가져와서 사용해야할까?" 혹은 "어떤 새로운 모델을 만들어야할까?" 였지만, 주어진 데이터 자체를 유의미하게 조작(?)하는 것이 원하는 바를 달성하기 위한 가장 쉽고 빠른 접근법이라는 것을 알게된 뜻깊은 도전이었다.  