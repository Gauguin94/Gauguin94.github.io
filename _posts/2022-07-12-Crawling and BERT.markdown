---
layout: post
---
<img src="/images/fulls/adven.jpg" style="width:292px; height:111px;">  
<strong>바칼 - 죤씨나, 바티스따, 렌디오턴, 드웨인좐슨, 성게의버프</strong> 
  
## **제가 했던 프로젝트의 일부는**<br/>
## **좌측 'Project List'의 목록을 클릭하여**<br/>
## **확인하실 수 있습니다.**<br/>
## **(모바일은 상단)**<br/><br/><br/>
  
# 던파 커뮤니티 크롤링과 BERT(Transformer)
### 토이 프로젝트<br/>  
> ---
> 얼마 전 출시된 이스핀즈는 개인적으로 손에 꼽히는  
> 던전앤파이터 컨텐츠 중 하나라고 생각한다.  
> 기존 그로기 메타의 경우, 과장을 섞어 말해보자면  
> 몬스터가 "이제 나 때려보세요!"라는 느낌이 들어  
> 몰입감이 떨어지는 경우가 있었는데,  
> 이스핀즈는 그로기가 매우 짧고  
> 몬스터가 시전하는 긴 동작의 패턴이나 전조가 긴 패턴이  
> 곧 데미지를 넣는 타이밍으로 느껴져  
> 몬스터와 전투를 벌이고 있다는 느낌이 확실히 든다.  
> 몰입감으로만 따지면 역대 최고의 컨텐츠가 아닐까 싶다.  
> 던전앤파이터 커뮤니티 중  
> 가장 활성화된 커뮤니티 중 하나인, **그리고 가장 공격성이 짙은**  
> <span style='background-color: #fff5b1'>DC 인사이드 지하성과 용사 마이너 갤러리</span> 유저들의  
> 이번 패치 이후 민심은 어떨까?  
> 7월 7일부터 7월 10일까지의 민심을 살펴보기 위해  
> 크롤링을 진행하였다.  

## 저는 로봇이 아닙니다. 사람입니다.<br/>
> ---
> <img src="/images/fulls/cmodule.JPG" style="width:993px; height:431px;"> 
> 
> 지하성과 용사 마이너 갤러리를 크롤링하는 흐름은 아래와 같다.  
> <span style='background-color: #fff5b1'>clickPost</span>는 현실 공간에서,  
> 사람이 게시글을 클릭하는 행동을 모사한다.  
> <span style='background-color: #fff5b1'>extractPost</span>는 게시글을 클릭 후,  
> 사람이 글의 내용과 제목을 읽는 과정을 모사한다.  
> 마지막으로, <span style='background-color: #fff5b1'>savePost</span>는 글을 읽은 후,  
> 머릿 속에 그것들을 저장하는 과정을 모사한다.  
>  

