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
> 몬스터가 시전하는 긴 동작의 패턴이나 전조가 긴 패턴이  
> 곧 데미지를 넣는 타이밍으로 느껴져  
> 몬스터와 전투를 벌이고 있다는 느낌이 확실히 든다.  
> 몰입감으로만 따지면 역대 최고의 컨텐츠가 아닐까 싶다.  
> 또한 유저에게 들어오던 데미지가 기존 체력 퍼센트 데미지 구조에서  
> 일반 데미지 구조로 변경되며 피격 시 피격 데미지가 특정 속성으로  
> 변환되는 마법석을 착용했을 경우,  
> 몬스터의 모든 공격에 히트앤런을 하던,  
> 아웃복서라는 직업명이 더 적절했던 인파이터가  
> 높은 피격 데미지 감소로 인해 진정한 인파이터로  
> 거듭났다는 점에서 "인파이터"라는 직업에  
> 더욱 몰입할 수 있는 컨텐츠라고 생각한다.  
> **반면에**, 던전앤파이터 커뮤니티 중  
> 가장 활성화된 커뮤니티 중 하나인  
> <span style='background-color: #fff5b1'>DC 인사이드 지하성과 용사 마이너 갤러리</span> 유저들의  
> 이번 패치 이후 민심은 어떨까?  
> 7월 7일부터 7월 10일까지의 민심을 살펴보기 위해  
> 크롤링을 진행하였다.  

## 저는 로봇이 아닙니다. 사람입니다.<br/>
> ---
> <img src="/images/fulls/cmodule.JPG" style="width:472px; height:216px;"> 
> 
> 시작하기 앞서, 지하성과 용사 마이너 갤러리를 크롤링하는 흐름은 아래와 같다.  
> <span style='background-color: #fff5b1'>clickPost</span>는 현실 공간에서,  
> 사람이 게시글을 클릭하는 행동을 모사한다.  
> <span style='background-color: #fff5b1'>extractPost</span>는 게시글을 클릭 후,  
> 사람이 글의 내용과 제목을 읽는 과정을 모사한다.  
> 마지막으로, <span style='background-color: #fff5b1'>savePost</span>는 글을 읽은 후,  
> 머릿 속에 그것들을 저장하는 과정을 모사한다.  
>   
> ### clickPost  
>> ---  
>> <img src="/images/fulls/dnfqq_address.JPG" style="width:320px; height:34px;">
``` python
import time
import requests
from bs4 import BeautifulSoup

PAGE_URL = "https://gall.dcinside.com/mgallery/board/lists/"
USER_AGENT = {'User-Agent' : "유출하면 안될 것 같습니다."}

def clickPost():
    # ~~(초기화 부분 생략)~~
    for page_num in range(1, 450):
        params = {'id':"dnfqq", 'page':f'{page_num}'}
        response = requests.get(PAGE_URL, params=params, headers=USER_AGENT)
        soup = BeautifulSoup(response.content, "html.parser")
        contents = soup.find('tbody').find_all('tr') # 게시글 정보(anchor등을 갖고 있다. 'a')

        for post_ in contents:
            # ~~(페이지마다 존재하는 공지글 제외하는 부분 생략)~~
            rand_value = randint(1, MAX_SLEEP_TIME) 
            time.sleep(rand_value) # 저는 사람입니다!

            title = post_.find('a').text # 게시글 제목 추출
            post_params = {'id':"dnfqq", 'no':post_.attrs['data-no'], 'page':f'{page_num}', headers=USER_AGENT}
            writing, date = extractPost(post_params) # 게시글 속 내용 추출 함수로 넘어감
            post_info.append([title, writing]) # 게시글 별 제목과 내용

    savePost(post_info, file_num) # 크롤링한 것 전부 csv 파일로 저장
```
>> 위 사진을 보면, id가 dnfqq임을 확인할 수 있다.  
>> params에 이를 저장하여 get 요청 시 활용하도록 한다.  
>> 또한 page_num을 1~450까지 증가시키는 부분을 확인할 수 있는데,  
>> 이는 1 페이지부터 450 페이지까지의 게시글을 확인하기 위해  
>> 선언한 것이라고 생각하면 된다.  
>> User-Agent를 비유해보자면,  
>> 서버에게 요청을 보낼 때 자신이 로봇이 아니고 사람임을  
>> 한 번 귀띔해준다는 뜻으로 생각하면 된다.  
>> [User-Agent 확인하는 사이트](http://m.avalon.co.kr/check.html) <- 클릭  
>> <img src="/images/fulls/tr_tbody.JPG" style="width:681px; height:157px;">
>> <img src="/images/fulls/a_tag.JPG" style="width:674px; height:48px;">
>> 이제 어떤 태그를 찾아야 게시글을  
>> 불러들일 수 있는지 확인해야한다.  
>> 첫 번째 사진을 보면, **'tr'**이라는 태그와  
>> 게시글이 연관되어 있음을 확인할 수 있다.  
>> soup.find('tbody').find_all('tr')을 사용하여  
>> 게시글 관련 정보(제목, href(URL))를 불러오도록 하였다.  
>> (두 번째 사진)  
>> 불러온 게시글 관련 정보는 **'contents'**에 담아두었다.  
>> <img src="/images/fulls/post_address.JPG" style="width:475px; height:33px;">
>> 이제 각 게시물을 확인하기 위해 위 사진에서 볼 수 있는  
>> 'no'를 확인하는 과정을 수행한다.  
>> 'no'는 위 코드에서 post_.attrs['data-no']를 통해 추출할 수 있었다.  
>> (**이미 contents에서 'a href'를 확인 가능한데 왜 이렇게 했냐?**
>> 돌아가는 길이지만 다른 방법도 사용해보고 싶었다.)  
>>