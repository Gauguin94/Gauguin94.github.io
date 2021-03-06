---
layout: post
---
<img src="/images/fulls/pls_latent_3d.JPG" style="width:413px; height:282px;">


# **대회 개요**
---
> [Credit Card Fraud Detection - Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)  
>  
> 카드사는 사기성 신용 카드 거래를 인식하여  
> 고객이 구매하지 않은 항목에 대해 요금이 청구되지 않도록
> 하는 것이 중요하다.  
>  
> ## **데이터 개요**
> ---
>> <img src="/images/fulls/data_shape.png" style="width:933px; height:100px;">  
>> 284,807 건의 거래 중 사기(이상치) 거래는 단 "492"건만이 존재한다.  
>> 이를 퍼센티지로 따지면 0.172%이다.  
>> 데이터들의 Feature는 Time, V1 ~ V28, Amount, Class이다.  
>> 거래 데이터의 특성 상 보안 문제를 야기할 수 있기 때문에,  
>> 거래 데이터를 온전하게 표현할 수 없다.  
>> 그리하여 거래 관련 데이터에 PCA(주성분 분석)를 적용하여   
>> V1 ~ V28의 Feature로 나타내었다.  
>> Time, Amount, Class는 당연히 PCA가 적용되지 않았다.  
>> Class는 0, 1로 이루어져 있으며, 0은 정상, 1은 사기 거래를 나타낸다.   
>> 클래스 간 불균형이 매우 심하기 때문에,  
>> 개최자는(데이터 제공자는) Precision-Recall Curve를  
>> 사용하여 평가하는 것을 권장하였다.  
>> 위 사진은 상위 샘플 5개를 나타낸 것이다.
>
> ## **데이터 간단히 살펴보기**
> ---
>> <img src="/images/fulls/data_corr.png" style="width:85px; height:369px;">  
>> 필자가 생각하기에는, 거래와 같은 이상 탐지에서는  
>> "시간"이라는 특성이 매우 큰 영향을 끼친다고 생각한다.  
>> 예를 들어, 어제까지 10년간 하루에 100원만 쓰던 사람이 갑자기  
>> 오늘 10,000원을 쓴다면 이상 거래를 의심해볼만 하다.  
>> <span style='background-color: #fff5b1'>왜?</span>  
>> 지금껏 하루에 100원만 썼는데("지금껏"이라는 시간의 개념.)  
>> 하루 아침에 달라졌기 때문이다.  
>> 하지만 이번에 다룰 데이터에서는 거래에 관련된 정보들에 이미 PCA를 적용하여  
>> V<sub>n</sub>에 해당하는 Feature로 함축시켰다.  
>> 그렇다면, 이미 시간 개념이 포함되어 있는 거래 관련 정보들이 모두  
>> PCA로 차원 축소되어 있다고 생각하여 이번 이상치 탐지에 크게 연관이 없을 것이라고  
>> 생각하였고, 때마침 상관성도 옅게 도출되었기 때문에  
>> Time은 과감하게 제외하고 분석을 진행하였다.  
>> <span style='background-color: #fff5b1'>(위의 사진이 Class와 다른 Feature 간 상관도를 나타낸다.)</span>  
  
