# 내 컴퓨터에 개발자용 작업실 꾸미기

## 1. 프로젝트 개요

이 프로젝트는 터미널, Docker, Git/GitHub를 활용하여 재현 가능한 로컬 개발 환경을 구성하는 것을 목표로 한다.

터미널을 이용하여 작업 디렉토리와 파일 권한을 관리하고, OrbStack과 Docker를 이용하여 컨테이너 기반 웹 서버를 실행했다. 또한 포트 매핑, 바인드 마운트, Docker 볼륨을 직접 검증하고 전체 수행 과정과 결과를 GitHub 저장소에 기록했다.

## 2. 실행 환경

- OS: macOS
- Shell: zsh
- Terminal: macOS Terminal
- Container Runtime: OrbStack
- Docker: 28.5.2
- Git: 2.53.0

## 3. 작업공간 생성

과제 전용 디렉토리를 생성하고 Git 저장소를 초기화했다.

```bash
mkdir -p ~/codyssey
cd ~/codyssey
mkdir codyssey-E1-1-workstation
cd codyssey-E1-1-workstation
git init
git branch -M main
```

현재 작업 경로:

```text
/Users/heoyulan/codyssey/codyssey-E1-1-workstation
```

### 수행 결과

![작업공간 생성 결과](evidence/terminal/workspace-created.png)

## 4. 터미널 기본 조작

터미널에서 디렉토리와 파일을 생성하고 파일의 복사, 이동, 이름 변경 및 삭제 작업을 수행했다.

### 사용한 명령어

```bash
mkdir -p terminal-practice/source
cd terminal-practice
touch source/sample.txt
echo "Codyssey terminal practice" > source/sample.txt
cat source/sample.txt
cp source/sample.txt source/sample-copy.txt
mv source/sample-copy.txt source/renamed.txt
rm source/renamed.txt
mkdir delete-test
rmdir delete-test
ls -la
```

### 명령어 설명

- `pwd`: 현재 작업 경로를 확인한다.
- `mkdir`: 디렉토리를 생성한다.
- `cd`: 디렉토리를 이동한다.
- `touch`: 빈 파일을 생성한다.
- `echo`: 문자열을 출력하거나 파일에 저장한다.
- `cat`: 파일의 내용을 확인한다.
- `cp`: 파일을 복사한다.
- `mv`: 파일을 이동하거나 이름을 변경한다.
- `rm`: 파일을 삭제한다.
- `rmdir`: 비어 있는 디렉토리를 삭제한다.
- `ls -la`: 숨김 파일과 권한을 포함한 목록을 확인한다.

### 수행 결과

![터미널 기본 조작 결과](evidence/terminal/terminal-basic-operations.png)

## 5. 파일 및 디렉토리 권한

파일과 디렉토리의 권한을 확인하고 `chmod` 명령으로 권한을 변경했다.

### 실행 명령어

```bash
mkdir -p permission-practice/test-directory
touch permission-practice/test-file.sh

chmod 700 permission-practice/test-directory
chmod 600 permission-practice/test-file.sh

ls -ld permission-practice/test-directory
ls -l permission-practice/test-file.sh

chmod 755 permission-practice/test-directory
chmod 644 permission-practice/test-file.sh

ls -ld permission-practice/test-directory
ls -l permission-practice/test-file.sh
```

### 권한 설명

- `r`: 읽기 권한
- `w`: 쓰기 권한
- `x`: 실행 권한 또는 디렉토리 접근 권한

숫자 권한은 다음 값을 더하여 표현한다.

- `r = 4`
- `w = 2`
- `x = 1`

따라서 다음과 같이 해석할 수 있다.

- `755` = `rwxr-xr-x`
- `644` = `rw-r--r--`
- `700` = `rwx------`
- `600` = `rw-------`

### 수행 결과

![파일 및 디렉토리 권한 변경 결과](evidence/permissions/permission-changed.png)

## 6. Docker 설치 및 기본 점검

OrbStack을 실행하고 Docker CLI 경로를 설정한 뒤 Docker 엔진의 동작을 확인했다.

### Docker 버전 확인

```bash
docker --version
which docker
```

확인 결과:

```text
Docker version 28.5.2, build ecc6942
/Users/heo.uran5175/.orbstack/bin/docker
```

