---
layout: post
---
<img src="/images/fulls/johncena.png" style="width:200px; height:230px;">

# 시간대별 강화 성공 비율을 확인해보자.
<img src="/images/fulls/yd_log.jpg" style="width:706x; height:363px;">  

친구 중에 정말 불쌍한 친구가 있었다...  
이 친구는 13강화 도전을 36번 실패했다...  
**(37번째 시도에 성공함.)**  
친구가 33번째 즈음 실패했을 때,  
친구가 너무 불쌍해서 전혀 근거는 없지만 13강화나  
그 이상의 강화를 성공하는 사람들은  
어느 시간대에 성공하는지 참고라도 할 수 있도록 로그를 뽑아보았다.  

# 던파모아에서 모험단 및 모험단 소속 캐릭터 이름 끌어오기.
```python
LETTER = [
    'SaeronHolic', '%ec%a0%84%eb%87%8c%ec%86%8c%eb%85%80%eb%8b%a8', 'loveintheice', '%eb%8f%84%ec%9c%a0TV', '%ea%b0%a4%eb%9f%ad%ec%8b%9cNote',\
    'NOHOLD', '%ea%b3%b5%ea%b0%90%ec%98%81%ec%96%b4', '%eb%aa%a8%eb%8b%88%ec%b9%b4%ed%95%9c%ec%83%81%ec%9e%90', '%ec%ac%b4',\
    '%eb%9d%b5%eb%b0%9c%ec%82%ac%ec%83%9d%ed%8c%ac6%ed%98%b8', '%eb%8b%b9%ea%b7%bc%eb%8b%b9%ea%b7%bc', '%ed%8f%ac%eb%8f%84%ec%a6%99%eb%86%8d%ec%9e%a5',\
    '%ec%9e%90%eb%b0%a9%ec%9d%b4%eb%84%a4', '%ec%b9%b8%ec%a1%b0%ec%bf%a0', '%eb%aa%a8%ec%82%b4%eb%8c%80', '%ec%a7%80%ed%99%98%ec%a7%b1',\
    '%eb%aa%85%ec%98%88%ed%9b%88%ec%9e%a5%ec%9e%84%eb%8b%a4', '%ec%97%90%ec%9d%b4%ec%a0%84%ed%8a%b8%ec%82%bc%ec%8b%9d', 'collet',\
    '%ec%a7%b1%ec%9d%b5%ec%a0%84%ec%9a%a9%ea%b2%8c%ec%9e%84', '%ec%9a%94%ea%b8%b0%eb%b2%a0%eb%9d%bc', '%ea%b7%b8%ec%9d%b8%ed%8c%8c',\
    '%ec%82%ac%eb%9e%91', '%ea%b8%b0%ec%96%b5', '%ec%b6%94%ec%96%b5', '%ec%9d%b4%eb%b3%84', '%ec%a7%80%ec%a1%b4', '%ec%a0%84%ec%82%ac',\
    '%eb%a1%9c%ea%b7%b8', '%ec%8a%a4%ed%95%8f', '%ec%9d%b8%ed%8c%8c', '%ec%8a%a4%ec%bb%a4', '%ec%8a%a4%ed%8c%8c', '%eb%84%a8%eb%a7%88',\
    '%ea%b7%b8%ed%94%8c', '%ed%86%a0%eb%84%a4', '%ea%b8%b0%eb%a6%b0', '%ed%87%b4%eb%a7%88', '%ed%81%ac%eb%a3%a8', '%ec%96%b4%eb%b2%b5',\
    '%ed%99%80%eb%a6%ac', '%eb%b2%84%ed%94%84', '%eb%b2%84%ed%8d%bc', '%ed%8e%b8%ec%95%a0', '%eb%9d%bc%ed%95%8c', '%ec%9d%b4%eb%a6%84',\
    '%eb%8d%98%ed%8c%8c', '%eb%a7%88%ed%87%b4', '%eb%ac%bc%ed%87%b4', '%eb%ac%bc%ea%b3%b5', '%eb%a7%88%ea%b3%b5', '%ec%98%a4%ea%b3%b5',\
    '%ec%9e%a5%ea%b5%b0', '%ec%9e%a5%ea%b5%90', '%ec%82%ac%eb%a0%b9', '%ed%99%94%eb%a0%a5', '%eb%a7%88%eb%a0%a5', '%eb%a7%88%eb%b2%95',\
    '%ec%9a%b0%eb%9f%ad', '%eb%aa%a8%ea%b8%b0', '%eb%b0%94%eb%9e%8c', '%ec%82%ac%eb%9e%8c', '%eb%8b%88%ec%95%8c', '%eb%b0%94%ec%95%8c',\
    '%eb%94%94%ec%95%84', '%eb%a9%94%ed%94%bc', '%eb%af%b8%ec%b9%b4', '%ed%8b%b0%eb%a6%ac', '%ec%9e%84%ed%8e%98', '%eb%a7%90%ed%8b%b0',\
    '%ed%83%80%eb%9d%bd', '%ec%9e%90%eb%9e%91', '%ec%8b%a0%ed%99%94', '%ed%8c%8c%ec%9b%8c', '%eb%82%98%eb%9d%bd', '%eb%9d%b5%ec%a7%84',\
    '%eb%aa%85%ec%a7%84', '%ec%9c%a4%eb%aa%85', '%ec%9c%a4%eb%9d%b5', '%ed%8c%a8%ed%99%a9', '%ed%8c%a8%ec%99%95', '%ed%88%ac%ec%8b%a0',\
    '%eb%b2%9a%ea%bd%83', '%eb%82%98%ec%84%a0', '%eb%87%8c%ec%a0%88', '%eb%8b%8c%ec%9e%90', '%ed%83%88%ec%a3%bc', '%ea%b3%b5%ea%b2%a9',\
    '%ea%b7%80%ec%8b%a0', '%ea%b2%80%ec%8b%a0', '%eb%87%8c%ec%8b%a0', '%eb%a7%88%ec%8b%a0', '%eb%a7%88%ec%99%95', '%ec%a0%9c%ec%99%95',\
    '%ed%99%a9%ec%a0%9c', '%eb%8c%80%ec%99%95', '%eb%b8%9c%eb%af%b8', '%eb%b8%9d%eb%af%b8', '%ec%8a%a4%ed%83%80', '%ed%99%94%ec%8b%a0',\
    'god', 'black', 'dark', 'love', 'blue', 'red', 'yellow', 'green', 'sky', 'wind', 'water', 'fire', 'flame', 'bomb', 'air', 'hi', 'bye', 'iu', 'a'
]
# saeronholic, 전뇌소녀단, loveintheice, 도유TV, 갤럭시Note
# 공감영어, 모니카한상자, 쬴
# 띵발사생팬6호, 당근당근, 포도즙농장
# 자방이네, 칸조쿠, 모살대, 지환짱
# 명예훈장임다, 에이전트삼식, collet
# 짱익전용게임, 요기베라, 그인파
# 사랑, 기억, 추억, 이별, 지존, 전사
# 로그, 스핏, 인파, 스커, 스파, 넨마
# 그플, 토네, 기린, 퇴마, 크루, 어벵
# 홀리, 버프, 버퍼, 편애, 라핌, 이름
# 던파, 마퇴, 물퇴, 물공, 마공, 오공
# 장군, 장교, 사령, 화력, 마력, 마법
# 우럭, 모기, 바람, 사람, 니알, 바알
# 디아, 메피, 미카, 티리, 임페, 말티
# 타락, 자랑, 신화, 파워, 나락, 띵진
# 벚꽃, 나선, 뇌절, 닌자, 탈주, 공격
# 명진, 윤명, 윤띵, 패황, 패왕, 투신
# 귀신, 검신, 뇌신, 마신, 마왕, 제왕
# 황제, 대왕, 븜미, 븝미, 스타, 화신
```  
생각나는 두글자 단어들과 영단어의 리스트를 만들었다.  
던파모아에서 '사랑'이라는 키워드로 검색을 진행하면,  
'사랑', '사랑해', '사랑한다' 등의 '사랑'이라는 키워드가 들어가는  
모험단 이름이 최대 20개까지 출력된다.  
그리고 영단어의 경우도 아주 기초적이고 접근성이 쉬운,  
모험단 계정을 만들 때 자주 사용할만한 영단어로 구성하였다.  
각 단어들은 URL 인코딩을 사용하여 변환하였다.  
또한 던파모아에서 캐릭터 강화 부분에서 상위권에 위치해 있는 캐릭터들의  
모험단 계정을 LETTER라는 리스트로 선언하였다.  
각 모험단 이름들은 URL 인코딩을 사용하여 변환하였다.  
[URL 인코딩 변환 사이트](https://www.convertstring.com/ko/EncodeDecode/UrlEncode)
  
```python
SEARCH_URL = 'https://dunfamoa.com/characters/adventure?search='

if __name__ == "__main__":
    for num, letter in enumerate(LETTER):
        dunfaMoa(SEARCH_URL + letter, num).run()
```  
먼저, 앞서 만들었던 리스트 내 단어들로 dunfamoa 홈페이지에서 검색을 실시한다.  
모험단 검색을 통해, 모험단 내 계정 캐릭터의 이름들을 전부 가져온다.  

```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
from json import loads
from .consts import*

class dunfaMoa:
    def __init__(self, search_url, num):
        self.search_url = search_url
        self.filename = num
```  
dunfamoa class는 loadAdv, loadChar, saveServer, saveCharinfo, run 메소드로 구성되어 있다.  

```python
    def loadAdv(self):
        adv_url = []
        href_info = []
        sourcecode = urlopen(self.search_url, context=context, timeout = 600).read()
        soup = BeautifulSoup(sourcecode, "html.parser")
        for href in soup.find("div", class_="Wrapper-sc-1noqpd4-1 gfctoN").find_all("a"):
            href_info.append(href)
        for url in href_info:
            if 'href' in url.attrs and 'class' in url.attrs:
                if url.attrs['class'] == ['Row-sc-jrfk9d-1', 'hiKazl']:
                    adv_url.append(RAW_URL + url.attrs['href'])
```  
HTTP client 역할을 수행할 수 있는 라이브러리 중 하나인 urllib를 사용한다.  
client란 고객, 즉, 서비스(데이터)를 요청(Request)하는 역할을 하며  
server가 이 요청(Request)을 받아들여 응답(Response)을 한다.(요청한 데이터를 보내준다.)  
'href'와 'a'를 찾아 원하는 것을 추출한다.  
'a'는 anchor, 즉, 닻을 의미하고 'href'는 URL을 의미한다.  
그렇다면 ' a href = "주소" '가 의미하는 것은  
우리가 웹(바다) 서핑(surfing:파도타기)을 할 때, 닻을 내려 해당 주소에 정박하겠다는 의미가 된다.  
adv_url.append(RAW_URL + url.attrs['href']) 을 통해  
모험단에 매칭되는 주소들을 리스트로 저장한다.(RAW_URL은 consts.py에서 선언한 const.)  

```python
    def loadChar(self, adv_url):
        adv_link = []
        char_info = []
        for url in adv_url:
            try:
                sourcecode = urlopen(url, context=context, timeout = 600).read()
                soup = BeautifulSoup(sourcecode, "html.parser")
                for href in soup.find("div", class_ = 'Outer-sc-1cu42wh-3 kCIUEG').find_all("a"):
                    adv_link.append(href)
                for data in adv_link:
                    if 'href' in data.attrs:
                        temp = data.attrs['href'].replace("/characters/", "")
                        temp = self.saveServer(temp)
                        char_info.append(temp)
            except:
                continue
        return char_info
```  
앞서 저장한 주소들로 던파모아의 모험단 페이지로 접속하여 서버와 캐릭터의 이름을 저장한다.  

# 끌어온 캐릭터의 로그 요청하기(to Neople)
```python
    def getInfo(self):
        char_list = []
        error_count = 0
        decoded_list = self.dataLoad()
        http = urllib3.PoolManager()
        for time, hash in enumerate(decoded_list):
            try:
                url = 'https://api.neople.co.kr/df/servers/{}/characters?characterName={}&limit={}&wordType={}&apikey={}'\
                        .format('all', hash, '200', 'match', APIKEY)
                req = http.request('GET', url)
                search_info = loads(req.data.decode('utf-8'))
                if len(search_info['rows']) >= 1:
                    for individual_info in search_info['rows']:
                        if individual_info['level'] > FWLV:
                            char_list.append({"serverId":individual_info['serverId'], "characterId":individual_info['characterId'], "characterName":individual_info['characterName']})
                            print(time, individual_info['characterName'])
            except:
                error_count += 1
                print("API server down or Freak character. [Error Count : {}]".format(error_count))
        char_list = self.deldupl(char_list)
        filename = whatTime()
        f = open("/home/kkn/DNF_epic/data/{}_3.txt".format(filename),'w')
        for line in char_list:
            f.write(line['serverId']+" "+line['characterId']+" "+line['characterName']+'\n')
        f.close()
        print('Success! and Error count: {}'.format(error_count))
        return 0
```  
NEOPLE Open API를 호출한다.  
GET 방식의 요청을 하며, 100레벨 이하인 캐릭터들은  
105레벨 아이템을 획득하지 못했거나  
획득을 거의 하지 못했으리라는 가정 하에 제외시킨다.  
filter로 걸러지지 않고 남은 캐릭터들을 txt파일에 저장한다.  
(나중에 다시 검색할 수고를 덜기 위해.)  

```python
    def logSearch(self, char_info):
        # 이상 생략
                try:
                    http = urllib3.PoolManager()
                    url = 'https://api.neople.co.kr/df/servers/{}/characters/{}/timeline?limit={}&code={}&startDate={}&endDate={}&apikey={}'\
                        .format(char['serverId'], char['characterId'], LIMIT, REINFORCE, start_time, end_time, APIKEY)
                    req = http.request('GET', url)
                    info = loads(req.data.decode('utf-8'))
                    start = end
                    if len(info['timeline']['rows']):
                        for log in info['timeline']['rows']:
                            if '불사조' not in log['data']['itemName']:
                                if log['data']['itemRarity'] == '유니크' or log['data']['itemRarity'] == '레전더리' or log['data']['itemRarity'] == '에픽' or log['data']['itemRarity'] == '신화':
                                    temp.append({'before':log['data']['before'], 'result':log['data']['result'], 'date':log['date']})
                                    if log['data']['result'] and (log['data']['before'] >= 12):
                                        print('Item: {}- {}강 {}'.format(log['data']['itemRarity'], log['data']['after'], log['data']['itemName']))
                    normal += 1
                except:
                    error_count += 1
                    error_char.append({'serverId':char['serverId'], 'characterName':char['characterName']})
                    if error_count == 1:
                        print('{}/{} API server down or Freak character: {}(in logSearch step)'.format(num + 1, length, char['serverId']+" "+char['characterName']))
        # 이하 생략
        return temp, error_char
```  
NEOPLE Open API 서버에 GET 방식으로 request를 보낸다.  
응답받은 로그 중 13 강화 이상 성공 기록만을 추출하도록 한다.  
다만, 불사조 무기나 커먼, 언커먼, 레어 아이템은  
개인적으로 불필요한 데이터라고 생각하기 때문에 제외시키도록 한다.  
또한 응답받는 과정에서 오류가 있는 캐릭터는 예외 처리를 하여  
따로 기록을 저장하도록 한다. 

# 끌어온 로그 만져보기
```python
    def selectValue(self, rein_log, val):
        win = []
        lose = []
        trash = []
        for log in rein_log:
            if log['result'] == 'True' and int(log['before']) == val:
                win.append(log)
            elif log['result'] == 'False' and int(log['before']) == val:
                lose.append(log)
            else:
                trash.append(log)
        return win, lose

    def arrangeBytime(self, win, lose):
        temp = []
        win_time = {}
        lose_time = {}
        for time in win:
            if time['date'][:2] not in win_time:
                win_time.setdefault(time['date'][:2], [])
            win_time[time['date'][:2]].append(int(time['date'][:2]+time['date'][-2:]))
        [win_time[hour].sort() for hour in win_time]
        win_time = sorted(win_time.items())

        for time in lose:
            if time['date'][:2] not in lose_time:
                lose_time.setdefault(time['date'][:2], [])
            lose_time[time['date'][:2]].append(int(time['date'][:2]+time['date'][-2:]))
        [lose_time[hour].sort() for hour in lose_time]
        lose_time = sorted(lose_time.items())
        return win_time, lose_time
```  
selectValue 메소드에서는 성공과 실패 로그 및 횟수를 나눠서 저장한다.  
arrangeBytime 메소드에서는 selectValue에서 초기화한 성공과 실패 리스트를 사용하여  
성공한 시간대, 실패한 시간대를 나눠 기록하는 작업을 수행한다.  
또한 시간대를 오름차순으로 정렬한다.  
시간대별 13강화 이상 성공 비율을 확인하는 작업이며  
전통적인 통계적인 방법을 사용하지 않아 의미없을 수 있는 기록이지만  
**~~13강 도전을 33번 실패한 친구에게는 유용할 수도 있을 것 같다...~~**  

# 결과 및 후기
<img src="/images/fulls/whole_log.JPG" style="width:542px; height:403px;">

위 사진은 출력 중 일부를 캡쳐한 것이다.  
성공 비율이 높은 새벽 특정 시간대나 15~18시의 경우,  
친구가 직장인이기 때문에 강화 시도에 어려움이 있으며,  
강화를 가능한 시간대 중 성공 비율이 높은 시간대에서는  
이미 많은 실패를 겪었기 때문에 성공 비율이 꽤나 높으며  
시도를 많이 안해본 시간대에 속하는 20시를 추천해줬다.  
(물론 전혀 근거없는 뇌피셜 및 미신이었지만.)  
<br/>
<img src="/images/fulls/miracle.jpg" style="width:525px; height:707px;">

그렇게 친구의 소원은 이루어졌다고 한다...  
앞으로도 같이 열심히 합시다!  