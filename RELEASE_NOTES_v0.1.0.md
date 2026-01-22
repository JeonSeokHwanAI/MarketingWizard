# 릴리스 노트 - v0.1.0

제목: 초기 릴리스
날짜: 2026-01-22

## 주요 변경사항
- Google/Claude API 선택 및 Claude 모델 지원(claude-sonnet-4-20250514) 추가
- Step 5에 Persona/Writing Rules 파일 입력 및 외부 rules 주입 기능 추가
- UI 레이아웃, 크기 조정, 프롬프트 표시 동작 개선

## 마케팅 사용 안내
- Step 5에서 Persona/Writing Rules 파일을 불러오면, 해당 내용이 최종 프롬프트에 반영됩니다.
- Persona/Writing Rules 파일이 없으면 기본 규칙을 사용합니다.
- 결과물은 Markdown(.md)으로 저장 가능합니다.

## 비고
- Claude 사용 시 `anthropic` 라이브러리가 필요합니다.
- `config.json` 등 민감 정보는 .gitignore로 제외됩니다.