### hello-world 실행

```bash
docker run --name hello-codyssey hello-world
docker ps -a
docker images
docker logs hello-codyssey
```

`docker run --name hello-codyssey hello-world` 명령은 로컬에 `hello-world` 이미지가 있는지 먼저 확인한다.

로컬에 이미지가 없으면 Docker Hub에서 이미지를 내려받고, 해당 이미지로 `hello-codyssey` 컨테이너를 생성하여 실행한다.

`hello-world` 컨테이너는 안내 메시지를 출력한 후 종료되므로 `Exited (0)` 상태가 정상이다.

### 이미지와 컨테이너의 차이

- 이미지는 컨테이너 실행에 필요한 파일과 설정을 담은 템플릿이다.
- 컨테이너는 이미지를 기반으로 생성되어 실제 실행되는 인스턴스이다.
- 하나의 이미지로 여러 개의 컨테이너를 생성할 수 있다.

### 수행 결과

![Docker 기본 점검 결과](evidence/docker/docker-basic-check.png)

## 7. Ubuntu 컨테이너 실습

Ubuntu 이미지를 이용하여 대화형 컨테이너를 생성하고 내부 Linux 환경을 확인했다.

### 컨테이너 실행

```bash
docker run -it --name ubuntu-practice ubuntu bash
```

옵션의 의미는 다음과 같다.

- `docker run`: 컨테이너를 생성하고 실행한다.
- `-it`: 터미널 입력을 받을 수 있는 대화형 모드로 실행한다.
- `--name ubuntu-practice`: 컨테이너 이름을 지정한다.
- `ubuntu`: 사용할 이미지이다.
- `bash`: 컨테이너 내부에서 실행할 프로그램이다.

### 컨테이너 내부 확인

```bash
pwd
ls -la
cat /etc/os-release
whoami
echo "inside ubuntu container"
```

`whoami` 결과로 `root`가 출력되었고, `/etc/os-release`를 통해 Ubuntu 환경임을 확인했다.

컨테이너 내부에서 다음 명령으로 종료했다.

```bash
exit
```

`bash`가 컨테이너의 주 프로세스이므로 `exit`로 해당 프로세스가 종료되면 컨테이너도 중지된다.

중지된 컨테이너는 다음 명령으로 다시 실행할 수 있다.

```bash
docker start -ai ubuntu-practice
```

### 수행 결과

![Ubuntu 컨테이너 내부](evidence/docker/ubuntu-container-inside.png)

![Ubuntu 컨테이너 상태](evidence/docker/ubuntu-container-status.png)

## 8. 웹페이지와 Dockerfile 작성

Nginx 정적 웹서버를 사용하기 위해 `site/index.html`과 `Dockerfile`을 작성했다.

### 프로젝트 파일

```text
Dockerfile
site/index.html
```

### Dockerfile

```dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="codyssey-workstation"

ENV APP_ENV=dev

COPY site/ /usr/share/nginx/html/

EXPOSE 80
```

### Dockerfile 명령 설명

- `FROM nginx:alpine`: Nginx가 포함된 가벼운 Alpine 기반 이미지를 사용한다.
- `LABEL`: 이미지의 설명용 메타정보를 추가한다.
- `ENV APP_ENV=dev`: 컨테이너에서 사용할 환경변수를 설정한다.
- `COPY`: 로컬 `site` 폴더를 이미지 내부의 Nginx 웹 루트로 복사한다.
- `EXPOSE 80`: 컨테이너가 80번 포트를 사용한다는 정보를 기록한다.

`EXPOSE 80`만으로 호스트에서 접속할 수 있는 것은 아니다. 실제 포트 연결은 컨테이너 실행 시 `-p` 옵션으로 설정한다.

## 9. 커스텀 Docker 이미지 빌드

작성한 Dockerfile을 이용하여 Nginx 기반 커스텀 이미지를 생성했다.

### 빌드 명령어

```bash
docker build -t codyssey-web:1.0 .
```

- `docker build`: Dockerfile을 이용해 이미지를 생성한다.
- `-t codyssey-web:1.0`: 이미지 이름을 `codyssey-web`, 태그를 `1.0`으로 지정한다.
- `.`: 현재 디렉토리를 빌드 컨텍스트로 사용한다.

