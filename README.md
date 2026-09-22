# 🎙️ Voice-based Image Retrieval

> Whisper + CLIP 기반 음성 이미지 검색 시스템

**Deep Learning Final Project**

음성 질의를 텍스트로 변환한 뒤, CLIP 임베딩 공간에서 관련 이미지를 검색하는 음성 기반 이미지 검색 시스템을 구현했습니다.

---

## 📌 Project Overview

### Objective

사용자가 말한 문장을 이미지 검색 질의로 사용하여,

1. 음성 질의를 텍스트로 변환
2. 텍스트와 이미지를 동일한 임베딩 공간으로 변환
3. Cosine Similarity를 이용하여 관련 이미지 검색
4. 유사도가 높은 Top-K 이미지를 반환

하는 전체 파이프라인을 구현했습니다.

---

## 🔄 Overall Pipeline

```text
Voice Query
     ↓
Whisper ASR
     ↓
Text Query
     ↓
CLIP Text Encoder
     ↓
Text Embedding
     ↓
Cosine Similarity
     ↑
Image Embedding
     ↑
CLIP Image Encoder
     ↑
Flickr8k Images
     ↓
Top-5 Retrieved Images
```

---

## 🛠️ Models & Methods

| Component | Model / Method |
|---|---|
| Speech-to-Text | Whisper Tiny English |
| Text Embedding | CLIP ViT-B/32 |
| Image Embedding | CLIP ViT-B/32 |
| Similarity | Cosine Similarity |
| Image Dataset | Flickr8k |
| Retrieval | Top-K Ranking |

### Whisper Tiny English

음성 파일을 영어 텍스트 질의로 변환하기 위해  
**Whisper Tiny English** 모델을 사용했습니다.

```text
Voice → Text
```

### CLIP ViT-B/32

Whisper가 변환한 텍스트와 이미지 데이터를  
공통 임베딩 공간으로 매핑하기 위해 **CLIP ViT-B/32**를 사용했습니다.

```text
Text  → 512-dimensional embedding
Image → 512-dimensional embedding
```

---

## 🔍 Retrieval Process

### 1. Voice Query Processing

입력 음성 파일을 전처리한 후 Whisper ASR을 이용하여 텍스트 질의로 변환합니다.

```text
Input: query.m4a
        ↓
Audio Preprocessing
        ↓
Whisper Tiny English
        ↓
Text Query
```

예시:

> `"A cyclist riding a bike"`

---

### 2. Image Database Construction

Flickr8k의 일부 이미지를 이미지 검색 후보군으로 사용했습니다.

각 이미지를 CLIP Image Encoder에 입력하여 이미지 임베딩을 생성하고,  
이를 사전에 계산하여 Embedding Database를 구성했습니다.

```text
Flickr8k Images
      ↓
Image Preprocessing
      ↓
CLIP Image Encoder
      ↓
Normalized Image Embeddings
      ↓
Embedding Database
```

이미지 임베딩은 **N × 512** 형태로 구성됩니다.

---

### 3. Text Embedding

Whisper가 변환한 텍스트 질의를 CLIP Text Encoder에 입력하여  
512차원의 텍스트 임베딩 벡터로 변환합니다.

```text
Text Query
    ↓
CLIP Text Encoder
    ↓
512-d Text Embedding
```

---

### 4. Similarity Search

정규화된 텍스트 임베딩과 이미지 임베딩 사이의  
**Cosine Similarity**를 계산합니다.

유사도 점수를 기준으로 내림차순 정렬한 후  
상위 **Top-5 이미지**를 최종 검색 결과로 반환합니다.

```text
Text Embedding
      +
Image Embeddings
      ↓
Cosine Similarity
      ↓
Similarity Ranking
      ↓
Top-5 Images
```

---

## 🧪 Experiment

### Input

**Voice Input**

```text
query.m4a
```

**Recognized Text**

```text
"A cyclist riding a bike"
```

### Final Output

음성 질의를 텍스트로 변환한 후  
자전거 및 라이딩과 관련된 이미지가 포함된 **Top-5 검색 결과**를 확인했습니다.

---

## 📊 Result

프로젝트를 통해 다음과 같은 전체 알고리즘 파이프라인을 구현했습니다.

- 음성 입력 → 텍스트 변환
- Whisper를 이용한 Speech-to-Text
- CLIP을 이용한 Text/Image Embedding
- Cosine Similarity 기반 이미지 검색
- Top-K 이미지 검색 결과 생성 및 시각화

### Conclusion

Whisper와 CLIP을 연결하여  
**음성 입력부터 이미지 검색까지의 전체 알고리즘 파이프라인을 구현했습니다.**

---

## ⚠️ Limitations

- CPU 환경으로 인해 일부 데이터셋만 사용
- Zero-shot 방식으로 인해 일부 검색 결과가 완벽하게 일치하지 않을 수 있음
- 음성 품질에 따라 ASR 결과가 달라질 수 있음

---

## 💡 Future Work

- 더 큰 이미지 데이터셋 적용
- GPU 기반 이미지 임베딩 사전 계산
- 한국어 질의 처리
- 한국어 음성 질의를 위한 번역 모듈 추가

---

## 📄 Project Presentation

프로젝트의 전체 알고리즘과 실험 결과는 발표자료에서 확인할 수 있습니다.

📎 **[View Project Presentation](./docs/Voice-based_Image_Retrieval.pdf)**
