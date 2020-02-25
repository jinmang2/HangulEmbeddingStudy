# 3. 한국어 전처리

Date: Feb 18, 2020
Reviewer: gyungsu min, KwangJune Choi
Status: Yes

책의 수록된 내용을 자신만의 방법으로, 설명 및 요약해서 소개해주시면 됩니다.
책을 읽다가 모르는 부분은 단톡방에 물어보시거나 다 같이 고민해보면 됩니다.
소개 자료는 Word, PPT, Notion, Markdown등 형식에 구애받지 않습니다.
너무 부담갖지 않으셔도 됩니다!!

## [3장에서 다루는 내용]

- 3.1 데이터 확보 & 한국어 전처리

        -  위키백과, KorQuAD, 네이버 영화 리뷰 말뭉치

- 3.2 지도학습 기반 형태소분석

        -  KoNLPy, Khaiii, 은전한닢(mecab)

- 3.3 비지도학습 기반 형태소분석

        -  soynlp, 구글 센텐스피스, 띄어쓰기 교정

### 3.1 데이터 확보 & 한국어 전처리

**전처리 완료된 데이터 목록**

 (1) 한국어 위키백과 (p.80)

       - 한국어 위키백과 원문(약 31만 line)

       - processed_wiki_ko.txt

제임스 얼 "지미" 카터 주니어 (, 1924년 10월 1일 ~ )는 민주당 출신 미국 39번째 대통령 (1977년 ~ 1981년)이다. 지미 카터는 조지아주 섬터 카운티 플레인스 마을에서 태어났다. 조지아 공과대학교를 졸업하였다. 그 후 해군에 들어가 전함·원자력·잠수함의 승무원으로 일하였다. 1953년 미국 해군 대위로 예편하였고 이후 땅콩·면화 등을 가꿔 많은 돈을 벌었다. 그의 별명이 "땅콩 농부" (Peanut Farmer)로 알려졌다. 1962년 조지아 주 상원 의원 선거에서 낙선하나 그 선거가 부정선거 였음을 입증하게 되어 당선되고, 1966년 조지아 주 지사 선거에 낙...

 (2) korQuAD (p.86)

       - 한국어 기계독해(주어진 질문에 대한 정답 예측)를 위한 데이터셋

       - by LG CNS, 2018

       - 한국어 위키백과에 기반해 데이터 7만 쌍 구축

             { 문단 1 : [질문-답변], [질문-답변]... }

             { 문단 2 : [질문-답변], [질문-답변]... }

       - 한국어 임베딩용 말뭉치로 적합 (p.86)

       - processed_korquad.txt

