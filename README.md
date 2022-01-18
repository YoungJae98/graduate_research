# 졸업연구진로 결과정리

    주제 : 유사어 탐색 모델 만들기

    구현 언어 : python

    구현 환경 : ubuntu, mac os

    주어진 데이터 : data1.csv (설문내용), data2.csv (각 응답자들의 성향)

    유사어 탐색에는 성향부분은 사용하지 않고 설문내용만 참고함

## 시작 (환경 설정)
    1. konlpy 설치 : https://konlpy.org/en/latest/install
  
      시키는대로 다운을 받게되면 tweepy 관련 오류가 발생할 수 있음

      4버전 이상으로 올라가있는 tweepy를 내려주는 과정이 필요함

      pip install tweepy==3.10.0 로 버전을 내려주면 실행됨.

      (이외 에러는 대부분 jpype문제일 수 있음 jpype 또한 1.3버전 이상에서는 작동이 안됨)
      
 
  
    2. kobert 설치 : https://github.com/SKTBrain/KoBERT/tree/master/kobert_hf

      원래 허깅페이스가 아니라 기본 경로를 이용해서 다운이 가능했으나, 저장소 이슈로 안되는 상황이기에 해당 경로에서 다운받기

      하지만 윈도우에서는 이유모를 에러로 인해 구현이 되지 않아서 wsl2를 이용해서 우분투 환경에서 구현에 성공함

      맥OS를 쓰는 경우에는 별다른 에러가 발생하지 않음.
  
  
  
    3. 기타 사용되는 라이브러리들 설치
      
      pandas, numpy, gensim, scipy 등등은 pip install을 이용해서 설치 가능
      
      
      
    4. 코드 설명
      
      주석에 간략한 설명 달아두었습니다. 그 외에 궁금한점은 yj122198@naver.com 으로 메일 부탁드립니다.
       
       
       
    5. conda 환경설정(선택)
    
      설정할 내용이 이것저것 많아서 conda 가상환경 세팅을 추천합니다.
      
      https://data-panic.tistory.com/4 해당 사이트 하단을 참고하거나
      
      conda 가상환경 만들기를 구글에 찾아보면 손쉽게 방법을 찾을 수 있습니다.
      
      
      
## 진행 방식

    유사어를 어떻게 하면 잘 찾을 수 있을까를 고민해봄.
    
    총 2가지 기존에 존재하는 라이브러리를 이용해서 해보기, 사전학습 모델을 이용해서 임베딩을 진행해보기를 함.
    
    기존에 존재하는 라이브러리를 이용한 것은 gensim의 fasttext를 이용해서 주어진 데이터로 학습하고 결과를 내봄
    
    사전학습 모델을 이용한 것은 skt의 ko_bert를 이용함
    
    ko_bert 이용시 문장을 토큰화할때, 기본적으로 센텐스피스 기반의 토크나이저를 지원하지만,
    
    해당 토크나이저를 이용할시에 각 단어들에 대한 벡터값을 뽑아낼 수 없다는 단점이 있기에 버트에서 제공하는 토크나이저가 아닌
    
    konlpy에서 지원하는 토크나이저를 이용해서 구현함.
    
    
## 결론
    
    결과를 내기는 했지만, 아래 2가지를 해결하면 더 좋은 결과물을 낼 수 있을 것 같음.
    
      1. 버트에 맞는 토크나이저를 사용하지 못했기에 기대하는 성능이 나오지 않을 수 있다.
        -> 단어별로 임베딩 결과를 내고 유사어를 찾다보니, 버트에서 지원하는 센텐스피스 기반의 토큰화로는 특정 단어에 대한 결과를 제대로 뽑기 어려웠다.
           이를 해결하기 위해서 문장을 konlpy의 okt에서 지원하는 토큰화 함수를 이용하여 쪼갠 것을 학습에 이용했다.
      
      2. 동일한 단어에 대해서 단순히 평균을 내어 임베딩 결과값을 냈다.
        -> 다른 문장에서 등장하는 같은 의미를 지닌 단어에 대해서 결과가 나오게 될텐데, 이때 중복되는 부분을 처리해주기 위해서 모두 더한 뒤 횟수로 나누어주는 방식을 사용했다.
           이처럼 구현하게 될 경우 동음이의어와 같은 단어나 여러가지 상황을 반영하지 못할 수 있기에 좋은 방법은 아니라고 생각된다.
        
      3. 데이터의 양이 충분치 못했을 수 있다.
        -> 주어진 데이터의 양이 비교적 적기때문에 유의미한 결과를 내지 못했을 수 있다.
        
      
       
