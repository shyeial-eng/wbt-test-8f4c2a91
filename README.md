# 월드 비하인드 트래커 0.2.2

Marinara Engine 2.4.4의 공식 capability-package API 1.14를 사용하는 Roleplay 전용 트래커입니다.

- 약 두 번의 대화마다 장면 밖 진행과 생활형 세계 정보를 생성합니다.
- 활성 Lorebook과 최근 RP 문맥을 읽되 Lorebook을 수정하지 않습니다.
- RP HUD 버튼과 Tracker Panel에 최신 결과를 표시합니다.
- HUD/패널 열람은 저장된 결과만 읽으므로 추가 AI 호출이 없습니다.
- 수동 갱신은 Marinara가 제공하는 트래커 재실행 동작을 사용하므로 AI 호출이 발생합니다.

## 권한

- `agent-runtime`: 결과 검증과 채팅별 저장
- `chat-read`: 요청한 채팅이 Roleplay인지 확인
- `routes`: UI가 최신 결과를 읽는 전용 경로
- `storage`: 채팅별 최신 결과 보관
- `ui`: HUD와 Tracker Panel 표시

네트워크 권한, 채팅 쓰기, Lorebook 쓰기, Full page access는 사용하지 않습니다.

## 호환성

- 확인 대상: Marinara Engine 2.4.4
- Capability API: 1.14
- 설치 후 서버 재시작 필요

현재 패키지는 Marinara Engine 2.4.4에서 인간 테스트를 진행한 친구 공유용 시험판입니다. 넓은 커뮤니티 공개 전에는 추가 회귀 테스트가 필요합니다.

## 설치 방법

Marinara Engine 2.4.4 설치 폴더의 `.env` 파일에 아래 두 줄을 넣습니다.

```env
ENABLE_EXTERNAL_EXTENSIONS=true
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

Marinara를 다시 열고 에이전트 다운로드 화면에서 **월드 비하인드 트래커**를 설치한 뒤 서버를 한 번 재시작합니다. 사용할 RP 채팅방의 에이전트 목록에도 이 트래커를 추가하고 활성화해야 합니다.

## 0.2.2 변경 사항

- 한국어 출력이 중간에 잘려 `WORLD` 블록이 누락되는 문제를 줄이기 위해 최대 출력 한도를 4096 토큰으로 높였습니다.
- 실제 출력이 짧게 끝나면 4096 토큰을 모두 사용하지 않습니다.

## 0.2.1 변경 사항

- Marinara Engine 2.4.4의 엄격한 에이전트 정의 검사에서 거부되던 중복 설정 항목을 제거했습니다.
- 사전 생성 주입을 포함하는 동작은 허용된 `defaultSettings` 안의 설정으로 그대로 유지됩니다.
