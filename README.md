# REAL DATA에 기반한 취업준비생의 "Personal Coach" 
## 🙌AI 대전 1 팀의 RAG 기반 챗봇 서비스
## ❓ 프로젝트 개요
- **프로젝트명**: DART 공시자료를 기반으로 삼성전자 취업준비생의 질문에 답변하는 RAG 기반 챗봇 서비스 구현
- **목표**:
  - 주제에 맞는 데이터를 선정, 정제, 활용하는 경험 쌓기
  - RAG 기반 기술로 챗봇 설계 및 실무 경험 강화
  - 사용자 맞춤형 기업 정보 제공으로 분석 효율 향상
- **결과물**:
  - 서비스 기획안
  - RAG 파이프라인
  - RAG 기반 챗봇 서비스 데모

---

## 🙋‍♀️ 주요 내용

### 문제 정의 및 해결방안
1. **정보 과부하**
   - 방대한 DART 자료에서 필요한 정보를 찾기 어려움.
2. **복잡한 전문 용어와 해석 난이도**
   - 회계·재무 용어로 인해 자료 해석 어려움.
3. **챗봇 응답의 신뢰도 및 정확도 부족**
   - 부정확한 요약 및 맥락 이해 부족.
4. **개인화된 컨텍스트 부족**
   - 사용자 맞춤 정보 제공 미흡.

**RAG 기반 해결 방안**
- **RAG**: 실시간 공시 문서 검색으로 LLM 최적화.
- **LLM**: 질의응답 및 텍스트 요약으로 간소화.
- **Chatbot**: 사용자 경험 극대화 및 개인화 제공.

---

## 🛠 주요 기능 및 기술적 특징

### 데이터 전처리
1. **OpenDART API 활용**:
   - 삼성전자 공시 목록을 2022년부터 2024년까지 수집.
2. **XML 데이터 추출**:
   - 주요 섹션: '주요 제품 및 서비스', '주요 계약 및 연구개발활동', '기타 참고사항'.
3. **데이터프레임 생성**:
   - 필요한 컬럼만 추출 후 `text` 컬럼에 결합하여 CSV 파일로 저장.

### RAG 파이프라인
1. **텍스트 파일 읽기**:
   - CSV 파일에서 `DataFrame` 형태로 데이터를 읽어오고 `Document` 형식으로 변환.
2. **텍스트 분할**:
   - `RecursiveCharacterTextSplitter`로 텍스트를 작은 조각으로 분할.
3. **임베딩 생성**:
   - `UpstageEmbedding`을 사용해 텍스트를 벡터화하여 Pinecone 인덱스에 저장.
4. **문서 검색**:
   - Ensemble(Retriever): Dense 0.7, Sparse 0.3.
5. **응답 생성**:
   - `LangChain` (LLM + Prompt + Parser)을 활용하여 쿼리와 추출된 문서로 결과 생성.

---

## 📡 API 활용 방법

1. **DART**:
   - 공시 데이터 수집.
2. **Pinecone**:
   - 벡터 스토리지 및 검색.
3. **Upstage**:
   - 임베딩 생성 및 LLM 활용.

---

## 📈 기대 효과
- **직접적 이점**:
  - 공시자료 분석 시간 절감.
  - 정보 이해도 및 면접 준비 효율 향상.
- **비즈니스 임팩트**:
  - 구독형 서비스 및 신규 비즈니스 모델 가능.
  - 운영 효율 극대화 및 비용 절감.

---

## 🧑‍💻 Code Block

```python
# RAG 기반 데이터 처리 예제
from langchain.vectorstores import Pinecone
from langchain.embeddings import UpstageEmbedding

def process_data(data):
    # 벡터화 및 저장
    embedding = UpstageEmbedding()
    vector_db = Pinecone(index_name="dart_data")
    processed_data = vector_db.add(data, embedding)
    return processed_data
```

## 삼성전자 공시자료 기반 챗봇 사용법
이 챗봇은 삼성전자의 공시자료를 기반으로 다양한 질문에 답변할 수 있도록 설계되었습니다. 아래의 활용 사례를 참고하여 효과적으로 사용해 보세요.
---

# 가능한 질문 예시
## 1. 삼성전자의 사업 및 기술 동향 파악
* 삼성전자가 주력으로 하고 있는 사업 분야는 무엇인가요?
* 최근 기술 개발 트렌드와 이에 따른 채용 확대 가능성이 있는 직무는 무엇인가요?
* 어떤 사업 부문에서 구조조정이나 축소 가능성이 있나요?
## 2. 자기소개서 첨삭 및 방향 제시
* 삼성전자의 최근 동향에 맞춘 자기소개서인지 확인할 수 있나요?
* 제가 작성한 자기소개서가 특정 직무에 적합한지 분석해 주세요.
* 어떤 강점과 경험을 자기소개서에 추가하면 더 효과적일까요?
## 3. 예상 면접 질문 및 답변 준비
* 삼성전자 채용 면접에서 나올 가능성이 높은 질문은 무엇인가요?
* 제 지원 직무와 관련된 심화 질문은 어떤 것들이 있을까요?
* 예상 질문에 대한 답변 피드백을 받을 수 있나요?

# 사용 방법 가이드
## 1. 질문하기
* 챗봇에게 위에 나열된 범위 내에서 자유롭게 질문하세요.
* 예시: "삼성전자의 반도체 사업이 앞으로 어떻게 될지 예상해 주세요."
## 2. 자료 활용하기
* 챗봇은 삼성전자의 최신 공시자료를 바탕으로 분석된 정보를 제공합니다.
* 답변이 끝난 후 관련 자료나 세부 정보를 요청할 수도 있습니다.
## 3. 답변 활용하기
* 제공받은 정보를 바탕으로 자기소개서를 수정하거나 면접 준비 자료를 작성해 보세요.
* 예시: 챗봇이 제공한 예상 면접 질문을 참고하여 답변을 연습하세요.

# 주의사항
* 챗봇의 답변은 참고용이며, 반드시 최신 자료와 함께 검토하는 것이 좋습니다.
삼성전자의 내부 정책이나 기밀 정보는 제공되지 않습니다.
이 챗봇을 통해 삼성전자 취업 준비에 실질적인 도움을 얻으시기 바랍니다! 😊

# 로컬 실행 방법
## Frontend
```python
npm run build
npm start
```

## Backend
1. requirements.txt를 참고하여 가상환경 설정
2. 환경변수 설정
3. 아래의 명령어 순서대로 실행
```python
python preprocessing.py
python vectorstore.py
python app.py
```

# 배포된 URL
## BACK(Fly.io)
https://ssafy-2024-backend-purple-frost-3496.fly.dev/

## FRONT(AWS)
https://master.d1p5kmphz6u7r6.amplifyapp.com/