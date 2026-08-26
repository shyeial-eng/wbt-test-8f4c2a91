# 🌍 월드 비하인드 트래커

Marinara Engine의 Roleplay 채팅을 위한 장면 밖 진행·생활형 세계 정보 트래커입니다.

- 확장 버전: **0.2.3**
- 지원 확인 버전: **Marinara Engine 2.4.4**
- Capability API: **1.14**
- 현재 상태: 친구 공유용 시험판

> 이 설명은 프론트엔드나 서버를 처음 다루는 사람도 따라 할 수 있도록 작성했습니다. 한 단계씩 천천히 진행하세요.

## 어떤 기능인가요?

- 약 두 번의 대화마다 장면 밖 인물과 집단의 진행 상황을 만듭니다.
- 현재 장면에서 자연스럽게 드러나는 생활형 세계 정보를 하나 기록합니다.
- 활성 Lorebook과 최근 RP 내용을 참고하지만 Lorebook을 수정하지 않습니다.
- 최신 결과를 RP 상단 버튼과 Tracker Panel에서 볼 수 있습니다.
- 저장된 결과를 열어보는 것만으로는 AI가 다시 호출되지 않습니다.
- 수동 갱신이나 자동 갱신이 실제로 실행될 때는 AI 사용량이 발생합니다.
- 최대 출력 한도는 4096토큰입니다. 출력이 짧게 끝나면 4096토큰을 전부 사용하지 않습니다.

## 설치 전에 꼭 읽어주세요

1. 이 시험판은 **Marinara Engine 2.4.4**에서 확인했습니다.
2. Marinara 서버를 직접 관리할 수 있는 사람만 설치할 수 있습니다.
3. 다른 사람이 운영하는 서버에 웹브라우저로 접속만 하는 사용자는 직접 설치할 수 없습니다. 서버 관리자에게 요청해야 합니다.
4. 가능하면 설치 전에 Marinara 백업을 만들어 주세요.
5. `.env`는 Marinara의 설정 파일입니다. 채팅과 캐릭터가 들어 있는 파일은 아니지만, 잘못 편집하면 서버가 정상적으로 열리지 않을 수 있습니다.
6. **설치 후 `.env` 파일 전체를 삭제하면 안 됩니다. 안내된 카탈로그 주소 한 줄만 삭제합니다.**
7. **PM2로 Marinara를 실행하는 서버에서는 Marinara 화면의 서버 재시작 버튼을 누르지 마세요.** PM2와 앱 내부 재시작이 동시에 실행되면 중복 서버와 재시작 반복이 발생할 수 있습니다.

### 🚨 PM2 서버 필수 경고

Vultr 서버에서 PM2를 사용한다면 설치, 업데이트, `.env` 변경 뒤의 모든 재시작은 SSH에서 PM2로만 진행하세요.

먼저 PM2가 Marinara를 관리하는지 확인합니다.

```bash
pm2 list
```

목록에 `marinara`가 있다면 다음 명령을 사용합니다.

```bash
pm2 restart marinara
pm2 status
```

PM2 목록의 이름이 `marinara`가 아니라면 실제 표시된 이름을 사용해야 합니다. 확실하지 않으면 재시작하지 말고 서버 관리자에게 확인하세요.

PM2 사용자는 다음 행동을 하면 안 됩니다.

- Marinara의 **Settings → Advanced → Server Restart** 버튼 누르기
- PM2가 실행 중인데 별도로 `./start.sh` 다시 실행하기
- 같은 서버를 여러 터미널에서 중복 실행하기
- 원인을 모르는 상태에서 `pm2 restart all` 사용하기

재시작 후 `pm2 status`의 재시작 횟수가 빠르게 계속 증가하면 정상 상태가 아닙니다. 같은 재시작 명령을 반복하지 말고 즉시 중단한 뒤 서버 관리자에게 도움을 요청하세요.

## 가장 먼저: 내 설치 방식 확인하기

