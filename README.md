# MarketingWizard

마케팅 캡틴(Marketing Captain)은 블로그/스피치 콘텐츠를 단계별로 기획하고 작성하도록 돕는 데스크톱 앱입니다.
Google Gemini 또는 Claude 모델을 선택하여 콘텐츠와 프롬프트를 생성할 수 있습니다.

## 주요 기능
- Step 1~5 워크플로 기반 콘텐츠 생성
- Persona / Writing Rules `.md` 파일 적용
- 결과물 Markdown(.md) 저장
- 이미지 프롬프트 텍스트 출력 및 복사
- Google/Claude API 선택 지원

## 설치
```bash
pip install ttkbootstrap google-genai pillow anthropic
```

## 실행
```bash
python marketing_wizard_Persona_Rule_API.py
```

## 설정
- 설정 탭에서 **Google** 또는 **Claude**를 선택
- 각 API Key 입력 후 저장
- Claude 모델: `claude-sonnet-4-20250514`

## 사용 방법 (요약)
1. Step 1~4에서 필요한 입력을 진행
2. Step 5에서 Persona/Writing Rules 파일을 선택(선택 사항)
3. 최종 생성 버튼으로 결과 출력
4. 복사 또는 저장 버튼으로 결과 활용

## 파일 구조 (핵심)
- `marketing_wizard_Persona_Rule_API.py` : Google/Claude 선택 포함 메인 앱
- `marketing_wizard_Persona_Rule.py` : Persona/Rules 파일 적용 버전
- `marketing_wizard_markdown.py` : Markdown 저장 중심 버전
- `2026-01-22_codex_change.md` : 변경 이력
- `RELEASE_NOTES_v0.1.0.md` : 릴리스 노트

## 주의 사항
- `config.json`은 API Key 저장용이며 `.gitignore`에 제외됩니다.
- API Key는 외부에 공개하지 마세요.

## 라이선스
별도 라이선스가 없다면 내부/개인 용도로만 사용하세요.