이미지 생성 여부는 다음 명령으로 확인했다.

```bash
docker images
```

### 수행 결과

![커스텀 Docker 이미지 빌드](evidence/docker/docker-image-build.png)

## 10. 포트 매핑과 웹 접속

커스텀 이미지를 이용하여 두 개의 Nginx 컨테이너를 실행했다.

### 8080 포트 실행

```bash
docker run -d \
  --name codyssey-web-8080 \
  -p 8080:80 \
  codyssey-web:1.0
```

### 8081 포트 실행

```bash
docker run -d \
  --name codyssey-web-8081 \
  -p 8081:80 \
  codyssey-web:1.0
```

`-p 8080:80`은 호스트 macOS의 8080 포트를 컨테이너 내부 Nginx의 80 포트에 연결한다는 뜻이다.

컨테이너는 격리된 네트워크에서 실행되므로 외부 브라우저에서 접근하려면 호스트 포트와 컨테이너 포트를 연결해야 한다.

동일한 `codyssey-web:1.0` 이미지로 이름과 호스트 포트가 서로 다른 두 개의 컨테이너를 실행할 수 있음을 확인했다.

### 터미널 확인

![포트 매핑 터미널 결과](evidence/port/port-mapping-terminal.png)

### 브라우저 접속

![8080 포트 접속](evidence/port/port-8080-browser.png)

![8081 포트 접속](evidence/port/port-8081-browser.png)

## 11. 바인드 마운트 변경 반영

호스트의 `site` 디렉토리를 Nginx 컨테이너의 웹 루트에 바인드 마운트했다.

### 실행 명령어

```bash
docker run -d \
  --name codyssey-bind \
  -p 8082:80 \
  -v "$(pwd)/site:/usr/share/nginx/html:ro" \
  nginx:alpine
```

`$(pwd)/site`는 호스트의 실제 디렉토리이며, `/usr/share/nginx/html`은 컨테이너 내부 Nginx의 웹 루트이다.

호스트의 `site/index.html`을 `Version 1`에서 `Version 2`로 수정했다. Docker 이미지를 다시 빌드하거나 컨테이너를 재시작하지 않았지만 브라우저를 새로고침하자 변경 내용이 즉시 반영되었다.

### 변경 전

![바인드 마운트 변경 전](evidence/bind-mount/bind-mount-before.png)

### 변경 후

![바인드 마운트 변경 후](evidence/bind-mount/bind-mount-after.png)

### 터미널 확인

![바인드 마운트 터미널 결과](evidence/bind-mount/bind-mount-terminal.png)

## 12. Docker 볼륨과 데이터 영속성

Docker가 관리하는 `codyssey-data` 볼륨을 생성하고 컨테이너의 `/data` 디렉토리에 연결했다.

### 볼륨 생성

```bash
docker volume create codyssey-data
docker volume ls
```

### 첫 번째 컨테이너에 데이터 생성

```bash
docker run -d \
  --name volume-test-1 \
  -v codyssey-data:/data \
  ubuntu sleep infinity

docker exec volume-test-1 \
  bash -lc 'echo "Codyssey persistent data" > /data/hello.txt'

docker exec volume-test-1 cat /data/hello.txt
```

### 컨테이너 삭제 후 데이터 확인

```bash
docker rm -f volume-test-1

docker run -d \
  --name volume-test-2 \
  -v codyssey-data:/data \
  ubuntu sleep infinity

docker exec volume-test-2 cat /data/hello.txt
```

첫 번째 컨테이너를 삭제한 뒤 같은 볼륨을 새 컨테이너에 연결했으며, `/data/hello.txt`의 내용이 그대로 유지되는 것을 확인했다.

컨테이너와 Docker 볼륨의 생명주기는 서로 분리되어 있기 때문에 컨테이너를 삭제해도 볼륨을 삭제하지 않으면 데이터가 유지된다.

### 컨테이너 삭제 전

![볼륨 데이터 생성](evidence/volume/volume-before-container-delete.png)

### 컨테이너 삭제 후

![볼륨 데이터 영속성](evidence/volume/volume-after-container-delete.png)

