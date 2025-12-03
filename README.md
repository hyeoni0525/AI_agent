# AI_agent
# 🎬 AI Instructor Agent: PPT to Video Generator

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![LangChain](https://img.shields.io/badge/LangChain-LangGraph-orange)](https://www.langchain.com/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-green)](https://openai.com/)
[![Gradio](https://img.shields.io/badge/Gradio-UI-yellow)](https://gradio.app/)

> **"PPT 파일만 업로드하세요. 대본 작성부터 음성 합성, 영상 제작까지 AI가 수행합니다."** > LangGraph 기반의 멀티모달 에이전트 워크플로우를 활용한 자동화 강의 영상 제작 서비스입니다.

---

## 1. Demo & Result

*(여기에 실제 구동 화면 GIF나 결과물 영상 캡처본을 넣어주세요. 방문자의 시선을 가장 먼저 사로잡는 곳입니다.)*

![Demo Animation](path/to/demo.gif)

* **입력:** 비즈니스 계획서, 기술 소개서 등 `.pptx` 파일
* **출력:** AI 강사가 설명하는 `.mp4` 강의 영상

---

## 2. Key Features

이 프로젝트는 정적인 문서를 동적인 영상 콘텐츠로 변환하는 **End-to-End AI Pipeline**을 구축했습니다.

* [cite_start]**📊 PPT 구조화 (Multimodal Parsing):** `python-pptx`를 활용하여 슬라이드 내 텍스트, 표, 이미지를 각각 분리하여 추출하고 구조화합니다. [cite: 1264]
* [cite_start]**🤖 LangGraph 기반 워크플로우:** 순차적인 작업 흐름(Parsing → Scripting → TTS → Video)을 그래프(Graph) 형태로 설계하여 안정적인 상태(State) 관리를 구현했습니다. [cite: 1250]
* [cite_start]**📝 문맥 인식 대본 생성 (Context-Aware Scripting):** 단순히 텍스트를 읽는 것이 아니라, 이전 슬라이드의 맥락을 고려하고 부족한 정보는 **SerpAPI(구글 검색)**를 통해 보완하여 풍부한 해설 대본을 작성합니다. 
* [cite_start]**🗣️ 자연스러운 음성 합성 (TTS):** OpenAI의 `gpt-4o-mini-tts` 모델을 사용하여 `alloy`, `echo` 등 다양한 톤의 네레이션을 생성합니다. [cite: 1238, 1502]
* [cite_start]**🎬 자동 영상 병합:** `FFmpeg`를 활용하여 생성된 음성과 슬라이드 이미지를 결합, 최종 강의 영상을 렌더링합니다. [cite: 1276]

---

## 3. Tech Stack

| Category | Technology | Description |
| :--- | :--- | :--- |
| **Language** | Python | Main Programming Language |
| **Framework** | **LangChain & LangGraph** | [cite_start]State-based Agent Workflow Orchestration [cite: 1250] |
| **LLM** | **OpenAI GPT-4o-mini** | [cite_start]Multimodal content understanding & Script generation [cite: 1223] |
| **TTS** | OpenAI TTS | [cite_start]Text-to-Speech (gpt-4o-mini-tts) [cite: 1238] |
| **Tools** | python-pptx | [cite_start]PowerPoint parsing & data extraction [cite: 1264] |
| **Search** | SerpAPI | [cite_start]Google Search Tool for context enrichment [cite: 1616] |
| **Video** | FFmpeg | [cite_start]Audio/Video processing & merging [cite: 1276] |
| **UI/UX** | Gradio | [cite_start]Web Interface for easy interaction [cite: 1697] |

---

## 4. System Architecture

이 프로젝트는 **LangGraph**를 사용하여 각 단계(Node)를 정의하고, `State`를 통해 데이터를 순환시키는 구조로 설계되었습니다.

### 🛠️ Workflow Diagram
*(아래는 프로젝트 PDF의 그래프 흐름을 도식화한 것입니다. 다이어그램 이미지가 있다면 교체하세요.)*

```mermaid
graph LR
    A[Start] --> B(Parse PPT)
    B --> C{Tool Search needed?}
    C -- Yes --> D(Google Search)
    C -- No --> E(Generate Content)
    D --> E
    E --> F(Generate Script)
    F --> G(TTS Audio)
    G --> H(Make Video Clip)
    H --> I{Last Page?}
    I -- No --> E
    I -- Yes --> J(Merge All Videos)
    J --> K[End]
