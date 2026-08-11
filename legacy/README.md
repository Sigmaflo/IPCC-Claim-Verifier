# legacy — IPCC AR6 RAG 챗봇 (v1) 산출물 보관

이 폴더는 프로젝트가 "뉴스 기사의 IPCC 인용 사실검증 시스템"으로 주제를 전환하기 전,
**IPCC AR6 한글 번역본 기반 RAG 챗봇** 시절(Phase 1~3)의 산출물을 보관한다.

## 구성

| 경로 | 내용 |
|---|---|
| `app/` | 배포됐던 애플리케이션 코드 (FastAPI 백엔드 + Streamlit 프론트엔드) |
| `Dockerfile` | Cloud Run 배포용 이미지 정의 (`app/backend` 기준) |
| `.env.example` | 구 백엔드 환경변수 템플릿 (`UPSTAGE_API_KEY`, `CHROMA_DIR`) |
| `proposals/` | IEP 실험 제안·결과 문서 15개 (IEP-0000 템플릿 ~ IEP-4005·4006) |
| `notebooks/` | IEP 실험 노트북 16개 + 실험 결과 인덱스 (`notebooks/README.md`) |
| `chatbot-stack.md` | 구 CLAUDE.md에서 옮겨온 확정 스택 설정값·코드 규칙 |

## 배포 관련 주의

- **Streamlit Cloud** 데모는 이 레포의 `app/frontend/app.py` 경로를 바라보도록 설정되어
  있었으므로, 이 이동이 main에 병합되면 데모가 깨진다.
  데모를 되살리려면 Streamlit Cloud 앱 설정에서 메인 파일 경로를
  `legacy/app/frontend/app.py`로 변경하면 된다.
- **Cloud Run** 백엔드는 이미 빌드된 이미지로 동작하므로 파일 이동의 즉시 영향은 없다.
  재배포 시에는 `legacy/Dockerfile` 경로 기준으로 빌드해야 한다
  (빌드 컨텍스트의 `COPY app/backend/` 경로 주의).

## 처분 방침

이 코드의 재사용/폐기 여부는 새 주제(사실검증 시스템) 요구사항 인터뷰가 끝난 뒤 결정한다.
특히 `app/backend/`의 검색 레이어(ChromaDB cosine + threshold + Solar 클라이언트)는
새 주제에서 재사용 후보다.