# **도전**
> ---
> <img src="/images/fulls/ae_pic.png" style="width:959px; height:449px;">  
> 이상치를 탐지하는 전형적인 방법들은  
> z-score를 통한 판단, K-nearest neighbor, k-means, isolation forest 등  
> 지도학습과 비지도학습을 아울러 여러 가지가 존재한다.  
> 그 중에서 가장 매력적인 방법이라고 생각되는 Auto Encoder(오토 인코더)를  
> 사용하여 데이터 분석을 진행하였다.  
> 오토인코더의 큰 틀에서의 개념은 매우 쉽다.  
> 그저 신경망의 입력으로 'A'라는 데이터를 집어 넣고  
> 신경망의 출력으로 'A'라는 데이터가 똑같이 나오도록 학습시키는 것이다.  
> (대개, fully connected layer를 연달아 사용하여 신경망을 구성한다.)  
> <span style='background-color: #fff5b1'>똑같이 나오도록 학습시키는게 이상 탐지랑 뭔 상관?</span>  
> 앞서 z-score, knn, k-means, isolation forest등 이상 탐지를 진행하는  
> 전형적인 방법들을 나열하였다.  
> 이들의 공통점이 무엇인가 하면,  
> 정상 데이터들과 이상 데이터들 간  
> 어떠한 차이가 있다는 것을 전제로 한다는 것이다.  
> 그리하여 정상 데이터들을 서로 군집시키거나, 이상 데이터를 고립시키는  
> 방법 등을 사용하여 정상 데이터와 이상 데이터를 분류할 수 있도록 한다.  
> <span style='background-color: #fff5b1'>그렇다면,</span>  
> 오토인코더는 말 그대로 데이터 복원에 초점을 맞춰 학습을 진행하므로  
> 오토인코더로 하여금 정상 데이터만을 학습시켜 복원하도록 한다면,  
> 새로운 정상 데이터를 집어 넣게 되었을 경우에는 성공적으로 복원을 할 것이며,  
> 이상 데이터를 집어 넣게 되었을 경우에는 복원을 제대로  
> 진행하지 못할 것이라는 기대를 할 수 있다.  
> 
> ## **대략적인 흐름**  
> ---
>> <img src="/images/fulls/lstm_ae.JPG" style="width:584px; height:387px;">  
  
```python
        self.conv_encoder1 = nn.Conv1d(1, 3, kernel_size=3).to(DEVICE)
        self.conv_encoder2 = nn.Conv1d(3, 8, kernel_size=3).to(DEVICE) 
        # conv_encoder2 이후 np.transpose 사용하여 피쳐맵을 원하는 shape으로 맞춰줌.

        self.lstm_encoder = nn.LSTM(
            input_size=8, hidden_size=1,
            num_layers=2,
            batch_first=True
        ).to(DEVICE)
        # lstm_encoder에서 latent vector 반환
        
        self.lstm_decoder = nn.LSTM(
            input_size=1, hidden_size=8,
            num_layers=2, 
            dropout=0.5,
            batch_first=True
        ).to(DEVICE)

        # lstm_decoder 이후 np.transpose 사용하여 피쳐맵을 원하는 shape으로 맞춰줌. 
        self.conv_decoder1 = nn.ConvTranspose1d(8, 3, kernel_size=3).to(DEVICE)
        self.conv_decoder2 = nn.ConvTranspose1d(3, 1, kernel_size=3).to(DEVICE)
```  
  