### 바인드 마운트와 Docker 볼륨 비교

| 구분 | 바인드 마운트 | Docker 볼륨 |
|---|---|---|
| 저장 위치 | 사용자가 지정한 호스트 경로 | Docker가 관리하는 경로 |
| 주요 용도 | 개발 파일 즉시 반영 | 데이터 영속성 |
| 호스트 접근 | 직접 접근하기 쉬움 | Docker 명령으로 관리 |
| 이번 검증 | HTML 수정 즉시 반영 | 컨테이너 삭제 후 데이터 유지 |

## 13. Git 및 GitHub 연동

로컬 프로젝트를 Git 저장소로 관리하고 GitHub 원격 저장소와 연결했다.

### Git 저장소 초기화

```bash
git init
git branch -M main
```

### 파일 커밋

```bash
git add .
git commit -m "feat: complete developer workstation mission"
```

`git add`로 변경 파일을 스테이징하고 `git commit`으로 해당 시점의 변경 이력을 저장했다.

### GitHub 원격 저장소 연결

```bash
git remote add origin https://github.com/thoroki/codyssey-E1-1-workstation.git
git push -u origin main
```

`origin`이라는 이름으로 GitHub 원격 저장소를 등록하고 로컬 `main` 브랜치를 GitHub에 업로드했다.

### Git과 GitHub의 차이

- Git은 로컬 컴퓨터에서 파일 변경 이력을 관리하는 버전 관리 도구이다.
- GitHub는 Git 저장소를 원격으로 보관하고 공유 및 협업할 수 있는 서비스이다.


heo.uran5175@c5r1s6 codyssey-E1-1-workstation % git config --list
credential.helper=osxkeychain
user.name=thoroki
user.email=heo.uran@gmail.com
init.defaultbranch=main
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/thoroki/codyssey-E1-1-workstation.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main



### 수행 결과

![GitHub push 성공](evidence/github/git-push-success.png)

![GitHub 저장소](evidence/github/github-repository.png)

## 14. Docker 컨테이너 운영

실행 중인 컨테이너의 상태, 로그와 리소스 사용량을 확인하고 컨테이너를 중지한 뒤 다시 시작했다.

### 상태 확인

```bash
docker ps
docker ps -a
```

- `docker ps`: 실행 중인 컨테이너를 확인한다.
- `docker ps -a`: 종료된 컨테이너를 포함한 전체 컨테이너를 확인한다.

### 로그 및 리소스 확인

```bash
docker logs codyssey-web-8080
docker stats --no-stream
```

### 중지 및 재시작

```bash
docker stop codyssey-web-8080
docker ps -a
docker start codyssey-web-8080
docker ps
curl http://localhost:8080
```

컨테이너를 중지한 뒤 다시 시작했고 웹페이지가 정상적으로 응답하는 것을 확인했다.

### 수행 결과

![Docker 컨테이너 운영 결과](evidence/docker/docker-container-operations.png)

## 15. 트러블슈팅

### 15.1 OrbStack 실행 후 docker 명령을 찾지 못한 문제

#### 문제

OrbStack을 설치하고 실행했지만 다음 오류가 발생했다.

```text
zsh: command not found: docker
```

#### 원인

OrbStack의 Docker CLI 파일은 설치되어 있었지만 `~/.orbstack/bin` 경로가 zsh의 `PATH`에 포함되지 않은 상태였다.

#### 확인

```bash
ls -l ~/.orbstack/bin
~/.orbstack/bin/docker --version
```

전체 경로로 실행했을 때 Docker 버전이 정상적으로 출력되는 것을 확인했다.

#### 해결

