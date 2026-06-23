# Shopping Price Comparator (이미지 기반 쇼핑 가격 비교)

> Capture a photo of any item or receipt → AI extracts the product name and price → compare instantly with Coupang, Naver Shopping, or Google.
> 물건이나 영수증 사진을 찍으면 AI가 품목명·가격을 추출하고, 온라인몰 최저가를 바로 비교해 주는 Streamlit 앱입니다.

---

## 🛠️ Tech Stack (기술 스택)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![Streamlit](https://img.shields.io/badge/Streamlit-1.39%2B-FF4B4B?logo=streamlit)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4.1--mini-412991?logo=openai)
![LangChain](https://img.shields.io/badge/LangChain-0.3%2B-1C3C3C?logo=langchain)
![pandas](https://img.shields.io/badge/pandas-3.0%2B-150458?logo=pandas)

| Layer | Technology |
|---|---|
| UI | Streamlit |
| Image Analysis | OpenAI Vision (`gpt-4.1-mini`) |
| Price Comparison LLM | LangChain + `ChatOpenAI` (streaming) |
| Content Moderation | OpenAI Moderation (`omni-moderation-latest`) |
| STT | `gpt-4o-mini-transcribe` |
| TTS | `gpt-4o-mini-tts` |
| Note Storage | Local JSON files |

---

## ✨ Features (주요 기능)

- **Image-based item extraction (이미지 품목 추출)** — Camera input or file upload; OpenAI Vision parses product name and price from any photo or receipt.
- **Editable results (결과 수정)** — Edit extracted name, price, and quantity before adding to the list.
- **Shopping list management (쇼핑 목록 관리)** — Checkbox-driven todo-style list; per-item and total price calculation; selective delete.
- **Online price comparison (온라인 가격 비교)** — Select items and a platform (Coupang / Naver Shopping / Google); LangChain streams a personalized comparison guide with direct search links.
- **Content moderation (입력 안전 검사)** — User-edited text is screened with OpenAI Moderation before being added to the list.
- **STT / TTS support (음성 입력·출력)** — `gpt-4o-mini-transcribe` for speech-to-text, `gpt-4o-mini-tts` for text-to-speech.
- **Note service (메모 서비스)** — Study notes are structured, saved as JSON, and can be loaded in recency order.

---

## 📁 Project Structure (프로젝트 구조)

```
Shopping_app/
├── shopping_app.py     # Main Streamlit UI (메인 앱 화면)
├── openai_service.py   # OpenAI & LangChain API calls (AI 호출 로직)
├── storage_service.py  # JSON-based note persistence (노트 파일 저장/조회)
├── note_service.py     # Note formatting helpers (노트 데이터 가공)
├── config.py           # Env vars & directory setup (환경 설정)
├── requirements.txt    # Python dependencies
├── notes/              # Auto-created; stores note JSON files
└── audio_outputs/      # Auto-created; stores TTS audio files
```

---

## 🔄 Usage Flow (사용 흐름)

```
[카메라/파일 업로드]
        ↓
[OpenAI Vision → 품목명·가격 추출]
        ↓
[사용자 확인·수정 → 목록에 추가]
        ↓
[쇼핑 목록에서 비교할 항목 체크]
        ↓
[플랫폼 선택: 쿠팡 / 네이버쇼핑 / 일반]
        ↓
[LangChain 스트리밍 → 비교 가이드 + 검색 링크 출력]
```

1. Take a photo or upload an image.
2. Click **AI 분석 시작** — Vision model extracts item name and price.
3. Review and edit the result, then click **목록에 추가**.
4. Check the items you want to compare in the shopping list.
5. Choose a platform and click **가격 비교 시작** — a streaming AI response with direct links appears.

---

## 🏗️ Architecture (아키텍처)

```
shopping_app.py  (Streamlit UI layer)
        │
        ├── openai_service.py
        │       ├── analyze_image()          # Vision → structured JSON
        │       ├── is_flagged()             # Moderation check
        │       ├── stream_shopping_feedback() # LangChain chain (stream)
        │       └── calculate_selected_total()
        │
        ├── storage_service.py
        │       ├── save_note()              # Write JSON to /notes
        │       └── load_recent_notes()      # Read latest N notes
        │
        ├── note_service.py
        │       ├── note_to_markdown()       # Dict → Markdown string
        │       └── build_note_filename()    # Safe filename from title
        │
        └── config.py
                └── Loads .env → exposes API keys, model names, paths
```

LangChain chain (in `openai_service.py`):

```
ChatPromptTemplate → ChatOpenAI (streaming=True) → StrOutputParser
```

---

## ⚙️ Environment Setup (환경 설정)

Create a `.env` file in the project root:

```env
# Required (필수)
OPENAI_API_KEY=sk-...

# Optional — defaults shown (선택, 기본값 표시)
DEFAULT_MODEL=gpt-4.1-mini
STT_MODEL=gpt-4o-mini-transcribe
TTS_MODEL=gpt-4o-mini-tts
TTS_VOICE=alloy
MODERATION_MODEL=omni-moderation-latest
```

| Variable | Default | Description |
|---|---|---|
| `OPENAI_API_KEY` | — | OpenAI API key **(required)** |
| `DEFAULT_MODEL` | `gpt-4.1-mini` | Vision + chat model |
| `STT_MODEL` | `gpt-4o-mini-transcribe` | Speech-to-text model |
| `TTS_MODEL` | `gpt-4o-mini-tts` | Text-to-speech model |
| `TTS_VOICE` | `alloy` | TTS voice preset |
| `MODERATION_MODEL` | `omni-moderation-latest` | Content moderation model |

The directories `notes/` and `audio_outputs/` are created automatically on first run.

---

## 🚀 How to Run (실행 방법)

```bash
# 1. Clone the repository
git clone https://github.com/vosnuev/Shopping_app.git
cd Shopping_app

# 2. Install dependencies
pip install -r requirements.txt

# 3. Set up environment variables
cp .env.example .env   # then fill in OPENAI_API_KEY

# 4. Run the app
streamlit run shopping_app.py
```

The app opens at `http://localhost:8501` by default.

---

## 📄 License & References (라이선스 & 참고 문서)

- [OpenAI API Docs](https://platform.openai.com/docs)
- [LangChain Docs](https://python.langchain.com/docs/)
- [Streamlit Docs](https://docs.streamlit.io/)
