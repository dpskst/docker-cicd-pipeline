# Docker CI/CD Pipeline

Docker와 GitHub Actions를 활용하여 애플리케이션의 빌드부터 Docker Hub 이미지 배포, 서버 배포까지 자동화한 CI/CD 프로젝트입니다.

## 📌 Project Overview

소스 코드 변경 및 Git Push를 시작으로 GitHub Actions에서 Docker 이미지를 자동으로 빌드하고 테스트한 후 Docker Hub에 Push합니다.

이후 원격 서버(TEST69)에 SSH로 접속하여 최신 Docker 이미지를 Pull하고 컨테이너를 재배포합니다.

## 🏗️ Architecture

```text
Developer
    │
    │ git push
    ▼
GitHub
    │
    ▼
GitHub Actions
    │
    ├── Checkout
    │
    ├── Docker Build
    │
    ├── Container Test
    │
    ├── Docker Hub Push
    │
    ▼
TEST69
    │
    ├── docker pull
    ├── docker stop
    ├── docker rm
    └── docker run
```

## 🛠️ Tech Stack

| Category       | Technology     |
| -------------- | -------------- |
| Container      | Docker         |
| CI/CD          | GitHub Actions |
| Image Registry | Docker Hub     |
| Web Server     | Nginx          |
| Deployment     | SSH            |
| Server         | Rocky Linux    |

## 📂 Project Structure

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

## 🔄 CI/CD Process

### 1. Source Push

```text
git push origin main
```

GitHub의 `main` 브랜치에 코드가 Push되면 GitHub Actions Workflow가 실행됩니다.

### 2. Docker Image Build

```bash
docker build
```

`app/Dockerfile`을 기반으로 Docker 이미지를 생성합니다.

### 3. Docker Container Test

빌드된 이미지를 임시 컨테이너로 실행한 후 HTTP 요청을 통해 애플리케이션의 정상 동작 여부를 확인합니다.

### 4. Docker Hub Push

테스트가 정상적으로 완료되면 Docker 이미지를 Docker Hub에 Push합니다.

이미지는 다음과 같이 관리합니다.

```text
dydcjsrjaror/my-devops-app:<commit SHA>
dydcjsrjaror/my-devops-app:latest
```

Commit SHA 태그를 사용하여 특정 버전의 이미지를 식별할 수 있도록 구성했습니다.

### 5. TEST69 Deployment

GitHub Actions에서 SSH를 이용해 TEST69 서버에 접속합니다.

```bash
docker pull
docker stop
docker rm
docker run
```

과정을 자동화하여 새로운 Docker 이미지로 컨테이너를 재배포합니다.

## 🔐 GitHub Secrets

서버 및 Docker Hub 인증 정보는 GitHub Actions Secrets를 사용하여 관리합니다.

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
SERVER_HOST
SERVER_USER
SERVER_SSH_KEY
```

민감한 인증정보를 Workflow 파일에 직접 작성하지 않고 GitHub Secrets를 통해 전달하도록 구성했습니다.

## 🎯 Project Goals

* Docker 컨테이너 기반 애플리케이션 배포 경험
* GitHub Actions를 활용한 CI/CD 자동화
* Docker 이미지 빌드 및 Registry Push 자동화
* SSH 기반 원격 서버 배포 자동화
* Commit SHA 기반 이미지 버전 관리

## 📚 What I Learned

* Dockerfile 작성 및 이미지 빌드
* Docker Container 실행 및 테스트
* GitHub Actions Workflow 구성
* GitHub Secrets를 활용한 인증정보 관리
* Docker Hub 이미지 관리
* SSH 기반 원격 서버 배포
* CI/CD Pipeline 구성 및 장애 대응
