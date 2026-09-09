# APPS LangGraph 입문 강의자료

이 폴더는 3주 동안 LangGraph의 기초를 익히기 위한 자료입니다. **Week 0은 세 번의 본 수업에 포함되지 않는 사전 환경 설정**이며 첫 모임 전에 각자 완료합니다.

이 과정의 목표는 API 사용법을 많이 외우는 것이 아닙니다. 학습자가 다음 질문에 자신의 말로 답할 수 있도록 하는 것이 목표입니다.

- Agent는 일반 챗봇과 무엇이 다른가?
- State는 언제, 어떻게 변하는가?
- 모델이 Tool을 요청하는 것과 실제로 실행하는 것은 어떻게 다른가?
- 어떻게 대화를 기억하고, 사람의 승인을 받은 뒤 안전하게 행동하는가?

## 학습 원칙

Week 0은 설정 완료가 목적이므로 안내대로 위에서부터 실행하면 됩니다. 아래 학습 원칙은 **Week 1~3 본 진도**에 적용합니다.

Week 1~3 노트북은 “위에서부터 Run All”만 하는 데모 자료가 아닙니다. 각 절을 다음 순서로 학습합니다.

1. **읽기**: 업무 상황과 개념 설명을 먼저 읽습니다.
2. **예측하기**: 코드를 실행하기 전에 State, 출력, 다음 Node를 예상합니다.
3. **실행하기**: 예상과 실제 결과를 비교합니다.
4. **설명하기**: “이 코드가 무엇을 했는지”를 한 문장으로 적습니다.
5. **바꾸기**: 빈칸을 채우고 업무 규칙 하나를 변경합니다.
6. **적용하기**: 같은 구조를 새로운 문제에 적용합니다.

예시 출력과 정답은 바로 보지 말고, 먼저 직접 예측하거나 작성한 뒤 확인합니다.

## 3주 학습 지도

Week 1~3은 **대학 학생지원 문의 처리 시스템**이라는 하나의 업무 영역을 점진적으로 확장합니다. 각 실습은 학습 목적에 맞는 State 이름과 칸을 사용하지만, 모두 같은 학생지원 업무를 다룹니다.

| 주차 | 핵심 질문 | 필수 개념 | 최종 산출물 |
|---|---|---|---|
| 사전 준비 · Week 0 | 수업 코드를 실행할 준비가 되었는가? | Jupyter, 가상환경, 패키지, API 키 | 환경 진단과 연결 확인 |
| Week 1 | 업무 흐름을 그래프로 어떻게 표현할까? | Agentic AI, State, Node, Edge, 조건 분기 | 학사 문의 라우팅 그래프 |
| Week 2 | 모델은 외부 도구를 어떻게 선택하고 사용할까? | Message, Reducer, Tool call, ToolNode, ReAct, 종료 조건 | 학사 규정 조회 Agent |
| Week 3 | 어떻게 기억하고, 멈추고, 사람의 승인을 받을까? | Checkpointer, thread, Memory, interrupt, 안전한 재개 | 사람 승인을 포함한 업무 Agent |

별도 표시가 없는 절은 **필수**입니다. `[보충]`, `[심화]`, `선택 부록`, `수업 후 과제`로 표시한 내용은 핵심 평가 범위에 포함하지 않습니다.

### 3주 핵심에서 제외한 심화 주제

다음 주제는 유용하지만 초보자가 3주 안에 모두 구현하기에는 부담이 큽니다. 팀 프로젝트에서 실제 필요가 생기면 추가로 다룹니다.

- Pydantic strict validation
- 대화 trim과 자동 요약 그래프
- SQLite Checkpointer와 DB 기반 Store 구현
- 정적 breakpoint, `update_state()`, time travel
- 병렬 Node와 Reducer 합류


## 1. Python과 VS Code 준비

- Python 3.11 또는 3.12를 설치합니다.
- VS Code에서 `Python`, `Jupyter` 확장을 설치합니다.
- 터미널에서 이 폴더로 이동한 뒤 가상환경을 만듭니다.

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --user --name apps-langgraph --display-name "APPS LangGraph"
```

Windows PowerShell:

```powershell
py -3.12 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m ipykernel install --user --name apps-langgraph --display-name "APPS LangGraph"
```

노트북 오른쪽 위의 **커널 선택**에서 `APPS LangGraph` 또는 이 폴더의 `.venv`를 선택합니다.

## 2. OpenAI API 키 준비

`.env.example`을 복사해 `.env`라는 이름으로 저장하고 값을 채웁니다.

```dotenv
OPENAI_API_KEY=your_openai_api_key_here
```

- OpenAI 키는 Week 0와 Week 2의 LLM 실습에 사용합니다.
- `.env`는 GitHub, 단톡방, 압축파일, 제출물에 포함하지 않습니다.
- 키 문자열을 코드 셀이나 캡처에 넣지 않습니다.
- 가능하면 교육용 OpenAI Project에 학회원을 초대하고 개인별 사용 한도를 설정합니다.

## 3. 자주 발생하는 문제

| 증상 | 먼저 확인할 것 |
|---|---|
| `ModuleNotFoundError` | 선택한 커널이 `.venv`인지 확인 |
| `유효한 키가 없어 연결 확인을 건너뜀` | `.env.example`을 `.env`로 복사하고 placeholder를 교체했는지 확인 |
| `AuthenticationError`, HTTP 401 | 키 오타와 앞뒤 공백 확인. 노출된 키는 폐기 후 재발급 |
| `RateLimitError`, HTTP 429 | Project 사용량과 모델별 rate limit 확인 후 재시도 |
| `APITimeoutError`, `ConnectionError` | 인터넷, 학교망 방화벽, VPN 상태 확인 |
| 셀 결과가 설명과 다름 | LLM 문장은 매번 다를 수 있음. 문장 대신 Message 타입과 Tool call 구조 확인 |
| 예전 대화가 섞임 | `thread_id`와 Checkpointer를 새로 만들었는지 확인 |
| 그래프 그림이 안 나옴 | Mermaid PNG 생성이 실패하면 텍스트 Mermaid 출력을 사용 |

질문할 때는 **오류가 난 셀, 오류의 마지막 5줄, Python 버전, 커널 이름**을 함께 전달합니다. API 키는 캡처하지 않습니다.


## 4. 3주 종료 기준

학습자가 다음을 **코드를 보지 않고 설명하거나, 제공된 스캐폴드에서 구현**하면 기초 목표를 달성한 것입니다.

- State의 칸과 타입을 정의하고 Node의 partial update를 추적한다.
- 선형 Edge와 조건부 Edge를 사용한 3~4개 Node 그래프를 만든다.
- Tool 요청과 실행 결과를 다른 Message 타입으로 구분한다.
- 도구 사용과 종료 경로가 있는 작은 ReAct 그래프를 완성한다.
- Checkpointer와 `thread_id`의 역할을 설명하고 대화를 분리한다.
- 위험한 행동 전에 `interrupt()`를 두고 승인과 거부를 모두 처리한다.


검증 기준일: 2026-09-09. 패키지 버전은 `requirements.txt`에 고정되어 있습니다.
