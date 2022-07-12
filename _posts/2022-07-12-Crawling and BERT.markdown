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
``` python  
    import time
    import requests
    from bs4 import BeautifulSoup

    PAGE_URL = "https://gall.dcinside.com/mgallery/board/lists/"
    USER_AGENT = {'User-Agent' : "유출하면 안될 것 같습니다."}

    def clickPost():
        post_info = []
        deadline = 2022070711
        error_count = 0

        for page_num in range(1, 450):
            params = {'id':"dnfqq", 'page':f'{page_num}'}
            response = requests.get(PAGE_URL, params=params, headers=USER_AGENT)
            soup = BeautifulSoup(response.content, "html.parser")
            contents = soup.find('tbody').find_all('tr') # 게시글 정보(게시글 별 제목, 주소(href))

            for post_ in contents:
                # ~~(페이지마다 존재하는 공지글 제외하는 부분 생략)~~
                rand_value = randint(1, MAX_SLEEP_TIME) 
                time.sleep(rand_value)

                title = post_.find('a').text # 게시글 제목 추출
                post_params = {'id':"dnfqq", 
                               'no':post_.attrs['data-no'], 
                               'page':f'{page_num}'}
                writing, date = extractPost(post_params) # 게시글 속 내용 추출 함수로 넘어감
                post_info.append([title, writing]) # 게시글 별 제목과 내용
                if date < deadline: # 이스핀즈 업데이트 당일 11시 이전 내용은 수집하지 않겠다.
                    break

        savePost(post_info) # 크롤링한 것 전부 csv 파일로 저장

    def extractPost(post_params):
        post_response = requests.get(POST_URL, params=post_params, headers=USER_AGENT)
        post_soup = BeautifulSoup(post_response.content, "html.parser")
        contents = post_soup.find('div', class_='write_div') # 게시글 내용 추출
        date = post_soup.find('span', class_='gall_date').text.replace(' ','').replace('.', '')[:10]
        date = int(date)
        return contents.text, date

    def savePost(post_info):
        feature = ['title', 'text']
        with open('testset.csv', 'w') as f:
            write = csv.writer(f)
            write.writerow(feature)
            write.writerows(post_info)
        print("save fin!")
```  
> 위 코드는 지하성과 용사 마이너 갤러리 크롤링을 수행하는 코드이다.  
> https 응답이 온전치 않을 경우가 있어서 원문에는  
> try, except를 사용하였지만 가독성을 위해 본문에서는 제외하였다.  
> 이전 Dunfamoa 크롤링과 크게 다른 점이 있다면,  
> User-Agent와 time.sleep()을 사용했다는 것이다.  
> DC 인사이드는 로봇으로 의심되는 유저를 접속 제한시켜  
> 외부의 공격이나 서버의 부하를 막는다.  
> 필자는 이런 점을 간과한 채로 크롤링을 시도하여  
> 20분간 접속 제한 상태가 되었다.  
> 그리하여 HTTP 요청을 하는 유저의  
> 주민등록증이라고 할 수 있는 User-Agent를 사용하였다.  
> 그리고 의도치 않은 서버 부하를 피하기 위해,  
> 로봇이 아닌 사람이라는 것을 어필하기 위해  
> 1~5초 사이 임의로 요청을 보내도록  
> time.sleep을 사용하였다.  