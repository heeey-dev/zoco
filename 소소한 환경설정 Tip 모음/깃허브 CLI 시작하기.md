# 깃허브 CLI 시작하기

Windows에서 GitHub CLI를 직접 다운로드하고, PowerShell에서 GitHub 계정에 로그인하는 방법을 정리한 문서입니다.

## 1. GitHub CLI 다운로드

GitHub CLI 공식 최신 릴리스 페이지에 접속합니다.

- <https://github.com/cli/cli/releases/latest>

페이지 아래쪽의 **Assets**를 펼친 후, 일반적인 64비트 Windows PC에서는 다음과 같은 이름의 파일을 다운로드합니다.

```text
gh_버전_windows_amd64.msi
```

예시:

```text
gh_2.97.0_windows_amd64.msi
```

다운로드한 `.msi` 파일을 더블클릭하여 설치합니다. 설치가 끝나면 기존 PowerShell 창을 완전히 닫고 새로 엽니다.

> `winget`이 정상 작동하는 경우에는 `winget install --id GitHub.cli --source winget` 명령으로도 설치할 수 있습니다.

## 2. 설치 확인

PowerShell에서 다음 명령을 실행합니다.

```powershell
gh --version
```

정상적으로 설치되었다면 다음처럼 버전 정보가 표시됩니다.

```text
gh version 2.97.0 (2026-07-31)
https://github.com/cli/cli/releases/tag/v2.97.0
```

## 3. GitHub 계정 로그인

PowerShell에서 다음 명령을 실행합니다.

```powershell
gh auth login
```

질문이 나오면 다음과 같이 선택합니다. 위·아래 방향키로 항목을 선택하고 `Enter`를 누릅니다.

```text
? Where do you use GitHub? GitHub.com
? What is your preferred protocol for Git operations on this host? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Login with a web browser
```

선택 항목을 한국어로 풀면 다음과 같습니다.

1. GitHub 사용 위치: `GitHub.com`
2. Git 작업 연결 방식: `HTTPS`
3. GitHub 계정으로 Git 인증: `Yes`
4. GitHub CLI 인증 방법: `Login with a web browser`

## 4. 브라우저에서 인증 완료

1. PowerShell 화면에 표시되는 일회용 코드를 복사합니다.
2. 안내에 따라 `Enter`를 눌러 브라우저를 엽니다.
3. GitHub 계정에 로그인합니다.
4. 일회용 코드를 입력합니다.
5. GitHub CLI 접근을 승인합니다.

## 5. 로그인 상태 확인

브라우저 인증을 마쳤으면 PowerShell에서 다음 명령을 실행합니다.

```powershell
gh auth status
```

출력에 다음과 비슷한 내용이 보이면 로그인 성공입니다.

```text
Logged in to github.com
```

## 6. 현재 프로젝트를 새 GitHub 저장소로 올리기

먼저 프로젝트 폴더로 이동합니다.

```powershell
cd "프로젝트 폴더 경로"
```

Git 저장소가 아직 아니라면 초기화하고 첫 커밋을 만듭니다.

```powershell
git init
git add .
git commit -m "Initial commit"
git branch -M main
```

비공개 GitHub 저장소를 만들고 현재 프로젝트를 바로 올립니다.

```powershell
gh repo create 저장소이름 --private --source=. --remote=origin --push
```

공개 저장소로 만들려면 `--private` 대신 `--public`을 사용합니다.

```powershell
gh repo create 저장소이름 --public --source=. --remote=origin --push
```

저장소 웹페이지 열기:

```powershell
gh repo view --web
```

## 7. 이후 변경사항 올리기

파일을 수정한 뒤에는 다음 명령을 반복해서 사용합니다.

```powershell
git add .
git commit -m "변경 내용 설명"
git push
```

## 참고

- `gh`는 GitHub 로그인, 저장소 생성, Pull Request 등 GitHub 관련 작업을 담당합니다.
- `git`은 파일 변경 이력 관리와 커밋·푸시를 담당합니다.
- `.env`, 비밀번호, API 키 등의 민감한 파일은 GitHub에 올리지 않습니다.
- 일반 Git 저장소에서는 100 MiB를 초과하는 단일 파일을 바로 푸시할 수 없습니다.
