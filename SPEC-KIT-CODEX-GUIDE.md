# Spec Kit + Codex 워크플로 가이드

## 1. 사전 준비
- **uv 설치**: 아직 `uv`를 설치하지 않았다면 <https://github.com/astral-sh/uv#installation> 안내에 따라 설치합니다.
- **Python 3.11+ 확보**: `uvx` 실행 시 Python 3.11 이상이 필요합니다.
- **Codex 확장 로그인**: VS Code Codex 확장에 로그인되어 있어야 합니다.
- **CODEX_HOME 설정**: PowerShell 새 창에서 아래 명령을 한 번 실행하고 VS Code를 재시작합니다.
  ```powershell
  setx CODEX_HOME "C:\Users\hojun\Desktop\Directory\Etc\SuperCode\.codex"
  ```
  > 경로는 각 프로젝트에 맞게 조정하세요. `setx`는 새 세션부터 적용되므로 명령 실행 후 VS Code/터미널을 모두 닫았다 다시 열어야 합니다.

## 2. 프로젝트 한 번에 초기화하기 (Option 2)
> 설치 없이 한 프로젝트를 부트스트랩할 때 사용하는 방법입니다.

1. 작업할 루트 디렉터리로 이동합니다.
   ```powershell
   cd <작업할_디렉터리>
   ```
2. `specify init`을 한 번만 실행합니다.
   ```powershell
   uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --ai codex --script ps
   ```
   - 현재 디렉터리에서 바로 초기화하려면 `<PROJECT_NAME>` 대신 `.` 또는 `--here` 조합을 사용합니다.
   - 명령이 끝나면 `.codex/`, `.specify/`, `scripts/`, `templates/` 등이 생성되어 Codex용 템플릿과 스크립트를 제공합니다.
3. 필요 시 `.codex/`를 `.gitignore`에 추가해 자격 증명 유출을 방지합니다.

## 3. Codex와 템플릿 수동 적용 순서
Codex 확장은 슬래시 명령을 직접 실행하지 않으므로 템플릿 파일을 채팅에 붙여 넣어야 합니다.

### 3.1 공통 작업 방식
1. VS Code에서 `.codex/prompts/<command>.md` 파일을 엽니다.
2. `Ctrl+Shift+P` → `Codex: Add to Codex chat`을 실행하거나, 내용을 복사해서 Codex 채팅 입력창에 직접 붙여 넣습니다.
3. “이 템플릿에 맞춰 산출물을 작성해 줘”처럼 지시하고, Codex가 묻는 추가 정보를 대화로 제공합니다.
4. Codex가 작성한 결과물을 지정된 경로에 저장합니다(필요하면 새 파일 생성).

### 3.2 실행 순서와 산출물 저장 위치
1. **/constitution** → 프로젝트 헌장 작성
   - 템플릿: `.codex/prompts/constitution.md`
   - 저장: `.specify/memory/constitution.md`
2. **/specify** → 기능/제품 명세 작성
   - 템플릿: `.codex/prompts/specify.md`
   - 권장 경로: `specs/<feature>/spec.md`
3. **(선택) /clarify** → 명세 보완용 질문/답변 기록
   - 템플릿: `.codex/prompts/clarify.md`
   - 저장: `specs/<feature>/clarifications.md`
4. **/plan** → 기술 구현 계획 수립
   - 템플릿: `.codex/prompts/plan.md`
   - `scripts/powershell/setup-plan.ps1 -Json`을 실행해 자동화할 수도 있습니다.
   - 저장: `plans/<feature>/plan.md`
5. **/tasks** → 실행 가능한 태스크 목록 생성
   - 템플릿: `.codex/prompts/tasks.md`
   - 저장: `tasks/<feature>/tasks.md`
6. **(선택) /analyze** → 명세/계획/태스크 간 일관성 검토
   - 템플릿: `.codex/prompts/analyze.md`
   - 저장: `reviews/<feature>/analysis.md`
7. **/implement** → 구현 단계 지침 생성(필요 시 코드 샘플 포함)
   - 템플릿: `.codex/prompts/implement.md`
   - 저장: `implementation/<feature>/implement.md` 또는 해당 코드 브랜치에 직접 적용

> `specs/`, `plans/`, `tasks/`, `reviews/`, `implementation/` 디렉터리가 없다면 미리 만들어 두세요. 기능 단위로 `<feature>` 이름을 통일하면 추적이 쉽습니다.

## 4. AGENTS.md에 추가할 권장 지침
프로젝트마다 다음 내용을 `AGENTS.md` 상단에 추가하면 Codex가 항상 같은 구조를 참조하게 됩니다.

```markdown
## Codex 프로젝트 지침
- 모든 산출물 기준 경로: <프로젝트_루트>
- 구조
  - `.specify/memory/constitution.md`: 최신 프로젝트 헌장
  - `specs/<feature>/spec.md`: 기능 명세
  - `specs/<feature>/clarifications.md`: 명세 보충 Q&A (있을 경우)
  - `plans/<feature>/plan.md`: 구현 계획
  - `tasks/<feature>/tasks.md`: 실행 태스크 목록
  - `reviews/<feature>/analysis.md`: /analyze 결과(선택)
  - `implementation/<feature>/implement.md`: 구현 지침/결과 요약
  - `notes/<feature>/*.md`, `checklists/<feature>/*.md`: 추가 메모·점검표
- Codex는 항상 이 문서를 먼저 읽고 위 경로만 사용해 문서를 작성하거나 갱신한다.
- 자동화 스크립트가 필요하면 `scripts/powershell/*.ps1` 또는 `scripts/bash/*.sh`를 사용하며, 실행 전 필요한 입력과 의존성을 명시한다.
```

## 5. 체크리스트 & 보조 스크립트 운용
- `scripts/powershell/`와 `scripts/bash/` 폴더에는 헬퍼 스크립트가 포함되어 있습니다.
  - PowerShell 예시: `pwsh .\scripts\powershell\setup-plan.ps1 -Json`
  - Bash 예시(WSL/Unix 환경): `bash ./scripts/bash/setup-plan.sh --json`
- 스크립트를 실행하기 전에 필요 런타임(예: PowerShell 7, git)이 설치되어 있는지 확인하세요.
- 체크리스트나 메모는 `checklists/`, `notes/` 하위에 기능별로 관리하고, Codex에게 업데이트 상황을 간단히 알려주면 최신 상태를 추적하기 쉽습니다.

## 6. 워크플로 마무리 체크
- 산출물을 모두 생성/갱신했는지 확인합니다.
- `.codex/` 경로에 민감 정보가 있는지 점검하고 필요한 부분만 커밋합니다.
- `AGENTS.md`를 최신 상태로 유지해 이후 협업 시에도 동일한 구조를 따르도록 합니다.
