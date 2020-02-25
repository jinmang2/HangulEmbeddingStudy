# Korean Embedding
한국어 임베딩 스터디 내역을 기록합니다.

### Reference
- http://ratsgo.github.io/embedding

### book

본 튜토리얼은 다음 도서를 보완하기 위해 작성됐습니다. 도서를 구매하지 않아도 튜토리얼 수행에 문제는 없으나 일부 내용은 도서를 참고해야 그 맥락을 완전하게 이해할 수 있습니다. 다음 그림을 클릭하면 도서 구매 사이트로 이동합니다.

<a href="http://www.yes24.com/Product/Goods/78569687"><img src="https://i.imgur.com/j03ENCc.jpg" width="500px" title="embeddings" /></a>

- [정오표](https://ratsgo.github.io/embedding/notice.html)

### embedding methods

본 튜토리얼에서 다루는 임베딩 기법은 다음과 같습니다.

- 단어 수준 임베딩
  - Latent Semantic Analysis
  - Word2Vec
  - GloVe
  - FastText
  - Swivel
- 문장 수준 임베딩
  - Weighted Embeddings
  - Latent Semantic Analysis
  - Latent Dirichlet Allocation
  - Doc2Vec
  - Embeddings from Language Models (ELMo)
  - Bidirectional Encoder Representations from Transformer (BERT)

### corpus preprocess

임베딩 학습데이터를 만들기 위해서는 전처리(preprocess)를 해야 합니다. 본 튜토리얼에서 다루는 오픈소스 패키지는 다음과 같습니다.

- KoNLPy : http://konlpy.org
- Khaiii : https://github.com/kakao/khaiii
- soynlp : https://github.com/lovit/soynlp
- sentencepiece : https://github.com/google/sentencepiece
