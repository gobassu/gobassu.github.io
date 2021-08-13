Text Mining 의 이해
==================
* 머신러닝 기법
  + LDA(Linear Discriminant Analysis) - 토픽 모델링
  + SVM(Support Vector Machine) - 문서 분류
* 딥러닝 기법
  + 순환 신경망 RNN (Recurrent Neural Network)
    ```
    RNN은 은닉층의 노드에서 활성화 함수를 통해 나온 결과값을 출력층 방향으로도 보내면서, 
    다시 은닉층 노드의 다음 계산의 입력으로 보내는 특징을 갖고 있습니다.
    ```
  + 장단기 메모리(Long Short-Term Memory, LSTM)
    ```
    RNN의 시점(time step)이 길어질 수록 앞의 정보가 뒤로 충분히 전달되지 못하는 단점을 보완하기 위해 
    은닉층의 메모리 셀에 입력 게이트, 망각 게이트, 출력 게이트를 추가하여 
    불필요한 기억을 지우고, 기억해야할 것들을 정하는 특징, 셀 상태(cell state)라는 값에서 장기상태와 단기상태로 나뉨
    ```
  + Transformer
    ```
    트랜스포머는 RNN을 사용하지 않지만 기존의 seq2seq처럼 인코더에서 입력 시퀀스를 입력받고, 
    디코더에서 출력 시퀀스를 출력하는 인코더-디코더 구조를 유지하고 있습니다. 
    다만 다른 점은 인코더와 디코더라는 단위가 N개가 존재할 수 있다는 점입니다.
    논문에서는 인코더와 디코더의 개수를 각 6개로 설정
    ```
  + BERT (Bidirectional Encoder Representations from Transformers)
    ```
    BERT의 기본 구조는 트랜스포머의 인코더를 쌓아올린 구조입니다. 
    Base 버전에서는 총 12개를 쌓았으며, Large 버전에서는 총 24개를 쌓았습니다. 
    레이블이 없는 방대한 데이터로 사전 훈련된 모델을 가지고, 
    레이블이 있는 다른 작업(Task)에서 추가 훈련과 함께 하이퍼파라미터를 재조정하여 
    이 모델을 사용하면 성능이 높게 나오는 기존의 사례들을 참고하였기 때문입니다. 
    다른 작업에 대해서 파라미터 재조정을 위한 추가 훈련 과정을 파인 튜닝(Fine-tuning)이라고 합니다.
    ```


### 1. 수집 
```
크게는 Download 방식과 Crawling 방식이 있음 
```

### 2. 전처리 

#### Step1: Tokenization
```
 Text를 정해진 단위(word | morph | Character | 초.중.종성)로 나누기
```

#### Step2: Stopword Removal 
```
 불용어 지우기 ex) a | an | the ... 
```
#### Step3: Capitalization
```
 대소문자 통일 (주로 소문자로 변환) 
```
 
#### Step4: Stemming and Lemmatization 
 
* Stemming : 알고리즘을 통해 기계적으로 구분  
* Lemmatization : 통일(어근)된 표현을 존재한는 표현으로 변환 

```
 어근 추출  ex) Played | playing | plays ==> Play  
```
 
### 3. 가공
* Text encoding : 텍스트를 Vector로 표현
```
Bag-of-word(말뭉치) : 문서내 발생단어로 구성된 전체 Vector 중 단어(명사) 포함 여부를 1 또는 0 으로 표현 
TF-IDF(term Frequency-Inverse Documentation Frequency) : 
     tf(t, d) --> 문서 안에 있는 가 단어 t의 빈도 
     idf(t, D) --> 단어 t가 전체 문서 D에서 등장한 문서 수의 역수 
```
* Word Embedding 
  + NPLM (Neural Probabilistic Language Model) 
    - 처음 제안된 밀집표현(Dense Representation)으로, Neural Network 이용 주변 단어 등장 확률 예측
  + word2vec
    - Skip-Gram with Negative Sampling 
    - NPLM 높은 계산량 문제 해결 (Softmax 이용) 
  + fastText
    - Subword SGNS 
    - word2vec의 OOV(Out of Vocabulary) 문제 해결, 학습 단위가 subword 
  + ELMO (Embedding from Language Model) 
    - Pre-trained Language Model 제안, Bi-directional 언어모델로 문맥을 반영한 워드 임베딩 기법 제시 
    - NLP에서 transfer Leaning 확산 

### 4. 분석
* 토픽 모델링 : 결과적으로 ㅁ든 단어들이 각 토픽에 속할 확률 계산 
* 감정 분석(Opinion Mining) : Text에 포함된 저자의 의견(감정)을 찾아내는 기법 
* 문서 분류 : 지정한 Category로 분류하는 방법으로 지도학습, 딥러닝 많이 사용 

### 5. 시각화 
* Word Cloud
* 의미 연결망 분석(Semantic Network Analysis) 