아래 중 자신에게 맞는 안내 하나만 따라가세요.

- Windows PC에서 `MarinaraLauncher.exe` 또는 `start.bat`로 실행한다 → [Windows 설치](#windows-pc-설치)
- Vultr 서버에 SSH로 접속하고 `start.sh`로 실행한다 → [Vultr 일반 설치](#vultr-일반-설치-ssh--startsh)
- Vultr 서버에서 PM2를 사용한다 → [PM2로 안전하게 재시작](#pm2로-안전하게-재시작하기)
- Vultr 서버에서 Docker Compose를 사용한다 → [Vultr Docker 설치](#vultr-docker-설치-ssh--docker-compose)

`SSR`이 아니라 **SSH**입니다. SSH는 내 컴퓨터에서 Vultr 서버의 명령 화면에 안전하게 접속하는 방법입니다.

---

## Windows PC 설치

### 1. Marinara 폴더 찾기

`MarinaraLauncher.exe`, `start.bat`, `package.json`이 들어 있는 Marinara Engine 폴더를 엽니다.

### 2. 숨김 파일 표시하기

`.env`는 이름이 점으로 시작해서 보이지 않을 수 있습니다. Windows 파일 탐색기에서 **보기 → 표시 → 숨긴 항목**을 켜세요.

### 3. `.env` 백업하기

`.env`를 복사한 뒤 같은 폴더에 붙여넣고, 복사본 이름을 다음처럼 바꿉니다.

```text
.env.wbt-backup
```

### 4. `.env` 편집하기

`.env`를 메모장으로 열고 맨 아래에 다음 한 줄을 추가합니다.

```env
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

주소에 대괄호, 괄호, 따옴표를 붙이지 마세요.

올바른 형태:

```env
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

잘못된 형태:

```text
MARINARA_AGENT_CATALOG_URL=[https://주소](https://주소)
```

저장한 뒤 Marinara를 완전히 종료하고 다시 실행합니다. 이제 [앱에서 트래커 설치하기](#앱에서-트래커-설치하기)로 이동하세요.

---

## Vultr 일반 설치: SSH + `start.sh`

이 부분은 Docker를 사용하지 않고 GitHub에서 Marinara Engine을 내려받아 `./start.sh`로 실행하는 서버용입니다.

### 1. SSH로 Vultr 서버에 접속하기

Vultr에서 서버의 IP 주소와 로그인 정보를 확인합니다. Windows에서는 PowerShell이나 Windows Terminal을 열고 다음과 같이 입력합니다.

```bash
ssh root@서버_IP주소
```

`서버_IP주소` 부분은 자신의 Vultr 서버 IP로 바꿔야 합니다. 처음 접속할 때 계속할 것인지 묻는 문장이 나오면 주소가 자신의 서버가 맞는지 확인한 뒤 `yes`를 입력합니다. 비밀번호를 입력할 때 화면에 글자나 별표가 나타나지 않아도 정상입니다.

> SSH 비밀번호, API 키, `.env` 내용 전체를 다른 사람에게 보내거나 GitHub에 올리지 마세요.

### 2. Marinara 설치 폴더로 이동하기

서버마다 설치 위치가 다르므로 자신이 Marinara를 설치했던 폴더로 이동해야 합니다. 흔한 예시는 다음과 같습니다.

```bash
cd ~/Marinara-Engine
```

현재 폴더의 파일을 확인합니다.

```bash
ls -la
```

목록에 `package.json`, `start.sh`, `.env`, `.env.example`이 보이면 올바른 폴더일 가능성이 높습니다. 보이지 않는다면 무작정 진행하지 말고 Marinara를 설치한 폴더부터 확인하세요.

### 3. `.env` 백업하기

다음 명령은 기존 `.env`를 `.env.wbt-backup`이라는 복구용 파일로 복사합니다. 채팅이나 캐릭터 데이터는 변경하지 않습니다.

```bash
cp -i .env .env.wbt-backup
```

이미 같은 이름의 백업이 있어서 덮어쓸지 묻는다면, 확실하지 않을 때는 `n`을 입력해 중단하세요.

### 4. `.env` 열기

초보자가 사용하기 쉬운 `nano` 편집기로 파일을 엽니다.

```bash
nano .env
```

`nano: command not found`가 나오면 서버 관리자에게 nano 설치를 요청하거나, 자신이 사용하는 서버 관리 패널의 파일 편집기를 이용하세요.

### 5. 카탈로그 주소 한 줄 추가하고 저장하기

방향키로 파일 맨 아래로 이동한 다음 아래 한 줄을 그대로 추가합니다.

```env
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

저장 방법:

1. `Ctrl` 키를 누른 상태에서 `O`를 누릅니다.
2. 파일 이름이 `.env`인지 확인하고 `Enter`를 누릅니다.
3. `Ctrl` 키를 누른 상태에서 `X`를 눌러 종료합니다.

### 6. Marinara 다시 시작하기

먼저 PM2 사용 여부를 확인합니다.

```bash
pm2 list
```

목록에 Marinara가 있다면 `./start.sh`를 실행하지 말고 아래의 [PM2로 안전하게 재시작하기](#pm2로-안전하게-재시작하기)를 따르세요.

PM2를 사용하지 않고 현재 터미널에서 `start.sh`로 Marinara를 실행 중인 경우에만 `Ctrl+C`로 안전하게 중지한 뒤 다음 명령으로 다시 실행합니다.

```bash
./start.sh
```

`systemd`나 서버 관리 패널로 실행하고 있다면 해당 관리 방법으로 Marinara만 재시작하세요. 실행 방식을 모르는 상태에서 서버 전체를 재부팅하거나 다른 서비스를 중지하지 마세요.

이제 [앱에서 트래커 설치하기](#앱에서-트래커-설치하기)로 이동하세요.

---

## PM2로 안전하게 재시작하기

이 부분은 `pm2 list`에 Marinara가 표시되는 Vultr 서버용입니다.

### 1. 현재 상태 확인

```bash
pm2 list
```

Marinara 프로세스의 이름과 현재 상태를 확인합니다. 아래 예시는 프로세스 이름이 `marinara`인 경우입니다.

### 2. PM2에서 한 번만 재시작

```bash
pm2 restart marinara
```

### 3. 정상 상태 확인

```bash
pm2 status
```

`marinara`가 `online`으로 표시되어야 합니다. 재시작 횟수가 짧은 시간 동안 계속 증가하거나 상태가 `errored`와 `restarting`을 반복하면 추가 재시작을 멈추세요.

> PM2 서버에서는 Marinara 화면의 서버 재시작 버튼을 누르지 마세요. 앱 내부 재시작과 PM2 재시작이 충돌하면 같은 포트를 차지하려는 중복 서버가 생길 수 있습니다.

---

## Vultr Docker 설치: SSH + Docker Compose

이 부분은 Marinara를 `docker compose up -d`로 실행한 서버용입니다.

공식 Docker 설치의 실제 `.env`는 컨테이너 안의 다음 위치에 있습니다.

```text
/app/data/.env
```

프로젝트 폴더에 보이는 다른 `.env`와 혼동하지 마세요.

### 1. SSH로 접속하고 Docker Compose 폴더로 이동하기

SSH 접속 후 `docker-compose.yml`이 있는 Marinara 폴더로 이동합니다.

```bash
cd ~/Marinara-Engine
```

파일과 실행 상태를 확인합니다.

```bash
ls -la
docker compose ps
```

목록에 `docker-compose.yml`과 `marinara` 서비스가 보이지 않는다면 다른 폴더일 수 있으므로 중단하고 설치 위치를 확인하세요.

### 2. 컨테이너의 `.env`를 서버 폴더로 복사하기

다음 명령은 컨테이너 안의 설정 파일을 현재 폴더의 `marinara.env.edit`로 복사합니다. 원본을 바로 수정하지 않기 때문에 실수를 줄일 수 있습니다.

```bash
docker compose cp marinara:/app/data/.env ./marinara.env.edit
```

복사한 파일을 한 번 더 백업합니다.

```bash
cp -i marinara.env.edit marinara.env.wbt-backup
```

### 3. 복사한 설정 파일 편집하기

```bash
nano marinara.env.edit
```

맨 아래에 다음 한 줄을 추가합니다.

```env
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

`Ctrl+O`, `Enter`, `Ctrl+X` 순서로 저장하고 종료합니다.

### 4. 편집한 파일을 컨테이너에 적용하기

아래 명령은 편집한 파일을 Marinara의 실제 설정 위치로 복사합니다.

```bash
docker compose cp ./marinara.env.edit marinara:/app/data/.env
```

그다음 Marinara 컨테이너만 재시작합니다.

```bash
docker compose restart marinara
```

이제 [앱에서 트래커 설치하기](#앱에서-트래커-설치하기)로 이동하세요.

---

## 앱에서 트래커 설치하기

1. Marinara에서 **Agents** 메뉴를 엽니다.
2. **Download Agents**를 누릅니다.
3. 목록에서 **월드 비하인드 트래커 0.2.3**을 찾습니다.
4. 설명, 버전, 권한을 확인합니다.
5. **Install**을 누릅니다.
6. 설치 완료 메시지가 나타나는지 확인합니다.
7. 설치가 끝나면 자신의 실행 방식으로 Marinara 서버를 한 번 재시작합니다.

### PM2 사용자는 여기서 잠깐 멈추세요

PM2 사용자는 설치 완료 직후 앱의 재시작 버튼을 누르지 마세요. 아직 `pm2 restart`도 실행하지 말고 다음 순서로 진행하면 재시작을 한 번으로 줄일 수 있습니다.

1. SSH에서 `.env`를 엽니다.
2. `MARINARA_AGENT_CATALOG_URL=`로 시작하는 한 줄만 삭제하고 저장합니다.
3. `pm2 restart marinara`를 한 번만 실행합니다.
4. `pm2 status`에서 `online`인지 확인합니다.

이 한 번의 PM2 재시작으로 새 트래커 활성화와 공식 카탈로그 복귀가 함께 적용됩니다.

재시작 방법:

- Windows: Marinara를 완전히 종료한 뒤 다시 실행
- 일반 `start.sh`: 실행 중인 터미널에서 `Ctrl+C` 후 `./start.sh`
- PM2: SSH에서 `pm2 restart marinara`
- Docker Compose: SSH에서 `docker compose restart marinara`

**PM2 사용자는 설치 완료 화면이나 Settings의 서버 재시작 버튼을 누르지 마세요.**

설치가 끝나면 바로 RP를 시작하기 전에 다음 정리 단계까지 진행하세요.

### 별도의 개인용 에이전트 토글이 필요한가요?

필요하지 않습니다. **Settings → Advanced → Danger Zone**의 개인용 또는 커스텀 에이전트 가져오기 허용 토글은 켜지 않아도 됩니다.

이 토글은 일반 에이전트 JSON 파일이나 커스텀 에이전트 저장소를 가져올 때 사용하는 기능입니다. 월드 비하인드 트래커는 **Agents → Download Agents**의 패키지 설치 방식을 사용합니다.

`ENABLE_EXTERNAL_EXTENSIONS=true` 역시 이 트래커 패키지 설치만을 위한 필수 설정은 아닙니다. 다른 확장 때문에 이미 `.env`에 들어 있다면 그대로 유지해도 됩니다.

---

## 설치 후 공식 카탈로그로 되돌리기

외부 카탈로그 주소를 계속 유지하면 **Download Agents** 화면이 이 시험용 저장소를 계속 바라볼 수 있습니다. 설치가 성공한 뒤 주소 한 줄을 제거하면 공식 다운로드 목록으로 돌아갑니다.

> **중요: `.env` 파일을 삭제하지 마세요. `MARINARA_AGENT_CATALOG_URL=...` 한 줄만 삭제합니다.**

다음 줄은 삭제합니다.

```env
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

`ENABLE_EXTERNAL_EXTENSIONS=true`가 이미 들어 있다면 이 트래커 때문에 지우거나 바꿀 필요는 없습니다. 다른 확장에서 사용할 수 있으므로 그대로 두세요.

### Windows에서 삭제하기

1. Marinara 폴더의 `.env`를 메모장으로 엽니다.
2. `MARINARA_AGENT_CATALOG_URL=`로 시작하는 한 줄만 삭제합니다.
3. 저장하고 Marinara를 재시작합니다.

### Vultr 일반 설치에서 삭제하기

SSH로 접속해 Marinara 폴더로 이동한 뒤 엽니다.

```bash
cd ~/Marinara-Engine
nano .env
```

`MARINARA_AGENT_CATALOG_URL=`로 시작하는 한 줄 전체를 지웁니다. `Ctrl+O`, `Enter`, `Ctrl+X`로 저장합니다.

- PM2가 아니라면 원래 사용하던 방식으로 Marinara를 재시작합니다.
- PM2를 사용한다면 앱의 재시작 버튼 대신 SSH에서 다음 명령을 사용합니다.

```bash
pm2 restart marinara
pm2 status
```

### Vultr Docker 설치에서 삭제하기

앞에서 만든 `marinara.env.edit`를 다시 엽니다.

```bash
cd ~/Marinara-Engine
nano marinara.env.edit
```

`MARINARA_AGENT_CATALOG_URL=`로 시작하는 한 줄 전체를 지운 뒤 저장합니다. 편집한 파일을 다시 적용하고 Marinara 컨테이너만 재시작합니다.

```bash
docker compose cp ./marinara.env.edit marinara:/app/data/.env
docker compose restart marinara
```

주소를 제거해도 이미 설치된 월드 비하인드 트래커는 삭제되지 않습니다. 다만 새 버전으로 업데이트할 때는 카탈로그 주소를 다시 추가해야 합니다.

---

## RP 채팅방에서 활성화하기

패키지를 설치하는 것과 채팅방에서 사용하는 것은 별도 단계입니다.

1. 사용할 **Roleplay 채팅방**을 엽니다.
2. 채팅 설정의 **Agents** 항목을 엽니다.
3. **Enable Agents**가 켜져 있는지 확인합니다.
4. **월드 비하인드 트래커**를 이 채팅방의 에이전트 목록에 추가하고 활성화합니다.
5. RP를 진행합니다.

UI가 보이더라도 해당 채팅방에 에이전트가 추가되지 않았다면 자동 갱신되지 않을 수 있습니다. 정상적으로 설정되면 약 두 번의 대화마다 새 결과가 생성됩니다.

---

## 업데이트 방법

새 버전이 공개되면 `.env`에 다음 주소를 다시 추가합니다.

```env
MARINARA_AGENT_CATALOG_URL=https://raw.githubusercontent.com/shyeial-eng/wbt-test-8f4c2a91/main/catalog/v2/catalog.json
```

자신의 실행 방식으로 Marinara를 재시작한 뒤 **Agents → Download Agents**에서 업데이트합니다. 업데이트가 끝나면 같은 줄을 다시 제거하고 같은 방식으로 재시작하면 공식 카탈로그로 돌아갑니다. PM2 서버에서는 앱의 재시작 버튼이 아니라 `pm2 restart marinara`를 사용하세요.

## 문제가 생겼을 때

### Download Agents에 트래커가 보이지 않아요

- `.env`의 주소에 대괄호나 괄호가 들어가지 않았는지 확인하세요.
- 주소가 한 줄로 입력되어 있는지 확인하세요.
- Marinara Engine 버전이 2.4.4인지 확인하세요.
- 서버가 GitHub의 `raw.githubusercontent.com`에 접속할 수 있어야 합니다.
- `.env` 변경 후 자신의 실행 방식으로 Marinara를 재시작해 보세요. PM2 서버에서는 `pm2 restart marinara`만 사용하세요.

### 설치했지만 UI가 보이지 않아요

- 설치 후 서버를 재시작했는지 확인하세요.
- 현재 채팅이 Roleplay 모드인지 확인하세요.
- 해당 채팅방의 Agents 목록에 월드 비하인드 트래커를 추가했는지 확인하세요.

### UI는 보이지만 자동 갱신되지 않아요

- 해당 채팅방의 **Enable Agents**가 켜져 있는지 확인하세요.
- 월드 비하인드 트래커가 해당 채팅방에서 활성화되어 있는지 확인하세요.
- 에이전트가 사용할 AI 연결과 모델이 정상인지 확인하세요.
- 자동 실행 간격이 있으므로 RP를 조금 더 진행한 뒤 확인하세요.

### PM2 재시작 횟수가 계속 증가해요

- 정상 상태가 아니므로 Marinara 화면의 재시작 버튼과 추가 재시작 명령 사용을 멈추세요.
- 같은 포트에서 PM2 외부의 Marinara가 중복 실행 중일 수 있습니다.
- `pm2 status` 화면을 서버 관리자에게 보여주고 중복 프로세스와 포트 점유 상태를 점검받으세요.
- 에이전트 파일이나 채팅 데이터를 먼저 삭제하지 마세요. 이 현상은 에이전트 연산보다 프로세스 관리 충돌일 가능성이 큽니다.

### `.env`를 잘못 편집했어요

당황해서 다른 파일을 삭제하지 마세요. 먼저 Marinara를 중지한 뒤 설치 전에 만든 백업을 확인하세요.

- 일반 설치 백업: `.env.wbt-backup`
- Docker 안내에서 만든 백업: `marinara.env.wbt-backup`

백업 복구가 필요하면 현재 `.env`와 백업의 차이를 먼저 확인하거나 서버 관리자에게 도움을 요청하세요. 확실하지 않은 상태에서 백업을 덮어쓰지 마세요.

## 권한과 개인정보

이 패키지는 다음 권한을 요청합니다.

- `agent-runtime`: 에이전트 결과 검사와 저장
- `chat-read`: 현재 RP 문맥 확인
- `routes`: 최신 저장 결과를 UI로 전달
- `storage`: 채팅별 최신 결과 저장
- `ui`: RP HUD 및 Tracker Panel 표시

네트워크 권한, 채팅 쓰기, Lorebook 쓰기, Full Page Access는 사용하지 않습니다. 실제 RP 내용, API 키, 비밀번호를 GitHub 저장소나 문제 제보 화면에 그대로 올리지 마세요.

## 변경 내역

### 0.2.3

- UI가 열려 있는 동안 2.5초마다 서버를 조회하던 반복 호출을 완전히 제거했습니다.
- 채팅방 진입, 채팅 UI 상태 변경 후, 수동 재실행 완료 후, 사용자가 트래커 창을 열 때만 최신 저장 결과를 확인합니다.
- 같은 채팅방에서 여러 트래커 UI가 동시에 요청하면 하나의 요청을 공유하고, 5초 동안 같은 결과를 재사용합니다.
- 일시적인 조회 실패가 발생해도 마지막 정상 결과와 트래커 버튼을 유지합니다.

### 0.2.2

- 한국어 출력이 중간에 잘려 `WORLD` 블록이 누락되는 문제를 줄이기 위해 최대 출력 한도를 4096토큰으로 높였습니다.
- 실제 출력이 짧게 끝나면 4096토큰을 모두 사용하지 않습니다.

### 0.2.1

- Marinara Engine 2.4.4의 엄격한 에이전트 정의 검사에서 거부되던 중복 설정 항목을 제거했습니다.
- 사전 생성 주입을 포함하는 동작은 허용된 `defaultSettings` 안의 설정으로 유지했습니다.
