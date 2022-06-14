---
layout: post
---
<img src="/images/fulls/chroma_feature.jpg" style="width:342px; height:300px;">  

# 기업 참여형 국가 연구 과제  
## (대외비 특성상 일부 서술)  
---
> 연구 보조원의 역할(참여율: 22.5%)로  
> '딥러닝을 활용한 고속 음악 탐색 기술 개발'이라는  
> 주제의 연구 과제에 6개월간 참여하였다.  

## 맡은 부분은 모델 개선 방안 도출  
## 모델 개발 보조, 그리고  
## Cover song 크롤링이다.  
<br/>
# 모델 개선 방안
---
## Adabound  
> 널리 쓰이는 optimizer인 'Adam' 대신  
> 'Adabound'의 활용을 제안하였다.  
> 최적화 과정에서, 시작은 Adam처럼 빠르고  
> 끝은 SGD처럼 일반화가 잘된다고 하여 석사 개인 연구에서  
> 개발한 모델에 사용한 결과, Adam을 사용한 결과보다  
> 미세하게 빠르지만 분류 성능이 더 좋게 도출되어  
> 제안하게 되었다.(일반화가 더 잘 된 결과.)

<img src="/images/fulls/dynamicbound.jpg" style="width:384px; height:225px;">  

> Adabound는 "Adaptive gradient methods with  
> dynamic bound of learning rate"라는 논문에서 제안된  
> 기법으로, 이전 optimizer들의 경우 learning rate가  
> 너무 크거나 작은 경우, 일반화 성능이 매우 떨어지거나  
> local minimum에 빠지는 문제등이 있다는 것을 고려하여  
> dynamic bound라는 기법을 제안하였다.  
> 이는 upper bound, lower bound를 통해 계속해서  
> clipping threshold를 줄여 나가며 bound를 점점 좁혀서  
> 최적의 learning rate를 찾아가는 방식이다.  

# 모델 개발 보조
--- 
> 모델 개발 시 오류가 있는 부분을 함께 수정하였으며,  
> 연구보조원(학우들)들의 전이학습 구현을 지원하였다.  
> MIT-BIH arrhythmia DB의 샘플들을  
> 모두 그래프 형식으로 저장하여  
> torchvision 라이브러리의 사전 학습된(pre-trained)  
> resnet50 모델의 전이학습을 진행하는 방식으로  
> 전이학습에 대한 지식을 전달하였고,  
> Convolutional Neural Network의  
> 전반적인 내용에 대한 지식 또한 전달하였다.    

# cover song 크롤링 
---
<img src="/images/fulls/pytube.jpg" style="width:320px; height:105px;"> 
> 파이썬 pytube 라이브러리를 사용하여 cover song과 원곡을 확보하였다.  