# NEURAL MACHINE TRANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE

> Paper URL: https://arxiv.org/pdf/1409.0473.pdf
>
> 지식이 깊지 않아 오역이나 오타가 있을 수도 있습니다.
>
> ※ 잘못된 정보, 수정, 지적 환영합니다.


## Abstracat

최근 많은 모델은 번역의 성능을 올리기 위해, 고정된 길이의 벡터로 encode하고, decoder로 하여금 번역을 수행합니다.

하지만 이러한 과정이 모델의 병목현상을 유발하며, 성능을 저하시키는 원인이 된다고 논문에서는 추측한다.

이러한 문제를 해결하기 위해 논문에서는 아래와 같이 새로운 모델을 제안한다.

**모델이 스스로 Target Word를 예측하는데 관련이 높은 Source 문장의 위치를 명시적으로 정의하지 않고, (soft-)search 할 수 없을까?**

또한 질적 분석 결과, 영어-프랑스어 번역 작업에서 모델에서 발견되는 (soft-)alignment가 일반적인

직관과 잘 일치하는 것으로 확인되었다.

> **`(soft-serach)`** : 사람이 지정해주지 않고, 스스로 탐색하는 것을 말한다.
>
> **`(soft-alignment)`**: alignment란 조정이라는 의미를 가지고 있으며,  언어마다 순서구조가 다르기 때문에 mapping시켜준다는 개념으로 이해하였다. 따라서 soft-alginment란 target에 대해 source 정보에서 스스로 alignment하여, target에 대해 의미있는 정보를 mapping 한다.

## 1. Introduction

논문에서 애기한 기존 모델의 문제는 아래와 같다.

encoder는 input 길이에 관계없이, 제한된 길이(fixed-length)의 vector로 압축해야 한다.

그렇기 때문에 훈련 Corpus보다 긴 문장이 들어올 경우, 대처하기가 어렵다.

또한 문장의 길이가 길수록, encoder의 성능이 떨어진다는 것은 논문에서 증명되었다.

제안된 모델은 제한된 길이로 encode하지 않고, 연속적인 벡터로 구성된 입력 문장에서

decoding 하는 동안 벡터의 일부분을 선택한다.

이렇게 하면 번역 성능이 향상되는 것은 물론, 문장의 길이에 구애받지 않는다.

## 2. Background: Neural Machine Translation

확률론적 관점에서 번역은 source 문장 $x$가 주어진 상태에서 target 문장 $y$의 조건부 확률을 최대화하는

y를 찾는 것. $arg max_yp(y\vert x)$

모델이 조건부 분포를 학습하게 되면, 주어진 soruce 문장에 조건부 확률을 최대화하는 target 문장을

검색하여 번역을 할 수 있음.

최근 이러한 조건부 분포를 직접 학습시키기 위해, 신경망의 사용이 제안되었음.

NMT(Neural Machine Translation)의 접근법은 아래와 같이 두 가지 요소로 구성됨.

> **1. source 문장 x를 encode 하는 과정**
>
> **2. target 문장 y로 decode하는 과정**

예를 들어 GRU, LSTM같은 두 개의 RNN 기반의 접근법은, 새로운 방식임에도 불구하고

놀라운 성능을 보여주었음.

따라서 RNN Encoder-Decoder라고도 불리는데, 밑에서 이 구조를 조금 더 설명한다.

### 2.1 RNN Encoder-Decoder

 Encoder는 source 문장 x를 $\mathbb{x}=(x_1,..., x_{T_x})$를 벡터 $c$의 형태로 읽어냄.

가장 일반적인 접근법은 아래와 같은 형태의 RNN을 사용하는 것

$$h_t = f(x_t, h_{t-1}) \tag{1}$$

$$c = q(\{h_1,..., h_{T_x}\})$$

> **$h_t \in \mathbb{R}^n$** 는 시간 $t$에서의 hidden state, $c$는 hidden state의 시퀀스에서 생성된 (컨텍스트)벡터
>
> **$f$** 와 **$q$** 는 비선형 함수

