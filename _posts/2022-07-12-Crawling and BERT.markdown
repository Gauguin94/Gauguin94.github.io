---
layout: post
---
<img src="/images/fulls/adven.jpg" style="width:292px; height:111px;">  
<strong>바칼 - 죤씨나, 바티스따, 렌디오턴, 드웨인좐슨, 성게의버프</strong> 

## **제가 했던 프로젝트의 일부는**<br/>
## **좌측 'Project List'의 목록을 클릭하여**<br/>
## **확인하실 수 있습니다.**<br/>

# 던파 커뮤니티 크롤링과 BERT(Transformer)
## 토이 프로젝트<br/>  
> ---
> 얼마전 출시된 이스핀즈는 개인적으로 손에 꼽히는  
> 던전앤파이터 컨텐츠 중 하나라고 생각한다.  
> 기존 그로기 메타의 경우, 과장을 섞어 말해보자면  
> 몬스터가 "이제 나 때려보세요!"라는 느낌이 들어  
> 몰입감이 떨어지는 경우가 있었는데,  
> 이스핀즈는 그로기가 매우 짧고  
> 몬스터가 시전하는 긴 동작의 패턴 시간이  
> 곧 데미지를 넣는 타이밍으로 느껴져  
> 몰입감으로만 따지면 역대 최고의 컨텐츠가 아닐까 싶다.  
> 그렇다면, 던전앤파이터 커뮤니티 중  
> 가장 활성화된 커뮤니티 중 하나인  
> <span style='background-color: #fff5b1'>DC 인사이드 지하성과 용사 마이너 갤러리</span> 유저들의  
> 이번 패치 이후 민심은 어떨까?  
> 7월 7일부터 7월 10일까지의 민심을 살펴보기 위해  
> 크롤링을 해보자.  

## 저는 로봇이 아닙니다. 사람입니다.<br/>  
> ---
> <img src="/images/fulls/cmodule.JPG" style="width:472px; height:216px;"> 
> 
> 시작하기 앞서, 지하성과 용사 마이너 갤러리를 크롤링하는 흐름은 아래와 같다.  
> <span style='background-color: #fff5b1'>clickPost</span>는 현실 공간에서,  
> 사람이 게시글을 클릭하는 행동을 모사한다.  
> <span style='background-color: #fff5b1'>extractPost</span>는 게시글을 클릭 후,  
> 사람이 글의 내용과 제목을 읽는 과정을 모사한다.  
> 마지막으로, <span style='background-color: #fff5b1'>savePost</span>는 글과 제목을 읽은 후,  
> 머릿 속에 그것들을 저장하는 과정을 모사한다.  