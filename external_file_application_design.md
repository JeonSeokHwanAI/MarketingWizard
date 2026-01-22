# External File Application Design

## 개요
두 개의 사용자 제공 `.md` 파일(Persona, Writing Rules)을 file dialog로 불러오고, UI에 내용을 표시하며 Step 5 prompt 생성에 적용합니다.

## 목표
- 사용자가 로컬 Markdown 파일 2개를 선택 가능:
  - Persona file
  - Writing Rules file
- 로드된 내용을 각 selector 옆 UI에 표시
- `.md` 파일만 선택 가능
- Step 5 prompt 생성 시 두 파일의 내용을 적용
- **이 단계에서는 코드 변경 없음** (design only)

## 제외 범위
- Markdown content 파싱/구조화
- Markdown semantics 검증
- 파일을 서버로 업로드

## UI 변경 (Step 5)
이 섹션은 **Step 5의 실행 버튼** 바로 위에 배치합니다:
- 대상 버튼: `self.create_action_button(left_frame, "🚀 마케팅 캡틴: 최종 완성본 출력", lambda: self.run_gemini(self.prompt_step5, self.txt_out5, "final_script"), "dark")`

**"External File Application"** 섹션을 추가하고 2개의 row로 구성합니다:

1) **Persona File**
- Text area (multiline): file content 표시
- "Load File" button: file dialog 열기

2) **Writing Rules File**
- Text area (multiline): file content 표시
- "Load File" button: file dialog 열기

### Layout
- 2 rows, 각 row 구성:
  - Left: label + multiline textbox
  - Right: button
- `.md` file filter만 사용

## File Loading Behavior
- file dialog filter: `*.md`
- UTF-8로 파일 읽기
- 해당 textbox에 content 표시
- Step 5에서 사용할 수 있도록 raw content를 메모리에 저장

## Data Model
`self.data`에 2개 필드 추가:
- `persona_md` (string)
- `writing_rules_md` (string)

초기값은 empty string.

## Step 5 Prompt 적용
### 현재 persona 사용
- `persona_style = self.data.get("persona_style", "Friendly")`

### 추가 제안
Step 5 prompt에 두 섹션을 주입:

1) **External Persona Profile**
- `persona_md`가 있으면 사용
- 예시 블록:
  - `# External Persona (from file)`
  - `<persona_md>`

2) **External Writing Rules**
- `writing_rules_md`가 있으면 사용
- 예시 블록:
  - `# External Writing Rules (from file)`
  - `<writing_rules_md>`

### Rule priority
- 파일이 로드되면 기존 prompt rule을 override 또는 extend.
- 권장 지시문:
  - "Follow these external rules strictly if provided; otherwise follow default rules."

## Error Handling
- dialog 취소 시: 변경 없음
- file read 실패 시: warning 표시
- content가 empty이면: 미제공으로 처리

## File Filters
- `filetypes=[("Markdown Files", "*.md")]`
- `defaultextension=".md"`

## Implementation Notes (Future)
- helper 추가: `load_md_file(target_textbox, data_key)`
- textbox는 preview 전용으로 editable 설정(옵션: read-only)

## Acceptance Criteria
- UI에서 `.md` 파일 2개 로드 가능
- 로드된 content가 textbox에 표시
- Step 5 prompt가 content를 포함
- `.txt` 등 다른 file type은 선택 불가