>> 일반 오토인코더가 아닌 CNN에서의 Convolution과 LSTM을 결합한 오토인코더를  
>> 설계하여 데이터 분석을 진행한다.(Convolutional Recurrent Neural Network, CRN)  
>> 입력이 되는 데이터는 Convolution layer를 지나 첫 번째 LSTM을 거치게 된다.  
>> <span style='background-color: #fff5b1'>(인코더 부분)</span>  
>> 첫 번째 LSTM을 통해 만들어진 **Latent Vector**가 두 번째 LSTM과  
>> Convoluional Transpose Layer를 지나 원래의 데이터에 가깝게 복원이 된다.  
>> 인코딩 부분과는 다르게 디코딩 과정에만 dropout과  
>> Batch Normalization과 같은 규제를 추가하였다.  
>> 이와 같이 설계한 이유는 **조금 자세하게? TMI** 부분에서 후술하겠다.  
>> <span style='background-color: #fff5b1'>(디코더 부분)</span>  
>> 구현된 오토인코더는 '데이터 전처리'의 역할을 맡는다.  
>> 구현된 오토인코더를 사용하여 전처리된 데이터를  
>> isolation forest와 random forest에 집어 넣어  
>> 분류 성능을 확인한다.  
>> ### **Latent Vector**  
>>> Latent Vector는 오토인코더의 인코더 부분에서 출력되는 feature map이다.  
>>> 인코더를 통해 원래 데이터를 Convolution 연산과 LSTM으로 함축시키게 된다.  
>>>  
>>> **함축?**  
>>> <img src="/images/fulls/conv.JPG" style="width:381px; height:230px;">  
>>> 보통, 합성곱 연산이라고 하면, Convolution에 의한 연산,  
>>> 즉, filtering과 stride, pooling에 의해  
>>> feature map의 크기가 점차 줄어 들게 된다.  
>>> 그리고 앞서 말했듯이, 우리는 ConvTrans를 통해 역컨볼루션 연산과 LSTM을 
>>> 취하여 원래 데이터를 복원할 것이기 때문에  
>>> 중간에 놓이게 되는, Latent Vector는 원래 데이터의 정보를 
>>> 함축시킨 효과를 불러 일으킨다.  
>>> 또한, 매우 러프하게 생각한다면,  
>>> 차원 축소에 사용되는 PCA는 Linear한 데이터의  
>>> 차원 축소에 효율적으로 사용될 수 있지만,  
>>> 비선형 데이터에는 100% 원하는대로 동작한다고 볼 수 없다.  
>>> 반면, 이 신경망을 이용한 인코더는 비선형 데이터의 차원 축소에도  
>>> 훌륭한 역할을 수행할 수 있다고 생각한다.  
>>> 이러한 차원 축소를 거쳐 분류, 혹은 학습에 사용되는 연산량을  
>>> 크게 줄일 수 있는 장점 또한 갖게 된다.  
>> <span style='background-color: #fff5b1'>그리하여</span>  
>> 정상 데이터만을 사용하여 ConvLstm AutoEncoder를 학습시키고,(CRN)  
>> 학습이 끝난 해당 오토인코더에 평가 데이터를 집어 넣어 추출되는  
>> Latent Vector를 활용하여 이상 탐지를 진행하려고 한다.  
>
> ## **조금 자세하게? TMI**
> ---
>> 1. **LSTM과 Convolution을 같이 사용한 이유.**  
>> <img src="/images/fulls/encoding_part.JPG" style="width:653px; height:411px;">  
>> 위 사진과 같이, 인코더 부분에서 LSTM으로 데이터가 입력되기 전에,  
>> Convolution을 사용하여 데이터의 차원을 줄이고(사진에서는 8에서 5.)  
>> conv를 통과한 텐서 내 원소 각각 n 개의 수치로 표현한다.  
>> (사진을 예시로, x<sub>1</sub>은 [11, 14, 14]로 표현됨, n=3)  
>> 매우 직관적으로 설명해보자면,  
>> X 내 '2'가 3개 존재한다.  
>> X가 이상 샘플이면서 숫자 '2'에 의해  
>> 이상 샘플로 분류되었다고 가정해보자.  
>> 모든 '2'에 의해서 이상 샘플로 분류된 것이 아니고  
>> 2번째(5행) '2'에 의해 이상 샘플로 분류되었다고 했을 떄,  
>> 같은 '2'라도 서로 다르게 표현해야 할 것이다.  
>> 그리하여 위와 같은 작업을 수행하였으며,  
>> 설계한 모델에서는 n=8로 진행하였다.(conv에서의 channel)  
>>   
>> 2. **디코딩 부분에서만 규제를 사용한 이유.**  
>> <img src="/images/fulls/dont_dropout.JPG" style="width:355px; height:308px;">  
>> 인코딩 부분에서 dropout 및 batch normalization을  
>> 사용한다면, 온전한 Latent Vector가 추출되지 않을 수 있다.  
>> (dropout과정에서 node들을 확률적으로 사용하기 때문에  
>> 인코딩 부분에서 dropout을 수행하게 되면 특정 node들이  
>> 활성화 되지 않은 vector가 추출될 수 있다.  
>> ex) [1, 2, 3, 2, 1] -> dropout -> [1, 0, 3, 2, 0])  
>> batch normalization 또한 regularization의  
>> 효과가 있기 때문에 인코딩 부분에서 사용하지 않았다.  
>> 디코딩 부분은 Latent Vector를 기반으로  
>> 원래의 데이터를 복원하는 역할을 하기 때문에  
>> 과대적합(Overfitting)이 일어나지 않게 만들기 위하여  
>> dropout과 batch normalization을 사용하였다.  
>> <span style='background-color: #fff5b1'>Latent Vector의 추출은 쉽게! 복원은 어렵게!</span>  
  