Decoder 과정은 주어진 컨텍스트 벡터인 $c$를 사용해서 다음 단어인 $y_{t'}$를 예측한다.

이러한 과정을 반복해서 전체 단어들인 ${y_1,...,y_{t'-1}}$을 예측한다.

즉 decoder는 다음의 확률로 해석될 수 있다.

target 문장 y는 특정 time stamp에서 예측된 단어 $y_1$부터 $y_t−1$까지와 컨텍스트 벡터 $c$일 때 $y_t$일 확률들의 곱으로 표현된다.

$$p(y_t\vert \{y_1,...,y_{t-1}\}, c) = g(y_{t-1}, s_t, c) \tag{3}$$

> **$g$** 는 $y_t$의 확률을 출력하는 비선형이면서 multi-layer인 함수
>
> **$s_t$** 는 RNN의 hidden state

## 3. Learning to align and translate

논문에서 새롭게 제안된 모델의 구조

![https://dos-tacos.github.io/images/lynn/190420/1.PNG](https://dos-tacos.github.io/images/lynn/190420/1.PNG)

### 3.1 Decoder: general description

이전 section에서 정의한 조건부 확률을 다시 새롭게 정의한다.

$$p(y_i\vert y_1,...,y_{i-1},\mathbf{x})=g(y_{i-1},s_i,c_i)$$

$$s_i = f(s_{i-1}, y_{i-1}, c_i).$$

> **$s_i$** 는 시간 $i$에서의 hidden state

달라진 점이 무엇일까?

바로 컨텍스트 벡터 $c$가 바뀌었다.

target $y_i$에 대하여, 각각의 다른 컨텍스트 벡터 $c_i$를 가지게 된다.

컨텍스트 벡터 $c_i$는 encoder가 입력 문장을 매핑한 결과인 일련의 sequence of annotation인 $h_1, ..., h_{T_x}$에 따라 달라지게 된다.

참고로 annotation이란 **Attention**의 또다른 표현이다.

여기서 각각의 annotation $h_i$는입력 문장 $i$번째 단어 주변에 좀 더 focus한 정보를 가지고 있다.

따라서 컨텍스트 벡터 $c_i$는 각 $h_i$들에 weight들을 각각 곱해서 계산된다.

$$c_i=\sum^{T_x}_{j=1}\alpha_{ij}h_j$$

이렇게 각 Hidden State에 대해 $i$번째 output과 $j$번째 input이 어느만큼 관련이 있느냐를 학습

하려는 모델이 바로 이 논문이 제시하고자 하는 모델이다.

각 annotation $h_j$의 가중치 $\alpha_{ij}$는 아래와 같이 계산되어 지면서 0~1 사이의 확률로 나타나게 된다.

$$\alpha_{ij} = \dfrac{\exp(e_{ij})}{\sum_{k=1}^{T_x}\exp(e_{ik})}, \tag{6}$$

$e_{ij}$는 alignment model로, $i$번째 output과 $j$번째 input이 얼마나 잘 매칭되는지를 평가한다.

$$e_{ij} = a(s_{i-1}, h_j)$$

기존의 NMT와 달리,  alignment는 잠재 변수(latent variable)로 간주되지 않고 여기서는 하나의 network의 variable이 된다.

대신 alignment 모델은 soft-alignment를 직접 계산하여 cost function의 gradient를 역전파할 수 있음.

따라서 학습 과정에서 cost의 gradient는 역전파를 통해,  

(1): alignment model, (2): translation model 두 개의 network 학습에 사용된다.

그리고 컨텍스트 벡터 $c_i$에서 각 annotation $h_i$들에 weight들을 각각 곱해서 계산되어지는 것은

annotation의 기대값을 구하는 것이라 생각하면 된다.

### 3.2 Encoder: Bidirectional RNN for annotating sequences

일반적인 RNN 구조에서는 input sequence에 대해 순방향으로 차례대로 계산을 하게 된다.

여기에서는 annotation이 앞선 단어에 대한 정보 뿐만 아니라, 이후에 오는 정보도 포함할 수 있도록

양방향의 RNN을 사용했다.

forward hidden state $\vec{h_j}$와 backward $\overleftarrow{h_j}$를 concatenate하여, 각 단어 $x_j$에 대한 annotation을 얻음.

$$h_j=\big[{\overset{\rightarrow}{h}}^T_j;{\overset{\leftarrow}{h}}_j^T\big]$$

## 4. Experiment Settings
생략

## 5. Results

![https://i.imgur.com/cNPVifz.jpg](https://i.imgur.com/cNPVifz.jpg)

위 그림은 각각의 단어에 대해 annotation $\alpha_{ij}$ 값을 grayscale로 나타낸 그림이다.

그림을 보면 각각 단어가 매칭되는 부분에서 annotation이 높은 값을 가지는 것을 알 수 있다.
