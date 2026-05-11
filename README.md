# 🛍️ Smart Shopping Assistant

사진을 찍으면 물품과 가격을 자동으로 추출하고, 온라인몰 가격과 비교할 수 있도록 도와주는 AI 기반 쇼핑 도우미 앱입니다.

---

## ✨ 주요 기능

- **이미지 분석**: 카메라 촬영 또는 파일 업로드로 물건 이름과 가격을 자동 추출 (GPT-4.1-mini Vision)
- **쇼핑 목록 관리**: 항목별 수량·가격 수정, 체크박스 선택, 선택 삭제, 합계 자동 계산
- **온라인 가격 비교**: 쿠팡 / 네이버쇼핑 / 일반 쇼핑몰 검색 링크 및 AI 가이드 제공 (스트리밍)
- **입력값 안전 검사**: OpenAI Moderation API로 부적절한 입력 필터링

---

## 🗂️ 프로젝트 구조

```
shopping_list_app/
├── shopping_app.py      # Streamlit 메인 UI
├── openai_service.py    # OpenAI API 호출 (이미지 분석, 가격 비교, 모더레이션)
├── config.py            # 환경변수 로드 및 경로 설정
├── note_service.py      # 노트 데이터 가공 유틸리티
├── storage_service.py   # JSON 파일 저장/로드
├── requirements.txt     # 의존성 패키지 목록
└── .env                 # API Key 설정 (git 제외)
```

---

## ⚙️ 설치 및 실행

### 1. 패키지 설치

```bash
pip install -r requirements.txt
```

### 2. 환경변수 설정

프로젝트 루트에 `.env` 파일을 생성하고 아래 내용을 입력하세요.

```
OPENAI_API_KEY=your_openai_api_key_here
DEFAULT_MODEL=gpt-4.1-mini
MODERATION_MODEL=omni-moderation-latest
```

### 3. 앱 실행

```bash
streamlit run shopping_app.py
```

---

## 🧩 사용 기술

| 항목 | 내용 |
|------|------|
| Frontend | [Streamlit](https://streamlit.io/) |
| AI | OpenAI GPT-4.1-mini (Vision, Chat, Streaming) |
| 안전 검사 | OpenAI Moderation API |
| 데이터 처리 | Pandas |
| 환경 관리 | python-dotenv |

---

## 📸 앱 사용 흐름

1. **사진 찍기** — 카메라 또는 파일 업로드로 물건 이미지 입력
2. **AI 분석** — 물건 이름과 가격 자동 추출, 수정 가능
3. **목록 추가** — 수량과 함께 쇼핑 목록에 등록
4. **가격 비교** — 원하는 항목 체크 후 온라인 최저가 검색 링크 확인

---

## 🔒 보안 주의사항

- `.env` 파일은 `.gitignore`에 등록되어 있어 GitHub에 업로드되지 않습니다.
- API Key는 절대 코드에 직접 작성하지 마세요.
