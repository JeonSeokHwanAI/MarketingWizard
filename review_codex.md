# marketing_wizard.py 분석

## 개요
- Tkinter + ttkbootstrap 기반의 GUI 앱으로, 5단계 마케팅 스토리/블로그 글 생성 워크플로를 제공합니다.
- Google Gemini API(`google-genai` SDK)를 사용해 텍스트/이미지 생성 기능을 호출합니다.
- 결과는 탭별로 저장/복사/파일 저장 기능을 제공하며, 이미지 생성 결과는 PIL로 렌더링합니다.

## 구조 요약
- `MarketingWizardApp`
  - UI 구성: `create_widgets`, `create_step_tab`, 단계별 `build_stepX_ui`
  - 상태 저장: `self.data`에 persona/strategy 및 생성 결과 저장
  - API Key 관리: `config.json` 읽기/쓰기 (`load_config`, `save_config`)
  - Gemini 호출: `run_gemini` (텍스트), `run_image_gen` (이미지)
  - 기타: 타입라이터 애니메이션, 클립보드 복사, 파일 저장 다이얼로그

## 동작 흐름 (핵심)
- Step 1: 제품/고객 pain 입력 → 텍스트 생성 + 이미지 생성(인물 사진)
- Step 2: 캐릭터 설정(역할/결함/배경) + 페르소나 스타일 선택 → 텍스트 생성
- Step 3: 4부작 시놉시스(표 형식) 생성 + 포스터 이미지 생성
- Step 4: 개별 에피소드 본문 초안 생성
- Step 5: 최종 블로그 글 + 섹션별 이미지 프롬프트 → 프롬프트 추출 후 이미지 생성

## 주의/리스크
- **문자 인코딩 문제**: 다수의 문자열이 깨져 보입니다(예: "留덉???罹≫떞"). UTF-8 소스가 아닌 인코딩으로 저장된 파일을 UTF-8로 읽은 흔적으로 보입니다. UI 텍스트/라벨이 의도대로 표시되지 않을 수 있습니다.
- **GUI 응답성**: 텍스트 출력은 별도 스레드로 처리하지만, 이미지 생성 결과를 `ImageTk.PhotoImage`로 매핑할 때 UI 스레드 안전성은 `root.after`로 일부 보완되어 있습니다. 다만 긴 작업 시 사용자 체감 대기는 있을 수 있습니다.
- **Gemini 모델명**: `gemini-2.5-flash` / `gemini-2.5-flash-image` 사용 중. 실제 사용 가능 모델명은 계정/지역/정책에 따라 다를 수 있어 API 호출 오류 가능성이 있습니다.
- **Step 5 이미지 프롬프트 추출**: `**[Image Prompt for Nano Banana]**:` 마커 기준으로 regex 추출합니다. 모델 출력이 포맷을 지키지 않으면 이미지 생성이 누락됩니다.
- **config.json 저장 위치**: 실행 디렉터리 기준 상대 경로입니다. 실행 위치에 따라 다른 경로에 저장될 수 있습니다.

## 외부 의존성 (별도 설치 필요)
아래 패키지가 설치되어 있어야 합니다.

- ttkbootstrap
- google-genai
- pillow

추가로 Python 표준 라이브러리 외:
- tkinter (Windows 기본 포함; 미설치 환경에서는 별도 설치 필요)

## 설치 예시 (터미널)
- `pip install ttkbootstrap google-genai pillow`

## 실행 방법
- Python에서 `marketing_wizard.py` 실행
- 최초 실행 시 Settings 탭에서 Gemini API Key 입력 후 저장

## 개선 제안(선택)
- 깨진 문자열을 UTF-8로 정상화(소스 파일 저장 인코딩 확인)
- 모델명/엔드포인트 유효성 체크 및 오류 메시지 개선
- `config.json` 경로를 사용자 홈/앱 전용 디렉터리로 고정
- Step 5 이미지 프롬프트 포맷 검증 로직 강화
