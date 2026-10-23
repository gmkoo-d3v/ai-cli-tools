# AI CLI Tools — Early Orchestration Archive

> **Early Archive | Multi-LLM Orchestration | Claude Code · Codex CLI · Gemini CLI**

Claude Code를 상위 감독자(Architect & Supervisor)로 두고, Codex CLI와 Gemini CLI에 코드 관련 작업을 위임하도록 설계한 초기 AI CLI 오케스트레이션 Skill입니다.

이 저장소는 당시의 설계와 도구 사용 지침을 보존하는 **초기 아카이브**입니다.

## Overview

이 프로젝트는 단일 LLM이 요구사항 분석, 코드 작성, 검토를 모두 수행하는 대신, **작업 감독과 코드 실행의 역할을 분리**하는 방식을 정의합니다.

Claude는 요구사항 분석, 작업 계획, 실행 도구 선택 및 결과 검토를 담당하고, Codex CLI와 Gemini CLI는 코드 생성, 분석, 리팩터링, 디버깅 등의 작업을 수행하도록 설계되었습니다.

## Architecture

```text
                  User
                   |
                   v
              Claude Code
         (Architect & Supervisor)
                   |
             Analyze & Plan
                   |
             Tool Selection
                   |
          +--------+--------+
          |                 |
          v                 v
      Codex CLI         Gemini CLI
          |                 |
          +--------+--------+
                   |
                   v
             Task Results
                   |
                   v
             Claude Review
                   |
              Report / Retry
```

### Agent Responsibilities

| Component | Responsibility |
|---|---|
| Claude Code | Architecture, planning, tool selection, supervision, result review |
| Codex CLI | Code generation, refactoring, analysis, multi-step execution |
| Gemini CLI | Code generation, large-codebase analysis, debugging, rapid iteration |

## Core Workflow

`ai-cli-tools` Skill은 다음 작업 흐름을 정의합니다.

1. **Check Override** — 사용자가 Claude의 직접 구현을 명시적으로 요청했는지 확인
2. **Assess & Plan** — 요구사항 분석 및 작업 분해
3. **Select Tool** — 작업 특성에 따른 Codex/Gemini 선택
4. **Formulate Instructions** — 실행 프롬프트, 제약조건 및 출력 요구사항 구성
5. **Execute & Delegate** — 선택한 CLI를 통한 작업 위임
6. **Review & Quality Check** — 생성 결과 검토
7. **Report** — 수행 내용 및 주요 결과 보고
8. **Iterate** — 필요시 재위임 또는 도구 변경

기본 정책은 코드 관련 작업을 외부 CLI에 위임하는 것입니다. 사용자가 명시적으로 요청한 경우에만 Claude가 직접 코드를 작성하도록 예외를 정의했습니다.

## Tool Selection

| Use Case | Preferred Tool |
|---|---|
| 복잡한 추론 및 다단계 작업 | Codex CLI |
| 샌드박스 권한 및 실행 환경 제어 | Codex CLI |
| 세션 재개를 활용한 반복 작업 | Codex CLI |
| 대규모 코드베이스 분석 | Gemini CLI |
| 빠른 반복 작업 및 프로토타이핑 | Gemini CLI |
| 대화형 디버깅 | Gemini CLI |

도구별 선택 기준은 당시 Skill 문서에 정의된 정책이며, 당시 구독제 계정 한도에 따른 배분이었지 테스트나 다른 정보를 통한 배분은 아니었습니다.

## Documentation

초기 Skill은 다음 문서로 구성됩니다.

| Document | Description |
|---|---|
| `SKILL.md` | Claude의 감독자 역할, 위임 정책, 표준 작업 흐름 |
| `references/codex.md` | Codex CLI 모델, 샌드박스, 실행 명령 및 세션 재개 지침 |
| `references/gemini.md` | Gemini CLI 모델, 실행 모드, 디렉터리 범위 설정 및 인증 지침 |

문서에는 당시 사용한 모델명, CLI 명령어, 옵션 및 작업 예시가 포함되어 있습니다.

## Scope & Limitations

이 아카이브의 중심은 **Skill 문서에 정의된 AI 작업 위임 및 감독 절차**입니다.

- Claude의 역할과 외부 CLI 위임 정책을 문서로 정의
- Codex/Gemini의 선택 기준과 사용 예시 제공
- 생성 결과의 검토 및 반복 수정 절차 명시
- 외부 CLI의 실제 실행 성공이나 검토 수행을 독립적으로 보장하는 시스템은 이 문서만으로 확인할 수 없음

당시 CLI 버전과 모델을 기준으로 작성된 자료이므로 현재 환경에서 명령어의 정상 동작을 보장하지 않습니다.

## Archive Notice

**이 저장소는 초기 오케스트레이션 설계를 보존하기 위한 Historical Archive입니다.**

최신 AI 개발 프레임워크로 유지보수하는 저장소가 아니며, 당시 작성된 Skill과 참조 문서의 내용을 기록하는 데 목적이 있습니다.

초기 오케스트레이션 실험부터 문서 가드닝 체계의 도입, 현재 Codeguide에 이르기까지의 기술적 발전 과정과 주요 설계 의사결정은 **[Codeguide History](https://github.com/gmkoo-d3v/codeguide-history)**에서 별도로 다룹니다.

---

**Status:** Archived  
**Category:** AI CLI Orchestration / Skill-based Delegation  
**Tools:** Claude Code, Codex CLI, Gemini CLI  
**Purpose:** Preservation of the original workflow design
