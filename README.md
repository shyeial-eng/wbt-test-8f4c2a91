# 월드 비하인드 트래커 0.2.0

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

현재 패키지는 인간 테스트 전 개발 빌드입니다. 실사용 환경이나 커뮤니티에 배포하지 마세요.
