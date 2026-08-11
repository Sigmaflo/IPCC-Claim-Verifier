# 구 챗봇(v1) 확정 스택 설정값

주제 전환 전 CLAUDE.md에 있던 확정 설정값과 코드 규칙의 보존본.
`legacy/app/` 백엔드를 재사용하기로 결정되면 이 내용을 새 CLAUDE.md 스택 규칙 섹션에 복원한다.

## 기술 스택
- 임베딩: jhgan/ko-sroberta-multitask (768차원)
- 벡터DB: ChromaDB (cosine), 컬렉션: ipcc_1001_case3_cosine_v1
- 청킹: chunk_size=1000, overlap=200 (CASE 3), 506청크
- SIMILARITY_THRESHOLD: 0.40 (cosine 기준, 2026-04-30 확정)
- 배포 LLM: 업스테이지 Solar (solar-pro3)
- 실험 LLM: llama3.1:8b (Ollama)
- 평가: RAGAS
- 인프라: GCP Cloud Run + Streamlit Cloud
- 실험 환경: Google Colab T4 / Mac Mini M4

## 확정된 설정값
- ChromaDB 메타데이터 키: page (int), source (PDF 경로) — chunk_id 없음
- similarity = 1 - cosine_distance (직접 변환, LangChain 의존 금지)
- TOP_K: 10
- Solar base_url: https://api.upstage.ai/v1
- 환경변수: UPSTAGE_API_KEY

## 코드 규칙
- f-string 내 \n 사용 금지
- Mac 로컬 실행 시 device="mps" (Colab은 "cuda", Cloud Run은 "cpu")
- ChromaDB 컬렉션 생성 시 반드시 metadata={'hnsw:space': 'cosine'} 명시
- similarity_search_with_relevance_scores 사용 금지 (버전별 변환 공식 불일치)

## 외부 저장소 (레포 밖 자산)
- 원문 PDF: KO_IPCC_AR6_SYR_FullVolume.pdf (188쪽) — Google Drive `MyDrive/IPCC_data/`
- 골든 데이터셋: `MyDrive/IPCC_data/IEP_1002/golden_dataset_100.csv`
- 운영 ChromaDB: `gs://ipcc-rag-chromadb/chroma_cosine/` (Cloud Run 기동 시 다운로드)
- 실험 ChromaDB: `MyDrive/IPCC_data/IEP_1001_CASE3/chroma_db`, `IEP_1002`, `IEP_1003`
