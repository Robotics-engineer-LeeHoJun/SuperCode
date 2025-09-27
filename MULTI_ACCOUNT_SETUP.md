# Windows에서 GitHub 다중 계정 사용 가이드

## 1. 상황 요약
- 현재 저장소는 개인 계정 `Robotics-engineer-LeeHoJun` 소유.
- Git Credential Manager(GCM)가 회사 계정 `HanyangRobot` 자격 증명을 저장해 두어, `git push` 시 GitHub가 403(권한 없음)으로 응답.
- 목표는 개인/회사 두 계정을 모두 사용하며 VS Code + GitLens, GitKraken에서 계정 충돌 없이 작업하는 것.

---

## 2. 잘못 저장된 자격 증명 지우기
GCM은 URL을 표준 입력으로 받아 처리합니다. PowerShell에서 아래 명령으로 `github.com` 항목을 삭제하세요.

```powershell
@"protocol=https
host=github.com

"@ | git credential-manager erase
```

> 위 명령이 성공하면 아무 출력 없이 종료됩니다. GUI를 선호하면 **제어판 → 사용자 계정 → 자격 증명 관리자 → Windows 자격 증명**에서 GitHub 항목을 찾아 삭제해도 됩니다.

---

## 3. HTTPS 기반 다중 계정 설정(추천)
### 3.1 기본 Git 설정 정리
```powershell
# 글로벌 기본값은 개인 계정으로
git config --global user.name "Lee Ho Jun"
git config --global user.email "dlghwns0201@gmail.com"

# 저장소별로 다른 계정을 사용할 때 URL 전체를 키로 삼도록 설정
git config --global credential.useHttpPath true
```

### 3.2 저장소별 사용자 정보 지정
회사 저장소에서는 로컬 설정을 덮어씁니다.
```powershell
# 회사 저장소 루트에서 실행
git config --local user.name "Lee Ho Jun"
git config --local user.email "work@company.com"
```

### 3.3 PAT(토큰) 발급 및 저장
1. 각 GitHub 계정에서 **Settings → Developer settings → Personal access tokens → Tokens(classic)** 경로로 들어가 `repo` 권한을 가진 토큰을 발급합니다.
2. 토큰은 계정마다 따로 보관(예: 1Password, KeePass 등).
3. `git push`를 실행하면 GCM이 새 자격 증명을 묻습니다. 사용자명에 GitHub 아이디, 비밀번호 대신 PAT를 입력하세요.
4. `credential.useHttpPath` 설정 덕분에 저장소마다 다른 자격 증명이 저장되어 서로 덮어쓰지 않습니다.

### 3.4 필요 시 수동 저장
프롬프트를 기다리지 않고 저장하려면 아래 예시처럼 수행합니다.
```powershell
@"protocol=https
host=github.com
path=Robotics-engineer-LeeHoJun/SuperCode.git
username=Robotics-engineer-LeeHoJun
password=<PERSONAL_PAT>

"@ | git credential-manager store
```
(회사 저장소도 동일 형식으로 `path`와 `username`, `password`만 바꿔서 저장하세요.)

---

## 4. SSH 기반 병행 사용(선호 시)
1. 두 개의 키를 생성합니다.
   ```powershell
   ssh-keygen -t ed25519 -f "$Env:USERPROFILE\.ssh\id_ed25519_personal" -C "personal@example.com"
   ssh-keygen -t ed25519 -f "$Env:USERPROFILE\.ssh\id_ed25519_work" -C "work@company.com"
   ```
2. `~/.ssh/config`에 호스트 별칭을 만듭니다.
   ```text
   Host github-personal
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_ed25519_personal

   Host github-work
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_ed25519_work
   ```
3. GitHub 각 계정의 **Settings → SSH and GPG keys**에 공개키(`.pub`)를 등록합니다.
4. 저장소마다 원격 URL을 별칭으로 변경합니다.
   ```powershell
   git remote set-url origin github-personal:Robotics-engineer-LeeHoJun/SuperCode.git
   # 회사 저장소에서는 github-work:조직/저장소.git
   ```
SSH 기반일 때는 PAT 입력이 필요 없고, 키만 올바르면 자동으로 인증됩니다.

---

## 5. VS Code, GitLens, GitKraken에서의 계정 분리 팁
### 5.1 VS Code
- 좌측 하단 계정 아이콘(또는 `Ctrl+Shift+P → Accounts: Manage Trusted Extensions`)에서 **GitHub** 계정을 추가/제거합니다.
- 개인·회사 계정을 모두 추가 가능하며, 저장소별로 어떤 계정으로 Sync를 할지 선택할 수 있습니다.
- `settings.json`에서 계정별 Git 프로필을 만들고 싶다면
  ```json
  "git.defaultCloneDirectory": "C:/Users/hojun/Desktop/Directory/Etc",
  "git.openRepositoryInParentFolders": "always"
  ```
  등 기본 경로만 감시해 두고, 실질적인 계정 전환은 Git 설정(위 3~4항)으로 처리합니다.

### 5.2 GitLens
- `Ctrl+Shift+P → GitLens: Sign In to GitHub` 실행 후 개인 계정으로 로그인합니다.
- 추가 계정 연결은 `GitLens: Integrations → Manage Authentication Sessions`에서 가능합니다. 필요 시 하나는 Personal, 하나는 Work로 Naming하여 구분하세요.
- GitLens는 Git이 반환하는 author 정보를 그대로 사용하므로, 저장소별 `user.name`/`user.email` 설정이 맞다면 충돌이 발생하지 않습니다.

### 5.3 GitKraken
- 좌측 상단 프로필 아이콘 → **Preferences → Authentication**으로 이동해 Personal/Work GitHub 계정을 각각 Connect 합니다.
- 저장소별로 어느 계정을 사용할지 **Preferences → Profiles**에서 정리하거나, 열린 저장소의 **Remote** 설정을 확인해 올바른 URL(HTTPS/SSH)이 지정되어 있는지 확인합니다.
- GitKraken은 내부적으로도 GCM 또는 자체 저장소에 자격 증명을 저장하므로, 문제가 생기면 `File → Preferences → Authentication → Clear`로 초기화 후 다시 연결할 수 있습니다.

---

## 6. 트러블슈팅 체크리스트
- `git remote -v`로 항상 올바른 원격 URL을 확인합니다.
- `git config --show-origin user.name`으로 적용 중인 사용자 정보의 출처(Global/Local)를 확인합니다.
- 동일한 저장소에서 두 계정을 번갈아 사용할 필요가 있다면, 브랜치를 분리하기보다 **별도의 클론**을 만드는 것이 안전합니다. (`SuperCode-personal`, `SuperCode-work` 폴더처럼.)
- 인증 문제 발생 시 우선 `git credential-manager erase`로 초기화한 뒤 다시 로그인하면 대부분 해결됩니다.

---

## 7. 다음 단계
1. 위 방법으로 자격 증명을 정리한 뒤 `git push`가 정상 동작하는지 확인합니다.
2. VS Code / GitLens / GitKraken에서 각각 올바른 계정으로 로그인했는지 검증합니다.
3. 필요 시 SSH 방식으로 전환해 비밀번호 입력 부담을 제거합니다.