```bash
echo 'export PATH="$HOME/.orbstack/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

#### 결과

```bash
docker --version
which docker
```

Docker 명령이 정상적으로 인식되고 OrbStack의 Docker CLI 경로가 출력되었다.

### 15.2 GitHub push 시 403 오류 발생

#### 문제

다음 명령으로 GitHub 원격 저장소에 push를 시도했다.

```bash
git push -u origin main
```

다음 오류가 발생했다.

```text
fatal: unable to access
'https://github.com/thoroki/codyssey-E1-1-workstation.git/':
The requested URL returned error: 403
```

#### 원인 가설

원격 저장소 주소는 정상적으로 등록되어 있었으므로 GitHub 인증에 사용한 Personal Access Token에 저장소 쓰기 권한이 없는 것으로 판단했다.

#### 확인

```bash
git remote -v
```

원격 주소가 대상 GitHub 저장소를 가리키는 것을 확인했다.

Fine-grained token 설정에서는 다음 상태였다.

- Resource owner: `thoroki`
- Repository access: `Only select repositories`
- 선택 저장소: `thoroki/codyssey-E1-1-workstation`
- Repository permissions: 미설정

#### 해결

Fine-grained token 화면에서 필요한 쓰기 권한을 설정할 수 없어 Personal Access Token classic을 생성했다.

기존 macOS 키체인에 저장된 GitHub 인증정보를 삭제했다.

```bash
printf "protocol=https\nhost=github.com\n\n" |
git credential-osxkeychain erase
```

그다음 새로운 토큰을 사용하여 push를 다시 실행했다.

```bash
git push -u origin main
```

#### 결과

GitHub 원격 저장소에 정상적으로 push되었으며 로컬 브랜치와 `origin/main`이 동기화되었다.

```text
main -> main
브랜치가 'origin/main'에 맞게 업데이트된 상태입니다.
커밋할 사항 없음, 작업 폴더 깨끗함
```

#### 정리

GitHub에서 HTTPS 방식으로 push할 때는 계정 비밀번호가 아니라 Personal Access Token을 사용해야 한다. 또한 해당 토큰에는 대상 저장소에 대한 쓰기 권한이 있어야 한다.



절대경로 상대경로 어떨때
* 터미널에서 기본 명령어로 폴더/파일 생성·이동·삭제를 수행한 흔적이 있는가?
*  파일 권한 변경 결과가 확인되는가?
*  docker --version이 출력되고, Docker가 동작 가능한 상태인가?
*  docker run hello-world가 정상 실행되는가?
*  이미지/컨테이너 목록 확인 및 정리 흔적이 있는가?
*  Dockerfile로 이미지 빌드가 가능한가?
*  매핑된 포트로 접속이 가능한가?
*  Docker 볼륨 데이터가 컨테이너 삭제 후에도 유지되는가?
*  Git 설정 및 GitHub 연동이 확인되는가?
* 터미널에서 기본 명령어로 폴더/파일 생성·이동·삭제를 수행한 흔적이 있는가?
*  파일 권한 변경 결과가 확인되는가?
*  docker --version이 출력되고, Docker가 동작 가능한 상태인가?
*  docker run hello-world가 정상 실행되는가?
*  이미지/컨테이너 목록 확인 및 정리 흔적이 있는가?
*  Dockerfile로 이미지 빌드가 가능한가?
*  매핑된 포트로 접속이 가능한가?
*  Docker 볼륨 데이터가 컨테이너 삭제 후에도 유지되는가?
*  Git 설정 및 GitHub 연동이 확인되는가?


동작 구조 설계
*  프로젝트 디렉토리 구조를 어떤 기준으로 구성했는지 설명할 수 있는가?
*  포트/볼륨 설정을 어떤 방식으로 재현 가능하게 정리했는지 설명할 수 있는가?

핵심 기술 원리 적용
*  이미지와 컨테이너의 차이를 “빌드/실행/변경” 관점에서 구분해 설명할 수 있는가?
*  컨테이너 내부 포트로 직접 접속할 수 없는 이유와 필요한 이유를 설명할 수 있는가?
*  절대 경로/상대 경로를 어떤 상황에서 선택하는지 설명할 수 있는가?
*  파일 권한 숫자 표기(예: 755, 644)가 어떤 규칙으로 결정되는지 설명할 수 있는가?

심층 인터뷰
*  “호스트 포트가 이미 사용 중”이라 포트 매핑이 실패한다면, 어떤 순서로 원인을 진단할지 설명할 수 있는가?
*  컨테이너 삭제 후 데이터가 사라진 경험이 있다면, 이를 방지하기 위한 대안을 설명할 수 있는가?
*  이 미션에서 가장 어려웠던 지점과, 해결 과정(가설 → 확인 → 조치)을 근거와 함께 설명할 수 있는가?

   