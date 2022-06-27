---
layout: post
---
<img src="/images/fulls/pls_latent_3d.JPG" style="width:413px; height:282px;">


# **대회 개요**
---
> [Credit Card Fraud Detection - Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
>
>
> 카드사는 사기성 신용 카드 거래를 인식하여  
> 고객이 구매하지 않은 항목에 대해 요금이 청구되지 않도록
> 하는 것이 중요하다.  
>  
> ## **데이터 개요**
---
<img src="/images/fulls/data_shape.png" style="width:933px; height:100px;">

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