```python  
    import csv
    import time
    import requests
    from random import randint
    from bs4 import BeautifulSoup

    MAX_SLEEP_TIME = 5
    PAGE_URL = "https://gall.dcinside.com/mgallery/board/lists/"
    POST_URL = "https://gall.dcinside.com/mgallery/board/view/"
    USER_AGENT = {'User-Agent' : "유출하면 안될 것 같습니다."}

    def clickPost():
        post_info = []
        deadline = 2022070711

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
  
> HTTP 응답이 온전치 않을 경우가 있어서 원문에는  
> try, except를 사용하였지만 가독성을 위해 본문에서는 제외하였다.  
> <img src="/images/fulls/im_human.JPG" style="width:443px; height:118px;">  
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
  
## WordCloud를 이용한 이스핀즈 관련 키워드 시각화  
> ---
> 앞선 크롤링 작업 후, 제목이나 내용 속에 '이스핀즈'란  
> 단어가 포함되어 있는 글만 선별하는 작업을 수행하였다.  
> 이스핀즈 패치 이후 이스핀즈와 같이 언급되는 단어 중  
> 가장 많이 출현하는 단어는 무엇일까?  
> 이를 확인해보고 시각화해보기 위해  
> 한국어 정보 처리를 위한 파이썬 패키지 'konlpy'와  
> 'WordCloud'를 사용하였다.  

```python
    import pandas as pd 
    from konlpy.tag import Okt
    from collections import Counter
    from tqdm import tqdm
    from wordcloud import WordCloud

    STOPWORDS = ['그냥', '캐릭', '지금', '진짜', '이번', '던전', '정도', '시즌', '생각', '존나', '던파', '시스템', '어차피', '무조건',
                 '새끼', '하나', '유저', '사람', '소모', '게임', '이상', '한번', '시발', '보고', '패치', '병신', '시간', '입장', '기준', '씨발', '이유', '때문',
                 '대충', '가능', '거의', '이벤트', '차이', '계속', '자체', '오늘', '다시', '사실', '지랄', '가면', '조금', '나머지', '상황', '따리', '느낌', '수준', '신청',
                 '부위', '일단', '원금', '추가', '시작', '얼마나', '캐릭터', '상태', '여기', '출시', '진입', '다른', '페이', '제일', '원래', '가기', '다음', '정상',
                 '마리', '가야', '제외', '어케', '의미', '그거', '오히려', '잡고', '거기', '이면', '공격', '처음', '하루', '해도', '삭제', '언제', '갈수', '거임',
                 '치면', '만하', '팩트', '페이지', '제발', '대신', '싸개', '이건', '해결', '가도', '아예', '이제', '금방', '전부', '거지', '소리', '건가', '마냥',
                 '개월', '부분', '잘만', '런가', '누가', '출발', '그대로', '본인', '그게', '절대', '념글', '제대로', '최소', '유효', '선택', '효율', '채용', '굳이', '쓰레기',
                 '기존', '저번', '질문', '무난', '실제', '분탕', '고민', '새끼', '어제', '피하', '어디', '안보', '바로', '추천', '영상', '플레이']
    TARGET_POS = ['Noun', 'Unknown']

    if __name__ == "__main__":
        okt = Okt() # 객체 생성
        df = pd.read_csv('./datasets/testset.csv', encoding='utf-8') # 데이터 불러오기

        pos_set = []
        for sentence in tqdm(df['text']): # 문장을 품사로 나눈다.
            pos_set.append(okt.pos(sentence))

        word_set = []
        for pos_ in tqdm(pos_set): # 나뉜 품사 중 명사, 알수없음에 해당하는 단어만 추출한다.
            for word, tag in pos_:
                if tag in TARGET_POS:
                    word_set.append(word)

        target_set = []
        for num, word in tqdm(enumerate(word_set)): # 추출된 단어 중 본인이 생각하기에 불용어나, 한 글자로 이루어진 단어, 실명이 언급되는 단어는 제외한다.
            if word not in STOPWORDS:
                if len(word)>1:
                    target_set.append(word)

        count = Counter(target_set) # 단어 출현 빈도를 확인한다. 빈도별 내림차순으로 저장한다.
        noun_list = count.most_common(150) # 150개까지만 저장하겠다.
        
        # wordcloud 그리기 및 저장
        wc = WordCloud(font_path='./datasets/08SeoulNamsanB_0.ttf',
                        background_color='white',
                        width=1000,
                        height=1000,
                        max_words=150,
                        max_font_size=250)

        wc.generate_from_frequencies(dict(noun_list))
        wc.to_file('wordcloud_dnf.png')
        print('fin!')