1839년 바그너는 괴테의 파우스트을 처음 읽고 그 내용에 마음이 끌려 이를 소재로 해서 하나의 교향곡을 쓰려는 뜻을 갖는다. 이 시기 바그너는 1838년에 빛 독촉으로 산전수전을 다 걲은 상황이라 좌절과 실망........
바그너는 괴테의 파우스트를 읽고 무엇을 쓰고자 했는가? 교향곡
바그너는 교향곡 작곡을 어디까지 쓴 뒤에 중단했는가? 1악장
바그너가 파우스트 서곡을 쓸 때 어떤 곡의 영향을 받았는가? 베토벤의 교향곡 9번
1839년 바그너가 교향곡의 소재로 쓰려고 했던 책은? 파우스트
파우스트 서곡의 라단조 조성이 영향을 받은 베토벤의 곡은? 합창교향곡
바그너가 파우스트를 처음으로 읽은 년도는? 1839
바그너가 처음 교향곡 작곡을 한 장소는? 파리
바그너의 1악장의 초연은 어디서 연주되었는가? 드레스덴

 (3) 네이버 영화 리뷰 말뭉치 (p.90)

       - 박은정 구축 : [https://github.com/e9t/nsmc](https://github.com/e9t/nsmc)

       - 전처리(processed) / 띄어쓰기 교정(corrected) 2개 카테고리로 분류됨.

         a. 전처리(processed) 데이터

             - 말뭉치 데이터(text + movie ID)

             - 비지도학습용 데이터(text)

             - 지도학습용 데이터(text + sentiment label)   :  train / test set

         a. 띄어쓰기 교정(corrected) 후 데이터

             - 비지도학습용 데이터(text)

             - 지도학습용 데이터(text + sentiment label)   :  train / test set

             - 비지도학습용 데이터(text)

 (4) 기타

       - 띄어쓰기 교정된 데이터를 통해 학습한 형태소 분석 모델(soyword)

       - 띄어쓰기 교정된 데이터를 통해 학습한 띄어쓰기 교정 모델(space-correct)

**전처리 완료된 데이터 다운로드 방법**

 (1) docker 활용 데이터 다운로드

  교재에서 활용되는 데이터를 docker container에서 쉽게 다운로드 받을 수 있음

    : GPU 환경 도커 컨테이너 띄우기
    docker pull ratsgo/embedding-gpu
    docker run -it --rm --runtime=nvidia ratsgo/embedding-gpu bash

    : CPU 환경 도커 컨테이너 띄우기
    docker pull ratsgo/embedding-cpu
    docker run -it --rm ratsgo/embedding-cpu bash   # rm : 컨테이너 날라감. 휘발성. rm 뺴고.

    # 파일 목록 확인 우분투 명령어
    root@de7f89818bf8:/notebooks/embedding# ll

: 실행 결과
합계 72
drwxr-xr-x 1 root root  4096  6월 29  2019 ./
drwxr-xr-x 1 root root  4096  9월  8 07:56 ../
drwxr-xr-x 8 root root  4096  6월 29  2019 .git/
-rw-r--r-- 1 root root  2793  6월 29  2019 .gitignore
-rw-r--r-- 1 root root  1068  6월 29  2019 LICENSE
-rw-r--r-- 1 root root   285  6월 29  2019 README.md
drwxr-xr-x 2 root root  4096  6월 29  2019 docker/
drwxr-xr-x 4 root root  4096  6월 29  2019 docs/
drwxr-xr-x 1 root root  4096  6월 29  2019 models/
drwxr-xr-x 2 root root  4096  6월 29  2019 preprocess/
-rwxr-xr-x 1 root root 10977  6월 29  2019 preprocess.sh*    :: 전처리 완료된 데이터 위치.
-rwxr-xr-x 1 root root 10974  6월 29  2019 sentmodel.sh*
-rwxr-xr-x 1 root root  7730  6월 29  2019 wordmodel.sh*
root@de7f89818bf8:/notebooks/embedding#

    : 최신 실습 코드 업데이트
    git pull origin master

    : 전처리 완료된 데이터 다운로드 -> to docker local directory.
    bash preprocess.sh dump-processed

Q. 위 코드 수행시 컴퓨터 로컬이 아닌 docker local directory로 파일이 저장되는것?

(2) web에서 데이터 다운로드

[https://ratsgo.github.io/embedding/downloaddata.html](https://ratsgo.github.io/embedding/downloaddata.html)



---

---

> 한국어는 "어근 + 접사(접두사 / 접미사 ...)"의 조합의로 단어의 기능이 결정되는 교착어이다. 따라서 단어의 주 의미를 내포하는 형태소(=어근)를 추출해 활용한다면 임베딩 등 NLP task의 효율성이 상승함.  → 형태소 분석

### 3.2 지도학습 기반 형태소분석

**KoNLPy**

 : 5개 형태소분석 오픈소스를 파이썬 환경에서 사용할 수 있도록 인터페이스를 통일한 패키지.

설치시 java 환경 / 환경변수 설정 / whl.  등 조율해야 할 사항이 다소 많음.

[https://blog.naver.com/koys007/221460757885](https://blog.naver.com/koys007/221460757885)

    from konlpy.tag import Okt

    tokenizer = Okt()


    text_raw = '구조도 그렇고 구멍에 이빨이 들어갈수있어서 치석제거도 잘될꺼 같네욬ㅋ'
    text_refined = '구조도 그렇고 구멍에 이빨이 들어갈 수 있어서 치석 제거도 잘 될 것 같네요ㅋㅋ'

    # morphs : 텍스트를 형태소 단위로 분리

    refined_basic = okt.morphs(text_refined)
    refined_norm = okt.morphs(text_refined, norm=True)   # 문장 정규화
    refined_stem = okt.morphs(text_refined, stem=True)   # 어근 추출

    print(refined_basic)
    print(refined_norm)   
    print(refined_stem)


    # ['구조도', '그렇고', '구멍', '에', '이빨', '이', '들어갈', '수', '있어서', '치석', '제거', '도', '잘', '될', '것', '같네요', 'ㅋㅋ']
    # ['구조도', '그렇고', '구멍', '에', '이빨', '이', '들어갈', '수', '있어서', '치석', '제거', '도', '잘', '될', '것', '같네요', 'ㅋㅋ']
    # ['구조도', '그렇다', '구멍', '에', '이빨', '이', '들어가다', '수', '있다', '치석', '제거', '도', '자다', '되다', '것', '같다', 'ㅋㅋ']

    raw_basic = okt.morphs(text_raw)
    raw_norm = okt.morphs(text_raw, norm=True)
    raw_stem = okt.morphs(text_raw, stem=True)
    raw_norm_stem = okt.morphs(text_raw, norm=True, stem=True)

    print(raw_basic)
    print(raw_norm)   # poor
    print(raw_stem)   # soso
    print(raw_norm_stem)


    # ['구조도', '그렇고', '구멍', '에', '이빨', '이', '들어갈수있어서', '치석제거', '도', '잘', '될꺼', '같네욬', 'ㅋ']
    # ['구조도', '그렇고', '구멍', '에', '이빨', '이', '들어갈수있어서', '치석제거', '도', '잘', '될꺼', '같네요', 'ㅋ']
    # ['구조도', '그렇다', '구멍', '에', '이빨', '이', '들어가다', '치석제거', '도', '잘', '되다', '같네욬', 'ㅋ']
    # ['구조도', '그렇다', '구멍', '에', '이빨', '이', '들어가다', '치석제거', '도', '잘', '되다', '같다', 'ㅋ']



    # morphs : 품사 테깅
    # https://docs.google.com/spreadsheets/d/1OGAjUvalBuX-oZvZ_-9tEfYD2gQe7hTGsgUpiiBSXI8/edit#gid=0

    refined_pos = okt.pos(text_refined, norm=True, stem=True)
    refined_pos_join = okt.pos(text_refined, norm=True, stem=True, join=True)

    print(refined_pos)
    print(refined_pos_join)


    # [('구조도', 'Noun'), ('그렇다', 'Adjective'), ('구멍', 'Noun'), ('에', 'Josa'), ('이빨', 'Noun'), ('이', 'Josa'), ('들어가다', 'Verb'), ('수', 'Noun'), ('있다', 'Adjective'), ('치석', 'Noun'), ('제거', 'Noun'), ('도', 'Josa'), ('자다', 'Verb'), ('되다', 'Verb'), ('것', 'Noun'), ('같다', 'Adjective'), ('ㅋㅋ', 'KoreanParticle')]
    # ['구조도/Noun', '그렇다/Adjective', '구멍/Noun', '에/Josa', '이빨/Noun', '이/Josa', '들어가다/Verb', '수/Noun', '있다/Adjective', '치석/Noun', '제거/Noun', '도/Josa', '자다/Verb', '되다/Verb', '것/Noun', '같다/Adjective', 'ㅋㅋ/KoreanParticle']

- okt 패키지 사용자 사전을 추가하고 싶을 경우 아래 경로의 json파일에 단어를 추가하면 될 것으로 예상(불확실) :    Anaconda3\Lib\site-packages\konlpy\data\tagset\twitter.json

**Khaiii**

 : 카카오에서 2018년 공개한 한국어 형태소 분석기(CNN)

    # docker environment.
    # 윈도우에서는 실패

    # bash -> python

    from khaiii import KhaiiiApi
    tokenizer = KhaiiiApi()

    data = tokenizer.analyze('아버지가방에들어가신다')
    print(data[0])
     ## 아버지가방에들어가신다  아버지/NNG + 가/JKS + 방/NNG + 에/JKB + 들어가/VV + 시/EP + ㄴ다/EC

    tokens = list()

    for word in data:  # 왜 루프를 돌린건지는 모르겠음.
        tokens.extend([str(m).split("/")[0] for m in word.morphs])
        # word.morphs: 형태서 분석한 결과 리스트
        # str(word.morphs[0]) == 아버지/NNG'
        # str(word.morphs[0].split('/')[0] == 아버지

    print(tokens)
    # ['아버지', '가', '방', '에', '들어가', '시', 'ㄴ다']

### 3.3 비지도학습 기반 형태소분석

        : 품사 정보의 입력 없이, 데이터 패턴 학습을 통해 형태소 분석(자주 등장하는 단어 = 형태소)

**soynlp**

 : 비지도학습 특성상 규모가 확보된 + 동질적인 dataset에서 학습 성능 높음(뉴스 기사, 영화 댓글...).

 : 응집 확률(Cohesion Probability) & 브랜칭 앤트로피(Branching Entropy) 기반 단어 점수 표로 작동.

> Branching Entropy

![Untitled](https://user-images.githubusercontent.com/47301926/75226043-06bbe980-57ef-11ea-8fc5-9dc4de237bae.png)

[https://github.com/lovit/soynlp/blob/master/tutorials/wordextractor_lecture.ipynb](https://github.com/lovit/soynlp/blob/master/tutorials/wordextractor_lecture.ipynb)

1. n / na / nat / natu ..... 등 단어를 구성하는 문자가 많이 제공될 수록 이후에 올 수 있는 문자를 예측할 확률은 상승함 → 불확실성(앤트로피)의 감소.

      (확률은 출현 빈도수 기반)

1. natura / natural // ?    natural 이후 어떤 문자가 올 지 예측하는 단계에서 앤트로피 크게 증가.
2. "단어의 경계"에서 앤트로피가 높을 것이라는 전제 하에 단어와 단어를 구분할 수 있음(중국어)
3. Korean letters?  정보 앤트로피 개념을 문장/어절에서 단어를 추출하기 위해 사용 가능.

![Untitled 1](https://user-images.githubusercontent.com/47301926/75226047-0885ad00-57ef-11ea-95d2-b15ce308c491.png)

> Cohesion Probability

![Untitled 2](https://user-images.githubusercontent.com/47301926/75226048-0885ad00-57ef-11ea-8756-80ef50b18b19.png)

1. 응집 확률(Cohesion Probability), 즉 하나의 문자집합 내에서 "연결강도"가 높을 수록 해당 문자집합은 하나의 단어로 보는 개념.

      ex) corpus 내 문자집합을 카운팅.

     노 (200)
     노래 (100)
     노란 (100)
     노래가 (50)
     노래는 (30)
     노래를 (20)
     **노란색 (100)**

1. "노란색"이라는 문자집합의 연결 강도는?

        노 & 노란    +    노란 & 노란색

        *cohesion(노란색) = {P(노란|노) x P(노란색|노란)} ^ (1/2) = (0.5 x 1) ^ (1/2) = 0.707*

    page 102 ~ 104

**구글 센텐스피스(sentencepiece)**

 : 바이트 페어 인코딩(BPE, Bite Pain Encoding) 기봅에 기반한 형태소 분석 패키지

 : "문자열 압축"

      ex) "aaabdaaabac"      →      "aa"     →      "ZabdZabzc"

       "ZabdZabzc"      →      "ab"      →      "ZYdZYac"

        —> 어휘집합(Z, Y,...)이 원하는 크기가 될 때까지 반복. 이것이 BPE 학습.

Korean letters?

    corpus → BPE training → BPE model

    "학교에서 밥을 먹었다"   →  BPE model   →   _학교, 에서, _밥, 을, _먹었, 다

     * "_"는 어절(띄어쓰기 기준 분리된 문자열)에서 처음으로 나타나는 어휘집합(서브워드).

BERT?

    BERT 모델은 BPE로 학습한 어휘집합을 사용함.

    BPE 학습 → 후처리(##, pad, unk, cls, mask, sep) → 모델 적용.
