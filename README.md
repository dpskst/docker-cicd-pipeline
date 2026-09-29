# Docker CI/CD Pipeline

Docker와 GitHub Actions를 활용하여 애플리케이션의 빌드부터 Docker Hub 이미지 배포, 원격 서버 배포까지 자동화한 CI/CD 프로젝트입니다.

## Project Overview

소스 코드 변경 및 Git Push를 시작으로 GitHub Actions에서 Docker 이미지를 자동으로 빌드하고 테스트한 후 Docker Hub에 Push합니다.

이후 원격 서버(`devops-lab`)에 SSH로 접속하여 최신 Docker 이미지를 Pull하고 기존 컨테이너를 재배포합니다.

## Architecture

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

## Environment

| Category        | Technology     |
| --------------- | -------------- |
| Container       | Docker         |
| CI/CD           | GitHub Actions |
| Image Registry  | Docker Hub     |
| Web Server      | Nginx          |
| Deployment      | SSH            |
| Server          | Rocky Linux    |
| Version Control | Git / GitHub   |

## Project Structure

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

## CI/CD Process

### 1. Source Push

```bash
git push origin main
```

GitHub의 `main` 브랜치에 코드가 Push되면 GitHub Actions Workflow가 실행됩니다.

### 2. Docker Image Build

GitHub Actions에서 `app/Dockerfile`을 기반으로 Docker 이미지를 생성합니다.

```bash
docker build
```

생성된 이미지는 Commit SHA와 `latest` 태그를 사용하여 관리합니다.

```text
dydcjsrjaror/my-devops-app:<commit SHA>
dydcjsrjaror/my-devops-app:latest
```

### 3. Docker Container Test

빌드된 이미지를 임시 컨테이너로 실행한 후 HTTP 요청을 통해 애플리케이션의 정상 동작 여부를 확인합니다.

테스트가 정상적으로 완료된 경우에만 Docker Hub Push 단계가 실행됩니다.

### 4. Docker Hub Push

Container Test가 완료되면 Docker Image를 Docker Hub에 Push합니다.

Commit SHA 태그를 사용하여 특정 Commit에서 생성된 Image를 식별할 수 있도록 구성했습니다.

### Docker Hub

![Docker Hub Image](docs/screenshots/dockerhub-image.png)

> <img width="1839" height="839" alt="image" src="https://github.com/user-attachments/assets/be31e338-70c5-4efb-a657-74a5c76b614b" />


### 5. Remote Server Deployment

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

![Docker Container](docs/screenshots/server-container.png)

> ![Uploading image.png…]()



### 6. GitHub Actions 실행 결과

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

![GitHub Actions](docs/screenshots/github-actions-success.png)

> <img width="790" height="788" alt="image" src="https://github.com/user-attachments/assets/8c0d68e1-8962-4195-9902-808f5f09b94f" />


### 7. Application Deployment Result

배포가 완료된 후 원격 서버의 Nginx 애플리케이션에 HTTP 요청하여 정상적으로 서비스되는 것을 확인했습니다.

### Application

![Application](docs/screenshots/application.png)

> <img width="525" height="393" alt="image" src="https://github.com/user-attachments/assets/73a6ddf7-a76d-49db-a386-c4499358ca24" />


## GitHub Secrets

서버 및 Docker Hub 인증 정보는 GitHub Actions Secrets를 사용하여 관리합니다.

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
SERVER_HOST
SERVER_USER
SERVER_SSH_KEY
```

민감한 인증정보를 Workflow 파일에 직접 작성하지 않고 GitHub Secrets를 통해 전달하도록 구성했습니다.

## Project Goals

* Docker 컨테이너 기반 애플리케이션 배포
* GitHub Actions를 활용한 CI/CD 자동화
* Docker 이미지 빌드 및 Registry Push 자동화
* Container Test를 통한 이미지 정상 동작 검증
* SSH 기반 원격 서버 배포 자동화
* Commit SHA 기반 이미지 버전 관리

## What I Learned

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

## Related Projects

### Project 2 — Kubernetes + ArgoCD GitOps

Docker 기반 애플리케이션을 Kubernetes 환경으로 확장하고 ArgoCD를 활용한 GitOps 배포를 구현했습니다.

### Project 3 — Prometheus + Grafana Monitoring

Kubernetes 환경에 Prometheus와 Grafana를 구성하고 CPU/Memory 모니터링, Alerting 및 Telegram 알림을 구현했습니다.

## DevOps Portfolio Architecture

```text
Project 1
Docker + GitHub Actions CI/CD
        |
        v
Project 2
Kubernetes + ArgoCD GitOps
        |
        v
Project 3
Prometheus + Grafana Monitoring
```

## Key Skills

* Docker
* Git / GitHub
* GitHub Actions
* Docker Hub
* Linux
* Nginx
* SSH
* CI/CD
* Container Deployment
* Kubernetes
* GitOps
* Monitoring

```
