# Docker CI/CD Pipeline

Docker와 GitHub Actions를 활용하여 애플리케이션의 빌드부터 Docker Hub 이미지 배포, 원격 서버 배포까지 자동화한 CI/CD 프로젝트입니다.

## 1. Project Overview

소스 코드 변경 및 Git Push를 시작으로 GitHub Actions에서 Docker 이미지를 자동으로 빌드하고 테스트한 후 Docker Hub에 Push합니다.

이후 원격 서버(`devops-lab`)에 SSH로 접속하여 최신 Docker 이미지를 Pull하고 기존 컨테이너를 재배포합니다.

## 2. Architecture

```text
Developer
    |
    | git push
    v
GitHub
    |
    v
GitHub Actions
    |
    +-- Checkout
    |
    +-- Docker Build
    |
    +-- Container Test
    |
    +-- Docker Hub Push
    |
    v
devops-lab
    |
    +-- docker pull
    +-- docker stop
    +-- docker rm
    +-- docker run
```

## 3. Environment

| Category        | Technology     |
| --------------- | -------------- |
| Container       | Docker         |
| CI/CD           | GitHub Actions |
| Image Registry  | Docker Hub     |
| Web Server      | Nginx          |
| Deployment      | SSH            |
| Server          | Rocky Linux    |
| Version Control | Git / GitHub   |

## 4. Project Structure

```text
my-docker-cicd
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── app/
│   ├── Dockerfile
│   └── index.html
├── .gitignore
└── README.md
```

## 5. CI/CD Process

### 5.1 Source Push

```bash
git push origin main
```

GitHub의 `main` 브랜치에 코드가 Push되면 GitHub Actions Workflow가 실행됩니다.

### 5.2 Docker Image Build

GitHub Actions에서 `app/Dockerfile`을 기반으로 Docker 이미지를 생성합니다.

```bash
docker build
```

생성된 이미지는 Commit SHA와 `latest` 태그를 사용하여 관리합니다.

```text
dydcjsrjaror/my-devops-app:<commit SHA>
dydcjsrjaror/my-devops-app:latest
```

### 5.3 Docker Container Test

빌드된 이미지를 임시 컨테이너로 실행한 후 HTTP 요청을 통해 애플리케이션의 정상 동작 여부를 확인합니다.

테스트가 정상적으로 완료된 경우에만 Docker Hub Push 단계가 실행됩니다.

### 5.4 Docker Hub Push

Container Test가 완료되면 Docker Image를 Docker Hub에 Push합니다.

Commit SHA 태그를 사용하여 특정 Commit에서 생성된 Image를 식별할 수 있도록 구성했습니다.

### Docker Hub



> <img width="1839" height="839" alt="image" src="https://github.com/user-attachments/assets/be31e338-70c5-4efb-a657-74a5c76b614b" />


### 5.5 Remote Server Deployment

GitHub Actions에서 SSH를 이용하여 `devops-lab` 서버에 접속합니다.

원격 서버에서 다음 작업을 자동으로 수행합니다.

```bash
docker pull
docker stop
docker rm
docker run
```

Docker Hub에서 새로운 이미지를 Pull한 후 기존 컨테이너를 종료하고 새로운 이미지로 컨테이너를 재배포합니다.

### Remote Server


> <img width="1336" height="48" alt="image" src="https://github.com/user-attachments/assets/677d5f0a-9513-4f56-8548-5cc2f1d76005" />



### 5.6 GitHub Actions 실행 결과

전체 CI/CD Pipeline은 GitHub Actions Workflow를 통해 자동으로 실행됩니다.

```text
Checkout
    |
Docker Build
    |
Container Test
    |
Docker Hub Push
    |
Remote Server Deployment
```

### GitHub Actions


> <img width="794" height="833" alt="image" src="https://github.com/user-attachments/assets/ed855f6b-7a6e-48c5-b80f-02b25bfc431b" />





### 5.7 Application Deployment Result

배포가 완료된 후 원격 서버의 Nginx 애플리케이션에 HTTP 요청하여 정상적으로 서비스되는 것을 확인했습니다.

### Application


> <img width="525" height="393" alt="image" src="https://github.com/user-attachments/assets/73a6ddf7-a76d-49db-a386-c4499358ca24" />


## 6. GitHub Secrets

서버 및 Docker Hub 인증 정보는 GitHub Actions Secrets를 사용하여 관리합니다.

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
SERVER_HOST
SERVER_USER
SERVER_SSH_KEY
```

민감한 인증정보를 Workflow 파일에 직접 작성하지 않고 GitHub Secrets를 통해 전달하도록 구성했습니다.

## 7. Project Goals

* Docker 컨테이너 기반 애플리케이션 배포
* GitHub Actions를 활용한 CI/CD 자동화
* Docker 이미지 빌드 및 Registry Push 자동화
* Container Test를 통한 이미지 정상 동작 검증
* SSH 기반 원격 서버 배포 자동화
* Commit SHA 기반 이미지 버전 관리

## 8. What I Learned

* Dockerfile 작성 및 Docker Image Build
* Docker Container 실행 및 HTTP 기반 테스트
* GitHub Actions Workflow 구성
* GitHub Secrets를 활용한 인증정보 관리
* Docker Hub Image 및 Tag 관리
* SSH 기반 원격 서버 배포
* Commit SHA 기반 Docker Image Version 관리
* CI/CD Pipeline 구성 및 장애 대응

## Project Screenshots

주요 구현 결과를 다음 화면을 통해 확인할 수 있습니다.

* GitHub Actions Workflow 실행 결과
* Docker Hub Image 및 Tag
* 원격 서버 Docker Container
* 배포된 Nginx Application
