---
layout: post
---
<img src="/images/fulls/sketchup.jpg" style="width:513; height:143px;">  

# 기업 참여형 국가 연구 과제  
## (대외비 특성상 일부 서술)  
---
> '스마트폰 기반 사물인식 및 Waypoint를 적용한  
> 농축산 E-mobility 상용화 기술 개발'이라는 주제의  
> 연구 보조원의 역할로 연구 과제에 2년간
> 참여하였다.(1년 차 참여율: 44.4%, 2년 차 참여율: 40.3%)
## 맡은 부분은 경로계획 알고리즘 개선,  
## CadMapper와 SketchUp을 이용한 지도 생성,  
## 그리고 모듈 통합 및 검증이다.  

# 경로계획
---
> 경로계획 알고리즘은 최적 또는 최적에 가까운 경로 비용으로,  
> 출발 상태에서 도착 상태까지 충돌로부터 자유로운  
> 경로를 찾는 것을 목표로 한다.  
> 여기서 상태는 위치, 방향, 속도 등을 말하며,  
> 경로는 출발 상태와 도착 상태를 연결하는  
> 일련의 상태들이라고 말할 수 있다.  
> 경로계획 알고리즘은 접근 방식에 따라  
> Cell Decomposition, Control-Based  
> Potentila Field, Sampling 기반의 방식 등이 존재한다.  
> 이 중 복잡하고 높은 차원의 문제에 대한 해를  
> 적은 계산량으로 산출할 수 있는  
> Sampling 기반의 경로 계획 방식이 선호된다.  

<img src="/images/fulls/rrtstar_ex.jpg" style="width:411px; height:491px;"> 

> 샘플링 기반의 경로계획은 형상공간(configuration space)을  
> 격자(grid)로 분할하지 않고,  
> 랜덤하게 샘플링 하여 샘플점(sample point)을  
> 여러 개 생성하고, 점점이(point-wise) 공간을  
> 탐색하여 경로를 찾아내는 방법이다.  
> 즉, 형상공간 내에서 충분한 수의 샘플점을 무작위로  
> 발생시키고 발생된 샘플점, 혹은 두 개의 샘플점을 잇는 선과  
> 장애물의 충돌 여부를 확인하여 자유공간의  
> (free space, 충돌로부터 자유로운 공간)  
> 구조를 효율적으로 파악한다.  
## RRT*
---
<img src="/images/fulls/rrtstar_pseudo.jpg" style="width:567px; height:350px;"> 

>> 경로계획에 사용되는 알고리즘은 RRTstar로 선정하였다.  
>> RRTstar는 RRT(Rapidly-exploring Random Tree) 알고리즘의  
>> 개선된 버전으로, 트리 재생성 및 연결(tree rewiring),  
>> **최근접 이웃 탐색(Nearest Neighbor Search)**이라고 하는  
>> 기능을 도입하여 경로 탐색 품질을 개선한 경로계획 알고리즘이다.  

<img src="/images/fulls/path_ex.jpg" style="width:189px; height:189px;">

>> 완성된 경로계획 모듈을 시연하기 위하여  
>> 만들어진, 혹은 제공된 지도에서 경로계획이 적용되는 로봇은  
>> 점 로봇(point robot)으로 모델링하였다.(SketchUp 이용)  
>> 점 로봇으로 경로를 탐색하면 상태가 로봇의 위치와 방향으로,  
>> 즉, (x좌표값, y좌표값, 차량방향)의 세 값으로  
>> 단순화된다는 장점이 있다.  
  
<img src="/images/fulls/bspline.jpg" style="width:307px; height:202px;">  

>> 하지만 생성되는 경로의 변화가 심하고 차량의 방향이  
>> 특정되지 않는 문제가 있어  
>> B-spline smoothing을 사용하였다.  
>> B-spline smoothing은 생성된 경로를  
>> 차량 동역학이 적용된 경로와 비슷하게 만들어준다.  