# **결과**  
> ---
> ```python
> from sklearn.model_selection import train_test_split as tts
> x_train, x_test, y_train, y_test = tts(
>       data, label, 
>       test_size=0.3, 
>       random_state=42,
>       stratify=label
> )
> ```  
> 284,807개의 샘플을 이상치 비율에 맞춰 train, test로 분할하였다.  
> train set 내에서 다시 한 번 train, valid로 분할하였다.  
>   
> ## **성능 비교**
> ---
>> <img src="/images/fulls/isofo.JPG" style="width:589px; height:174px;">  
>> 왼쪽은 제공된 데이터를 isolation forest에 그대로 집어 넣어  
>> 추론한 결과, 오른쪽은 latent vector를  
>> isolation forest에 집어 넣어 추론한 결과이다.   
>> 오토인코더를 통한 전처리를 수행한 경우가  
>> 조금이나마 더 나은 성능을 지님을 확인할 수 있다.  
>>   
>> <img src="/images/fulls/ranfo.JPG" style="width:588px; height:183px;">  
>> 왼쪽은 제공된 데이터를 random forest에 그대로 집어 넣어  
>> 추론한 결과, 오른쪽은 latent vector를  
>> random forest에 집어 넣어 추론한 결과이다.  
>> 오토인코더를 통한 전처리를 수행한 경우가  
>> 더 나은 성능을 지님을 확인할 수 있다.  
>  
> ## **어떻게 됐길래 성능이 좋아졌을까?(데이터 시각화)**
> ---
>> <img src="/images/fulls/tsne_vis.JPG" style="width:528px; height:277px;">  
>> t-SNE를 사용하여 train set을 시각화하였다.  
>> 노란색은 정상 데이터, 붉은색은 이상치를 나타낸다.  
>> 왼쪽은 제공된 데이터 원본, 오른쪽은  
>> latent vector를 나타낸 것이다.  
>> 한 눈에 보기에도 왼쪽 데이터에서는 패턴을 찾지 못하여  
>> 정상 데이터와 이상 데이터가 골고루 섞여있어 분류하기 어려워 보인다.  
>> 반면, 오른쪽 데이터에서는 이상 데이터들이 어느 정도 밀집된 것을  
>> 확인할 수 있다.  
>> <span style='background-color: #fff5b1'>모델이 어떠한 패턴을 찾은 것이다!</span>  
>>  
>> <img src="/images/fulls/pls_vis.JPG" style="width:666px; height:259px;">  
>> PLS regression을 사용하여 train set을 시각화하였다.  
>> 왼쪽은 제공된 데이터 원본, 오른쪽은  
>> latent vector를 나타낸 것이다.  
>> 왼쪽의 경우 위와 마찬가지로 분류하기 어려워 보인다.  
>> 반면, 오른쪽은 위와 다르게 이상 데이터가 밀집되어 있지 않지만,  
>> 정상 데이터와 다르게 퍼져있음을 확인할 수 있다.  
  
# 끝으로
---
<br/>

> 당연하겠지만, 같은 데이터에 대하여 지도학습과 비지도학습은 성능 차이가 존재한다.  
> (isolation forest vs Random forest)  
> 사견으로, 인간의 입장에서 분류나 예측이 쉬운 데이터는  
> 분류기나 예측기의 입장에서도 학습이 쉬울 것이라고 생각한다.  
> 그렇기 때문에 데이터 분석에서 또 한가지 중요한 점은  
> 합리적인 방법을 통해 최대한 분류나 예측하기 쉽도록  
> 데이터를 가공하는 것이라고 생각한다.  