```  

> 먼저, STOPWORDS는 필자 생각하기에  
> 불용어에 해당하거나 제외해야할 단어를 모은 것이다.  
> 개인의 이름이나 현 상황을 나타낸다기에 애매한 단어,  
> 욕설을 제외하였다.  
> TARGET_POS는 konlpy를 통해 추출되는 한글 중  
> 원하는 품사만을 확인하기 위한 것이다.  
> 명사와 '알 수 없는(게임 용어?)'이란 카테고리에  
> 존재하는 단어들만을 확인하도록 한다.  
> 그리고 던전앤파이터 관련 용어는 당연히 konlpy가  
> 분류하거나 찾을 수 없을 것이기 때문에  
> 이를 직접 넣어주도록 한다.  
> (이스핀즈를 '이', '스핀', '즈'로 반환한다...)  
>  
> <img src="/images/fulls/new_word.JPG" style="width:744px; height:324px;">
>  
> 먼저 konlply가 존재하는 디렉토리를 찾는다.  
> 그리고 표시되어있는 jar파일을 클릭하여  
> 표시되어있는 txt파일에 직접 던전앤파이터 관련 용어들을  
> 기입해주면 konlpy가 해당 단어들에 대해 인식 가능하게 된다!  
>  
> <img src="/images/fulls/wordcloud_dnf.png" style="width:1000px; height:1000px;">
>  
> 글씨 크기가 클수록 출현 빈도가 높은 단어이다.  
> 앞서, '이스핀즈'라는 단어가 포함되어 있는 글만  
> 선별하였기 때문에 이스핀즈가 가장 크게 자리잡고 있음을  
> 확인할 수 있다.  
> 이스핀즈 업데이트 이후, 파티를 구성하기 위한  
> 딜러의 수가 모자라다는 의견이 대다수였는데,  
> 확실히 '딜러', '버퍼', '딜러난'이란 단어의 출현 빈도가  
> 높음을 확인할 수 있다.  
> 또한 이번 시즌의 특징 중 하나로,   
> 솔로 플레이에 대해 많은 개선으로 인해  
> 솔로 플레이를 적극적으로 즐기고 있는 유저들이 많다.  
> 그렇기 때문에 '솔플'이란 단어도 매우 크게 자리잡고  
> 있는 것을 확인할 수 있다.  
  
## BERT를 이용한 던파 커뮤니티 게시글 분류  
> ---
> "Attenion is all you need"라는 페이퍼의 등장 이래로  
> attention과 encoder, decoder를 기반으로 한  
> Transformer는 문자영역에서 Computer Vision 영역에 이르기까지,  
> 정말 다양한 분야에서 사용되며 발전하고 있다.  
> 2018년 구글에서 공개한 pre-trained model인  
> Multilingual BERT를 사용하여 앞서 수집한 글에 대해 분류를 시도해보자!  

## 두 가지 방법  
> ---
> <img src="/images/fulls/dnf_review.JPG" style="width:671px; height:294px;">
>  
> 지하성과 용사 마이너 갤러리 크롤링을 통해  
> 얻은 글은 대략 15,000개 정도이다.  
> 이 중에서 '이스핀즈'라는 키워드가 들어가는 글은  
> 대략 2,700개 정도이며,  
> 이 중에서 현재 패치에 대한 감정(만족, 불만족)을  
> 드러내는 글을 개인적으로 선별한 결과  
> 571개의 글만이 남게 되었다.  
> 전이학습이 적은 데이터셋에 대해서도  
> 좋은 결과를 낳을 수 있다고 하지만,  
> 개인적으로 너무나 작은 규모의 데이터라고  
> 생각했기 때문에 그나마 데이터의 성격이 비슷하다고 할 수 있는  
> [네이버 영화 리뷰(NSMC)](https://github.com/e9t/nsmc)를  
> 학습에 함께 사용하였다.(150,000개)  
> <img src="/images/fulls/nsmc_review.JPG" style="width:682px; height:265px;">

### 1. 두 개의 데이터 셋을 합쳐보자.  
> ---
> <img src="/images/fulls/summation.JPG" style="width:753px; height:448px;">  
>  
> 먼저, 지하성과 용사 마이너 갤러리 크롤링에서 선별한  
> 글 571개중 100개를 평가용 데이터로 미리 제외시켰다.  
> 그리고 네이버 영화 리뷰 데이터 150,000개와  
> 크롤링 데이터 471개를 합쳐 150,471개의  
> 학습용 데이터셋을 구성하였다.  
  
```python
nsmc = pd.read_csv("./datasets/ratings_train.txt", sep='\t').drop(['id'], axis=1)
dnf = pd.read_csv("./datasets/testset.csv", encoding='cp949').dropna()
dnf_train = dnf[:471]
test = dnf[471:571]

temp = pd.DataFrame()
for i in range(4):
    temp = temp.append(dnf_train, ignore_index=True)
train = nsmc.append(temp, ignore_index=True)
train = train.astype({'label':int})

# ~~ 중략(토크나이징, 패딩, 어텐션 마스킹) ~~#

train_inputs, validation_inputs, train_labels, validation_labels = train_test_split(input_ids, train['label'].values, random_state=42, test_size=0.1)
train_masks, validation_masks, _, _ = train_test_split(attention_masks, input_ids, random_state=42, test_size=0.1)

