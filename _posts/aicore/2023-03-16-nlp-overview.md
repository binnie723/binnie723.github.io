---
title:  "자연어처리(NLP) 개념 소개" 

categories:
  - AI Core Projects
tags:
  - [ChatGPT]

toc: true
toc_sticky: true

date: 2023-03-16
last_modified_at: 2023-03-16
---


<br/> 
## 자연어 처리(NLP) 의미

Natural Language Processing의 줄임말로, 컴퓨터의 언어로 자연어를 처리하는 기술

여기서 자연어란 사람들이 일상적으로 쓰는 언어를 의미하는데, 인공적으로 만들어진 언어와 구분하기 위해 만들어진 개념이다. 

자연어 처리는 다음과 같이 크게 두 가지 분야로 구분 :

- **자연어 이해** (NLU) : 글 속에서 관계와 유사도를 측정하고 분석하는 기술
- **자연어 생성** (NLG) : 자동 문장 완성이나 스토리 제작과 같이 글을 생성하는 기술

![image](https://user-images.githubusercontent.com/86834982/225531368-937f548f-c05a-405c-93ca-91b904ef92a8.png){: width="550px"}  

NLP의 활용으로는 주로 추천 시스템을 다루는 경우가 많다. 하지만 아직 국내에서는 사용자들의 대부분이 만족하는 완벽한 추천 시스템이 등장하지는 않았다. 

그래도 네이버와 카카오 등의 기업들이 구글의 원천 기술을 활용한 다양한 서비스를 제작 중에 있으며, 대표적인 예시로는 암 재발 조기 진단 AI 시스템, 우울등 예후 예측 시스템, 콜센터 자동 분류 시스템, 그리고 감정 분석 챗봇 시스템(ex. 이루다) 등이 있다. 



<br/>   
## 자연어 처리 모델 구축 과정

- 모델 : 입력을 넣어서 어떤 일련의 처리 과정을 거쳐 출력으로 반환
    
    ![image](https://user-images.githubusercontent.com/86834982/225531433-f26d9e65-9509-4d82-b99b-0e6cd553470e.png){: width="550px"}  
    

- 활용 예시 : 영화 리뷰 감성(sentiment)기반 자연어 처리 모델
    
    ![image](https://user-images.githubusercontent.com/86834982/225531438-5d107a35-2508-4815-8bb8-d438a8e5b5c7.png){: width="550px"}  
    


<br/> 
### 데이터 수집

학습에 필요한 데이터를 모으는 과정, 주로 다음 중 하나의 방식으로 데이터를 수집한다.

- 파이썬을 이용한 웹 크롤링 
    - request 라이브러리로 웹 정보를 받고 BeautifulSoup로 파싱
    - Selenuim 라이브러리로 웹에 들어가서 직접 긁어오기 
- 이미 제공되어 있는 데이터를 찾아서 불러오는 방법


<br/> 
### 데이터 전처리

- 문장/레이블 처리 : 어디에 속하는지 지정해주는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/225531858-2215d70f-c19f-4540-ad4f-30b4f6f0dc54.png){: width="550px"}  
    
- 데이터 분할 : 학습(train) 데이터와 검증(test) 데이터를 나누는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/225531863-5666ae2a-316a-47c4-a990-f4dc02b71e8b.png){: width="550px"}  
    
    -> 비율이 어떤 것이 적당한가? : 보통 8:2나 7:3으로 나누어서 분할 
    
<br/> 
### 데이터 Normalization, Cleaning

- **정규화 (normalization)** : 대소문자 통합, 규칙 기반 단어 수정
    
    ex. 전달하다, 전해주다, 건네다 -> 전달하다로 통일 
    

- **정제 (cleaning)** : 텍스트 데이터로부터 노이즈를 제거하는 과정
    
    여기서 노이즈는 불용어(주로 조사), 중복 단어 등을 의미한다. 
    
    ex. NLP를 학습**을** 하고 있습니다. -> NLP를 학습 하고 있습니다.
    
    ![image](https://user-images.githubusercontent.com/86834982/225532318-df309b0f-fa9c-491c-bbb0-84cdae786b3e.png){: width="550px"}  
    

<br/> 
### 데이터 Tokenization

텍스트를 토큰 단위(작은 단위)로 분할하는 작업 

- **단어 토큰화**
    - word tokenize
        
        *“Do”, “n’t”, “forget”, “my”, “name”*
        
    - WordPunctTokenizer
        
        *“Don”, “’”, “t”, “forget”, “my”, “name”*
        
    - text to word sequence
        
        *“don’t”, “forget”, “my”, “name”*
        
    
    -> 일부 방법은 불필요해 보이지만, 나름의 어떤 특정한 상황에서 필요에 의해 구현된 방식들임 !
    
- **한국어 단어 토큰화**
    - Open korea text (Okt)
        
        “자연어”, “처리”, “연구”, “를”, “기반”, “으로”
        
    - word tokenize
        
        “자연어”, “처리”, “연구”, “를”, “기반”, “으로”
        
    - split sentence
        
        “자연어 처리 연구를 기반으로 하고 있습니다.”
        
    
    -> 굳이 무거운 모델을 쓸 필요는 없다. 다른 가벼운 모델 가능하면 쓰는 것 좋음 !
    
    ![image](https://user-images.githubusercontent.com/86834982/225532504-ac89b929-fcd5-440f-8d59-878400836f7c.png){: width="550px"}  
    

<br/> 
### 데이터 Encoding

컴퓨터 정보의 형태나 형식을 다른 형식으로 변환하는 것으로, 자연어 처리에서는 주로 텍스트를 숫자로 변환하는 과정을 말한다. 

![image](https://user-images.githubusercontent.com/86834982/225532507-7ea57107-0a37-4bdd-a4d7-45f0529268d8.png)

![image](https://user-images.githubusercontent.com/86834982/225531716-88747637-49da-4e62-b8be-d860eb8faedd.png){: width="550px"}  

- **One-hot encoding** : 표현하고 싶은 인덱스에 1을 부여하고 나머지에 0을 부여하는 방식의 인코딩으로, 하나의 문자만 발현시키고 싶을 때 사용함.
    - "red" can be represented as [1, 0, 0]
    - "green" can be represented as [0, 1, 0]
    - "blue" can be represented as [0, 0, 1]


<br/> 
### Padding

문장의 길이를 임의로 동일하게 수정하는 과정

문장의 길이는 모두 다르다. 인공지능은 생각보다 똑똑하지 않기 때문에 에러를 발생시킨다. 따라서 문장의 길이를 맞춰주는 작업이 필요하다. 

![image](https://user-images.githubusercontent.com/86834982/225532079-166586b2-7220-4bb4-8e96-a16ea9a30435.png){: width="550px"}  

-> 정보를 잃지 않으면서 중요한 정보를 포함하는 방식으로 수정하는 것이 중요

-> 앞에서부터 할지 뒤에서부터 할지는 개인과 상황에 따라 결정 


<br/> 
### 데이터 Embedding

단어를 벡터로 표현하는 것

 0이라는 무의미한 숫자를 계속 넣을 이유가 있을까?에서 시작된 개념이다. 임베딩이 중요한 이유는 계속 차원을 늘리면서 실수 형태로 표현할 수 있도록 하기 때문

![image](https://user-images.githubusercontent.com/86834982/225532316-2a5a62f1-fb65-412d-ae1c-1999f1cfb945.png){: width="550px"}  


<br/> 
### 학습 모델 구성

1. **딥러닝 모델** 선정
    
    수행 과제 기반 적절한 모델을 선택해서 사용하기 
    
    State of the art (Sota), CNN, RNN, LSTM, GRU, Yolo, Bert, VGG, ResNet 등
    
2. **하이퍼 파라미터** 설정 
    
    = 직접 튜닝을 하는 파라미터 (epoch, 학습률, 미니배치 크기, hidden layer 수 등)
    
    ![image](https://user-images.githubusercontent.com/86834982/225532646-89d242f7-e945-4f6a-b566-28dbce95eb88.png){: width="550px"}  
    

<br/> 
### 추론 모델 구축

추론 모델은 학습한 가중치를 가진 모델을 말한다. 

학습 모델은 주어진 입력 데이터를 출력 데이터와 매핑해서 학습하는 반면, 추론 모델은 학습이 끝난 이후 새로운 데이터에 대해 예측과 추론을 한다는 점에서 차이가 있다. 

![image](https://user-images.githubusercontent.com/86834982/225532654-dcb56ae2-711c-4637-8b08-3690e8337208.png){: width="550px"}  

-> 보통 외부 서버나 클라우드 플랫폼에 호스팅된 모델의 경우에는, API가 만들어져야 개인 컴퓨터와 연동해서 사용할 수 있다. 


<br/> 
### 관련 유용한 사이트 및 자료들

- AI hub : 양질이라 보기 힘들지만 무료 데이터셋을 받아오기 좋은 사이트
    
    [https://aihub.or.kr](https://aihub.or.kr/)
    
- Huggingface example : 잘 정리된 좋은 코드들을 모아둔 사이트
- 그 외의 추천하는 책들 리스트

![image](https://user-images.githubusercontent.com/86834982/225531577-3673a72e-0c0a-4603-ae89-73804c31ddfb.png){: width="550px"}  


<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