```  
  
> <img src="/images/fulls/sum_set.JPG" style="width:630px; height:338px;">  
>  
> 그리고 Tokenizing, Padding, Attention Masking을  
> 데이터에 적용하고 학습용 데이터를  
> 학습 및 검증 데이터로 나눈 뒤 자료형을 텐서로 변환한다.  
>  
> <img src="/images/fulls/bert_pre.JPG" style="width:873px; height:482px;">  
  
```python
model = BertForSequenceClassification.from_pretrained("bert-base-multilingual-cased", num_labels=2)
model.cuda()
```  
  
> 감정(만족, 불만족)에 따라 분류하는 모델이 목표이기 때문에  
> 2 종류의 label로 Classification을 진행하는 버전의  
> pre-trained BERT를 불러온 뒤, 학습 및 평가를 진행한다.  
>  
> <img src="/images/fulls/1st_result.JPG" style="width:460px; height:282px;">  
>  
> 결과는 생각보다 나쁘지 않았다.  
> 비교 대상이 없어서 객관적인 평가를 내리긴 어렵겠지만,  
> 지하성과 용사 마이너 갤러리를 크롤링하고 선별한 뒤  
> 선별한 글을 모두 읽으며 라벨링을 직접 하면서  
> 너무도 애매하다고 생각되는 글이 많았다.  
> 예를 들어,  
> "평소에는 할게 없는데 이스핀즈는 매주 기다려질듯?"  
> 문장 안에 긍정과 부정이 섞여있는 다수의 글과  
> 비아냥(?)거리는, 혹은 돌려 말하는 글이 다수 존재하여  
> 라벨링을 하는 필자도 어떻게 라벨링을 해야할지  
> 판단이 어려웠으며 아직까지도 제대로 라벨링을  
> 진행한게 맞는지 아리송한 면이 있는데,  
> 그러한 데이터에 대해 위와 같은 결과가 나왔다는 것에  
> 정말 놀라웠다. (필자는 acc, auc 0.50 정도를 예상하였음.)  
>  
### 2. 두 번의 Fine-tuning을 진행해보자.  
> ---  
> <img src="/images/fulls/tuning_twice.JPG" style="width:1381px; height:705px;">  
>  
> 앞선 모델을 구현하며, 문뜩 이런 생각이 들었다.  
> 150,000개의 데이터에 471개의 데이터를 추가해서  
> 150,471개의 데이터가 되었다.  
> 목표는 지하성과 용사 마이너 갤러리의 글을  
> 분류하는 것인데, 150,471개의 데이터 중  
> 단 471개의 데이터를 추가하는 것만으로  
> 지하성과 용사 마이너 갤러리의 성향이 충분히 반영될 수 있을까?  
> <span style='background-color: #fff5b1'>그리하여 필자는 가설을 세우고 접근해보았다.</span>  
> 두 번의 Fine-tuning을 진행해보면 어떨까?  
> 1차 Fine-tuning에서는 NSMC 150,000개의 데이터 학습을 통해  
> "이것이 리뷰에 대한 이진 분류(Binary classification)이다."라는 것에  
> 모델로 하여금 포커싱하게 한 뒤,  
> 2차 Fine-tuning에서는 크롤링한 데이터 471개로 학습을 진행한다면  
> "리뷰"라는 점에 대해 1차 Fine-tuning에서  
> 어느 정도 포커싱되었기 때문에  
> 적은 데이터로 학습이 이루어지더라도  
> optimization이 앞선 결과보다 좋게 이루어지지 않을까?  
>  
> <img src="/images/fulls/2nd_result.JPG" style="width:453px; height:277px;">  
>  
> 가설을 필두로 새로이 구현해본 결과  
> 앞선 결과보다 향상된 성능을 보였다!  
> 일부 성능 평가에서 인간보다  
> 더 높은 정확도를 보인다는 BERT.  
> 제대로된 라벨링이 되어있다고 보장할 수 없는  
> 데이터셋에 대해서도 설계자의 예측보다  
> 더 좋은 성능을 낸다는 점을 직접 보니  
> 일부 성능 평가보다 좋다는 말에서  
> "일부"라는 표현이 무색해지는 것 같다.  
  
## 끝으로  
> ---  
>  
> <img src="/images/fulls/sesame_bert.JPG" style="width:473px; height:867px;">  
>  
> BERT는 다양한 특징 및 기법을 사용하고 있다.  
> 먼저, 학습이 되어있지 않은 단어라고 하더라도  
> 현재 학습을 하는 과정에서 자주 등장하는 단어를  
> 단어로 학습하는 Wordpiece Embeddings,  
> 단어의 위치 정보를 처리하는 Positional Encodings,  
> 그리고 양방향성 등을 지니고 있다.  
> 이러한 여러 기법들은 BERT로 하여금  
> 다음에 위치할 문장, 혹은 단어에 대해 좋은 예측 성능을  
> 지니도록 만들었으며, 같은 단어가 존재하는 문장이라도    
> 문맥에 따라 긍정/부정 혹은 다른 카테고리들로  
> 분류를 잘 할 수 있도록 만들어줬다.  
> 이러한 BERT의 장점을 살려서(욕설주의)  
> "와 아이템 떴어 ㅈㄴ 행복해!"  
> "아 님 왜 그럼? ㅈㄴ 짜증나게 하네."  
> 같은 "ㅈㄴ"란 욕이 들어가더라도  
> 추임새(?)에 가까운, 남에게 향하지 않는 욕설과  
> 남에게 향하는 욕설을 분류하는 작업을 진행해보고 싶다.  
> (물론 욕설은 어느 상황에서든 사용하지 않는게 제일 좋습니다...)